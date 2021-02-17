## Overview

A minimum project template for morden python development.

## Usage

### Initialize project from the template

1. Install [cookiecutter](https://github.com/audreyr/cookiecutter) if you haven't.

2. Run the following command to Initialize your project from this template:

   ```bash
   cookiecutter https://github.com/huntzhan/morden-python-project-template.git
   ```

### Save and load the helper snippets

Copy [the helper snippet](helper_snippet.sh) to `~/.bashrc` (or to the other configuration file according to your setup) and load those bash functions.

### Initialize the pyenv virtualenv

1. Install [pyenv](https://github.com/pyenv/pyenv) if you haven't.

2. The following command will setup virtualenv, install deps and push to git remote repository for you.

3. Command:

   ```bash
   pyproject_init -r git@github.com:<username>/<repo>.git
   
   # Parameters:
   # -r: Required. The git remote url.
   # -c: Optional. Path to the pip cache folder.
   #     If provided, will explicitly download the required distributions to
   #     and install distributions from such folder.
   # -p: Optional. Python version. Default to '3.8.7'.
   # -t: Optional. Setup extra tag. Default to 'dev'.
   ```

### Update dependencies

1. Run after initialization.

2. Run if `install_requires` or `extras_require` has been changed.

3. The following command will install the required distributions for you.

4. Command:

   ```bash
   pyproject_install_deps
   
   # Parameters:
   # -c: Optional. Path to the pip cache folder.
   #     If provided, will explicitly download the required distributions to
   #     and install distributions from such folder.
   # -t: Optional. Setup extra tag. Default to 'dev'.
   ```

### Make a release

1. Run after initialization.

2. Run if you want to make a release. Make sure all the changes have been committed.

3. The following command will change the `version` in `setup.cfg`, commit automatically, push to remote as well as creating a new tag in remote for you.

4. Command:

   ```bash
   pyproject_bump_version <mode>
   
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
   flake8 <package_name>
   ```

3. Build a distribution:

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

