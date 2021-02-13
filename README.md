# A Minimum Morden Project Template For Python Development

Install [cookiecutter](https://github.com/audreyr/cookiecutter) if you haven't.

```bash
cookiecutter https://github.com/parseforce/morden-python-project-template.git
```

Then, enter the folder you've just created.

```bash
pyenv virtualenv 3.8.7 $(basename $(pwd))
pyenv local $(basename $(pwd))
export PIP_INDEX_URL=https://mirrors.cloud.tencent.com/pypi/simple/
pip install -U pip
pip install -e .'[dev]'
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
python -m build
```

For more details:

* https://www.python.org/dev/peps/pep-0517
* https://www.python.org/dev/peps/pep-0518
* https://packaging.python.org/tutorials/packaging-projects/
* https://setuptools.readthedocs.io/en/latest/userguide/declarative_config.html
* https://www.python.org/dev/peps/pep-0508/
* https://www.python.org/dev/peps/pep-0440/#direct-references
