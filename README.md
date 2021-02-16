# A Minimum Morden Project Template For Python Development

Install [cookiecutter](https://github.com/audreyr/cookiecutter) if you haven't.

```bash
cookiecutter https://github.com/huntzhan/morden-python-project-template.git
```

Enter the folder you've just created and initialize the Python virtualenv:

```bash
pyenv virtualenv 3.8.7 $(basename $(pwd))
pyenv local $(basename $(pwd))
export PIP_INDEX_URL=https://mirrors.cloud.tencent.com/pypi/simple/
pip install -U pip
```

Install dependencies:

```bash
pip install -e .'[dev]'

# Alternatively, cache the packages explicitly.
export MY_PIP_DOWNLOADED_FOLDER="/Users/huntzhan/.cache/pip-downloaded"
pip download .'[dev]' --dest "$MY_PIP_DOWNLOADED_FOLDER"
pip install -e .'[dev]' --no-index --find-links="file://${MY_PIP_DOWNLOADED_FOLDER}"
```

Or you can execute the following function:

```bash
function pyproject_init {
    # Parse arguments.
    local arg git_remote pip_cache_folder python_version pip_install_tag

    while getopts 'r:c:p:t' arg ; do
        case "$arg" in
            r) git_remote=${OPTARG} ;;
            c) pip_cache_folder=${OPTARG} ;;
            p) python_version=${OPTARG} ;;
            t) pip_install_tag=${OPTARG} ;;
            *) return 1
        esac
    done

    echo "git_remote=${git_remote}"
    echo "pip_cache_folder=${pip_cache_folder}"
    echo "python_version=${python_version}"
    echo "pip_install_tag=${pip_install_tag}"

    if [ -z "$git_remote" ] ; then
        echo "-r is required, abort"
        return 1
    fi

    # Make sure cwd is not git initialized.
    if git rev-parse --git-dir > /dev/null 2>&1 ; then
        echo "Git initialized, abort."
        return 1
    fi

    # Initialize pyenv virtualenv.
    folder_name=$(basename $(pwd))

    if pyenv virtualenv-prefix "$folder_name" > /dev/null 2>&1 ; then
        echo "pyenv virtualenv name=${folder_name} exists, abort"
        return 1
    fi

    if [ -z "$python_version" ] ; then
        python_version=3.8.7
    fi
    echo "pyenv virtualenv name=${folder_name}, python_version=${python_version}"

    pyenv virtualenv 3.8.7 "$folder_name"
    pyenv local "$folder_name"
    if [ "$?" -ne 0 ] ; then
        echo "Failed to setup pyenv virtualenv name=${folder_name}, abort"
        return 1
    fi

    # Install dependencies.
    if [ -z "$pip_install_tag" ] ; then
        pip_install_tag=dev
    fi
    echo "pip_install_tag=${pip_install_tag}"

    if [ -n "$pip_cache_folder" ] ; then
        if [ ! -d "$pip_cache_folder" ] ; then
            echo "pip_cache_folder=${pip_cache_folder} is not a folder, abort"
            return 1
        fi
        pip download "$(pwd)[${pip_install_tag}]" --dest "$pip_cache_folder"
        pip install -e "$(pwd)[${pip_install_tag}]" --no-index --find-links="file://${pip_cache_folder}"
    else
        pip install -e "$(pwd)[${pip_install_tag}]"
    fi

    if [ "$?" -ne 0 ] ; then
        echo "Failed install dependencies, abort"
        return 1
    fi

    # Initialize git.
    git init
    git add --all
    git commit -am 'init'
    git branch -M master
    git remote add origin "$git_remote"
    git push -u origin master
}

pyproject_init
```

Code formatting:

```bash
yapf -i -r <package_name>
```

Code linting:

```bash
flake8 <package_name>
```

Versioning:

```bash
PYTHON_SCRIPT=$(
cat << 'EOF'

import sys
import re
import os.path

assert len(sys.argv) == 2
_, version_mode = sys.argv

version_mode = version_mode.lower()
if version_mode not in ['major', 'minor', 'patch']:
    print('invalid version_mode, abort')
    exit(1)

if not os.path.exists('setup.cfg'):
    print('setup.cfg not found, abort')
    exit(1)

with open('setup.cfg') as fin:
    text = fin.read()

pattern = r'^version = (\d+)\.(\d+)\.(\d+)$'
matches = re.findall(pattern, text, re.MULTILINE)
if len(matches) != 1:
    print('Failed to match the current version, abort')
    exit(1)

major, minor, patch = matches[0]
if version_mode == 'major':
    major = int(major) + 1
    minor, patch = 0, 0
elif version_mode == 'minor':
    minor = int(minor) + 1
    patch = 0
else:
    assert version_mode == 'patch'
    patch = int(patch) + 1
new_version = f'{major}.{minor}.{patch}'

text = re.sub(pattern, f'version = {new_version}', text, flags=re.MULTILINE)
with open('setup.cfg', 'w') as fout:
    fout.write(text)

print(new_version)
exit(0)

EOF
)

function pyproject_bump_version {
    if [ -n "$(git status --porcelain 2>&1)" ] ; then
        echo 'git status not clean, abort'
        return 1
    fi

    new_version=$(python -c "$PYTHON_SCRIPT" $1)
    if [ ! "$?" -eq 0 ] ; then
        echo 'Failed to bump version, abort'
        return 1
    fi

    git commit -am "Bump version to ${new_version}"
    git push
    git tag "$new_version"
    git push origin "$new_version"
}

pyproject_bump_version patch
pyproject_bump_version minor
pyproject_bump_version major
```

Distribution:

```bash
python setup.py clean --all
python -m build --wheel
```

For more details:

* https://www.python.org/dev/peps/pep-0517
* https://www.python.org/dev/peps/pep-0518
* https://packaging.python.org/tutorials/packaging-projects/
* https://setuptools.readthedocs.io/en/latest/userguide/declarative_config.html
* https://www.python.org/dev/peps/pep-0508/
* https://www.python.org/dev/peps/pep-0440/#direct-references
* https://setuptools.readthedocs.io/en/latest/userguide/datafiles.html
* https://dev.to/bowmanjd/easily-load-non-python-data-files-from-a-python-package-2e8g
