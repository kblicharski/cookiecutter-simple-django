cookiecutter-simple-django
==========================

A cookiecutter_ template for Django.

.. _cookiecutter: https://github.com/audreyr/cookiecutter

Description
-----------

A customized and opinionated fork of ``cookiecutter-simple-django``, originally
inspired by Daniel Greenfeld's ``cookiecutter-django``. This fork is personalized
from the version I created for The Signal Center, intended to be used for
bootstrapping personal projects and hackathon projects.

Usage
------

Let's pretend you want to create a Django project called "redditclone".
Rather than using `startproject` and then editing the results to include your
name, email, and various configuration issues that always get forgotten until
the worst possible moment, get cookiecutter_ to do all the work.

Set up your virtualenv with virtualenvwrapper and install cookiecutter::

    $ mkvirtualenv redditclone
    $ workon redditclone
    $ pip install cookiecutter

Use cookiecutter to clone and configure this repository::

    $ cd <your-workspace>
    $ cookiecutter https://github.com/kblicharski/cookiecutter-simple-django.git

It will prompt you for questions. Answer them::

    project_name [project_name]:
    repo_name [repo_name]:
    author_name [Your name]:
    email [Your email]:
    description [A short description of the project]:
    year [Current year]:
    use_sqlite_as_local_db_engine [no]:

If you used the default of "no" for ``use_sqlite_as_local_db_engine``,
`create a database for the project`_ before continuing.

.. _`create a database for the project`: https://www.digitalocean.com/community/tutorials/how-to-use-mysql-or-mariadb-with-your-django-application-on-ubuntu-14-04

Continue setting up your project::

    $ cd redditclone/
    $ pip install -r requirements/local.txt
    $ python ./manage.py syncdb
    $ python ./manage.py migrate
    $ python ./manage.py runserver

Create a GitHub repo and push your project::

    $ git init
    $ git add .
    $ git commit -m "(build): Initial project config"
    $ git remote add origin git@github.com:kblicharski/example-project-repo.git
    $ git push -u origin master

**Note**: The ``requirements`` files don't define any package versions because it makes
more sense for you to use the latest ones when you set up your
project. After that point though, you really want to take note of the specific
versions installed so that they are not going to get updated without you knowing it.

In order to do this, activate your virtual environment, and
update your requirements files using ``pipdeptree``::

    $ workon redditclone
    $ pipdeptree -f --warn silence | grep '^[a-zA-Z]' > requirements.txt

Structure
---------

The structure used should look quite familiar:

**Requirements**

The ``requirements`` folder contains a requirements file for each environment.

If you need to add a dependency please choose the right file.

**Settings**

The ``settings`` folder contains a settings file for each environment.

The ``testing.py`` module is loaded automatically after ``base.py`` and
``local.py`` every time you run ``python ./manage.py test``.

**Apps**

The ``apps`` folder should contain all your local Django apps in order to keep
the structure of the project clean.

When it's time to ``python ./manage.py startapp <name>``, move the generated
module to ``apps``. If you want to know why this works, just take a look at the line::

    sys.path.insert(0, root('apps'))

in ``settings/base.py``.
