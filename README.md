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
