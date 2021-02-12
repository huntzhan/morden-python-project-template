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
* https://pip.pypa.io/en/stable/reference/pip_install/#vcs-support
