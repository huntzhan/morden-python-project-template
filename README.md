# A Minimum Morden Project Template For Python Development

Install [cookiecutter](https://github.com/audreyr/cookiecutter) if you haven't.

```bash
cookiecutter https://github.com/huntzhan/morden-python-project-template.git
```

Then, enter the folder you've just created.

```bash
pip install -U pip poetry
poetry install
```

Code formatting:

```bash
yapf -i -r <package_name>
```

Code linting:

```bash
flake8 <package_name>
```

## Virtualenv setup for the project

```bash
pyenv virtualenv 3.8.5 $(basename $(pwd))
pyenv local $(basename $(pwd))
PIP_INDEX_URL=https://mirrors.aliyun.com/pypi/simple/ pip install -U pip poetry
poetry install
```

