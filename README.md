## Overview

A minimum project template for morden python development.

## Usage

### Prerequisite

Please install [cookiecutter](https://github.com/audreyr/cookiecutter),  [pyenv](https://github.com/pyenv/pyenv) and [direnv](https://direnv.net/).

### Initialize the project

1. Execute the following command to Initialize your project from the template:

   ```bash
   cookiecutter https://github.com/huntzhan/morden-python-project-template.git
   ```

1. Activate the helper snippets:

   ```bash
   cd <project>
   direnv allow
   ```

1. Execute the following command will setup virtualenv, install deps and optionally push to git remote repository for you:

   ```bash
   pyproject-init
   
   # Parameters:
   # -r: Optional. The git remote url. If provided, will initialize git and push to remote.
   # -c: Optional. Path to the pip cache folder.
   #     If provided, will explicitly download the required distributions to
   #     and install distributions from such folder.
   # -p: Optional. Python version. Default to '3.8.7'.
   # -t: Optional. Setup extra tag. Default to 'dev'.
   ```

### Update dependencies

Execute the following command to install or upgrade your dependencies. The default behavior is equivalent to `pip install -e .'[dev]'`.

```bash
pyproject-install-deps

# Parameters:
# -c: Optional. Path to the pip cache folder.
#     If provided, will explicitly download the required distributions to
#     and install distributions from such folder.
# -t: Optional. Setup extra tag. Default to 'dev'.
```

### Make a release

The following command will change the `version` in `setup.cfg`, then commit automatically, and finally push to the default remote as well as creating a new tag in remote for you. Make sure all the changes have been committed.

```bash
pyproject-bump-version <mode>

# Parameters:
# mode: Required. Should be one of the ['major', 'minor', 'patch']
```

### Others

1. Code formatting:

   ```bash
   yapf -i -r <package_name>
   ```

2. Code linting:

   ```bash
   flake8
   ```

3. Execute static analyzer:

   ```bash
   pytype
   ```

4. Build a distribution:

   ```bash
   python setup.py clean --all
   python -m build --wheel
   ```


### References

* https://www.python.org/dev/peps/pep-0517
* https://www.python.org/dev/peps/pep-0518
* https://packaging.python.org/tutorials/packaging-projects/
* https://setuptools.readthedocs.io/en/latest/userguide/declarative_config.html
* https://www.python.org/dev/peps/pep-0508/
* https://www.python.org/dev/peps/pep-0440/#direct-references
* https://setuptools.readthedocs.io/en/latest/userguide/datafiles.html
* https://dev.to/bowmanjd/easily-load-non-python-data-files-from-a-python-package-2e8g

