.. _devel_workflow:

=========================
Development Workflow
=========================

Congratulations, we have everything installed, now it's time to start working on
the project. Here are some guidelines and scenarios to help you get started.

.. important::

    - MS Windows users: replace ``./manage.py`` with ``python manage.py``
    - Run ``manage.py`` commands from the `Open-Knesset` directory, with the
      **virtualenv activated**.


Commits and Pull Requests
========================================

Make it easier for you and the maintainers, increasing the chances of a pull
request getting accepted:

- No big Pull Requests. It makes reviewing and ensuring correctness hard. If
  possible, break it to smaller commits/pulls, each related to a specific issue.
- Always work on a specific issue from our `issue tracker`_. Open new issue if
  needed and claim it in the comments.
- Discuss big things in the `Open Knesset Developers group`_.

.. _issue tracker: https://github.com/hasadna/Open-Knesset/issues?state=open
.. _Open Knesset Developers group: https://groups.google.com/forum/#!forum/oknesset-dev

Before Coding
==========================

We need to make sure we're in sync with changes done by others (upstream).

.. important::

    Please do this every time before you start developing:

Update the code and requirements
--------------------------------------

Enter the `Open-Knesset` directory, and run:

.. code-block:: sh

    git pull git@github.com:hasadna/Open-Knesset.git master

.. note::

    Running this command requires having SSH keys registered with github. If you don't have these, or
    if you don't understand what this means and do not want to look it up, you can use:

    git pull https://github.com/hasadna/Open-Knesset.git master

If `requirements.txt` was modified, make sure all of them are installed (no harm
running this command even in case of no changes):

    `pip install -r requirements.txt`

.. note::

    We recommend running the pip command from the parent diretory (the
    virtualenv's root), as it may create an ``src`` directory when pulling
    packages from git repos. in that case::


        `pip install -r Open-Knesset/requirements.txt`



Run migrations and tests
--------------------------------

.. code-block:: sh

    ./manage.py migrate
    ./manage.py test
    # by default the browser tests use firefox - make sure it's installed first
    ./manage.py test --testrunner=knesset.browser_test_runner.Runner

If there are any failures, contact the other developers in the `oknesset-dev`_
group to see if that's something you should worry about.

.. _oknesset-dev: https://groups.google.com/forum/#!forum/oknesset-dev


See :ref:`devel_tips` for a few bash functions that may help.

While Coding
==============

General
---------

- Write tests for everything that you code.
- Keep performance in mind - test the number of db queries your code performs
  using ``./manage.py runserver`` and accessing a page that runs the code you
  changed. See the output of the dev-server before and after your change.


Adding a field to existing model
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We use South_ to manage database schema migrations. The work process is:

- Add the field you want to model `SampleModel` in app `sample_app`
- `python manage.py schemamigration sample_app --auto` this generates a new migration
  under sample_app/migrations. You should review it to make sure it does what
  you've expected.
- `python manage.py migrate` runs the migration against the database.
- Don't forget to **git add** and **commit** the migration file.

.. _South: http://south.aeracode.org/

After you code
~~~~~~~~~~~~~~~~

- ``./manage.py test`` # make sure you didn't break anything
- ``./manage.py test --testrunner=knesset.browser_test_runner.Runner`` # run the browser tests (see :ref:`browser_tests`)
- ``git status`` # to see what changes you made
- ``git diff filename`` # to see what changed in a specific file
- ``git add filename`` # for each file you changed/added.
- ``git commit -m "commit message"`` 
  
  Please write a sensible commit message, and include "fix#: [number]" of the issue number you're working on (if any).
- ``git push`` # push changes to git repo
- go to github.com and send a "pull request" so your code will be reviewed and
  pulled into the main branch, make sure the base repo is
  **hasadna/Open-Knesset**.
