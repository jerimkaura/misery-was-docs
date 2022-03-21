=============
Project Setup
=============

This documents the project setup process & tools.

Dependency Management
---------------------

Create a virtual environment (ideally with Python 3.9): `python3 -m venv venv` & activate.

Dependency management is done using `poetry <https://python-poetry.org/docs/>`_:
    Poetry is a tool for dependency management and packaging in Python. It allows you to declare the libraries your project depends on and it will manage (install/update) them for you.

Basic Usage
~~~~~~~~~~~

`<https://python-poetry.org/docs/basic-usage/>`_

Create a new project: `poetry new poetry-demo`, creates a new directory with `pyproject.toml` & `README.rst` files and source & test folders.

Poetry tracks dependencies using a `poetry.lock` file (like `npm`'s `package.json` & `yarn`'s `yarn.lock` files)

To install runtime dependencies: `poetry add Django`

To install development dependencies: `poetry add black --dev`

To install dependencies in a `poetry.lock` file, simply run `poetry install`.

To update dependencies, run `poetry update` & to uninstall a dependency, run `poetry remove pendulum`.

Django
------

Use the `.env.sample` file to create your `.env` file. These are loaded into our environment using the `python-dotenv` library in `misery_was_backend.config.settings`.

Run `scripts/setup-postgres.sh` to setup Postgres. You can change the password & user if needed.

The `django-cors-headers` library is used for `CORS <https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS>`_ to configure a server to indicate any origins (domain, scheme, or port) other than its own from which a browser should permit loading resources. Useful if we have a separate frontend.

Git Submodules
--------------

The project is split into two repositories: the `source code repository <https://github.com/jerimkaura/misery-was-backend>`_ and the `documentation repository <https://github.com/jerimkaura/misery-was-docs>`_.
See the `.gitmodules` file in the source repository.

These are linked as `Git Submodules <https://git-scm.com/book/en/v2/Git-Tools-Submodules>`_:
    Submodules allow you to keep a Git repository as a subdirectory of another Git repository. This lets you clone another repository into your project and keep your commits separate.

To add a submodule, run: `git submodule add git@github.com:jerimkaura/misery-was-docs.git`

If a submodule already exists, run `git submodule init` and `git submodule update`.

Branch Protection Rules
-----------------------

`Docs on branch protection rules <https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/managing-a-branch-protection-rule>`_

The following rules are in place:

1. Require a pull request before merging (at least 1 approval)
2. Require status checks to pass before merging (branches must be up to date before merging)
3. Require conversation resolution before merging
4. Require linear history (`Rebase & merge` or `Squash & merge`)

Linear history: `A Git Workflow for Agile Teams <http://reinh.com/blog/2009/03/02/a-git-workflow-for-agile-teams.html>`_ & `Git team workflows: merge or rebase? <https://www.atlassian.com/git/articles/git-team-workflows-merge-or-rebase>`_.


Code Formatting and Automated Testing
-------------------------------------

The project uses `Flake8`, `Black`` , `Isort`` and `Mypy`` for formating code into 
conventional starndards. The tools are setup using `pre-commit hook <https://pre-commit.com/>`_ to run before commits 
done.

To setup the hooks, run:

- `poetry add pre-commit`
- create a file named `.pre-commit-config.yaml`
- run `pre-commit install` to set up the git hook scripts
- `poetry run pre-commit` to test formatting and Testing

In addition, the github actions perfoms Testing of the code upon every push or pull.

