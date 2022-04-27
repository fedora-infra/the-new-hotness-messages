Contributing
============

The New Hotness Messaging Schema welcomes contributions! This document should help you get started.


Contribution Guidelines
-----------------------

When you make a pull request, a Fedora Infrastructure team member will review your
code. Please make sure you follow the guidelines below:

Code style
^^^^^^^^^^

We follow the `PEP8 <https://www.python.org/dev/peps/pep-0008/>`_ style guide for Python.
The test suite includes a test that enforces the required style, so all you need to do is
run the tests to ensure your code follows the style. If the unit test passes, you are
good to go!

We are using `Black <https://github.com/ambv/black>` to automatically format
the source code. It is also checked in CI. The Black webpage contains
instructions to configure your editor to run it on the files you edit.

To automatically format the code run the following in project root. The ``.tox`` folder
will be created when ``tox`` will be run.

.. code-block:: bash

   .tox/format/bin/black .


Unit tests
^^^^^^^^^^

The test suites can be run using `tox <http://tox.readthedocs.io/>`_ by simply running
``tox`` from the repository root. These tests include unit tests, a linter to ensure
Python code style is correct, checks for possible security issues, and checks the
documentation for Sphinx warnings or errors.

All unit tests must pass. All new code should have 100% test coverage.
Any bugfix should be accompanied by one or more unit tests to demonstrate the fix.
If you are unsure how to write unit tests for your code,
we will be happy to help you during the code review process.


CI (Continuous Integration)
---------------------------

The New Hotness Messaging Schema has a CI set up to run on each PR. As a CI of choice 
The New Hotness Messaging Schema is using
`Fedora zuul <https://fedoraproject.org/wiki/Zuul-based-ci>`_ and the configuration
could be found in `.zuul.yaml` in Anitya Messaging Schema root directory.

The CI runs unit tests for all supported python versions, code style test, coverage test,
flake8 test (linter), documentation test build and bandit (to check for any security issue).

The successful run of CI is a requirement for merge of the PR.


Documentation
-------------

Anitya Messaging Schema uses `sphinx <http://www.sphinx-doc.org/>`_ to create its documentation.
New packages, modules, classes, methods, functions, and attributes all should be
documented using `"Google style" <http://www.sphinx-doc.org/en/1.7/ext/example_google.html>`_
docstrings. For historical reasons you may encounter plain reStructuredText-style
docstrings. Please consider converting them and opening a pull request!


Release notes
-------------

To add entries to the release notes, create a file in the ``news`` directory
with the ``source.type`` name format, where the ``source`` part of the filename is:

* ``42`` when the change is described in issue ``42``
* ``PR42`` when the change has been implemented in pull request ``42``, and
  there is no associated issue
* ``username`` for contributors (``author`` extention). It should be the
  username part of their commit's email address.

And where ``type`` is label of the issue or PR that is named ``type.label``. If the issue or PR is missing a label, please ask maintainer to add one.

News type can be one of the following:

* ``feature``: for new features
* ``bug``: for bug fixes
* ``api``: for API changes
* ``dev``: for development-related changes
* ``author``: for contributor names
* ``other``: for other changes
  
For example:

If this PR is solving `bug 714`
the file inside ``news`` should be called ``714.bug``
and the content of the file would be matching the issue title.

The text inside the file will be used as entry text.
A preview of the release notes can be generated with ``towncrier --draft``.


Release Guide
-------------

To do the release you need following python packages installed::

    wheel
    twine
    towncrier

If you are a maintainer and wish to make a release, follow these steps:

1. Change the version in ``hotness_schema.__init__.__version__``. This is used to set the
   version in the documentation project and the setup.py file.

2. Add any missing news fragments to the ``news`` folder.

3. Get authors of commits by ``python get-authors.py``.

.. note::
   This script must be executed in ``news`` folder, because it
   creates files in current working directory.

4. Generate the changelog by running ``towncrier``.

.. note::
    If you added any news fragment in the previous step, you might see ``towncrier``
    complaining about removing them, because they are not committed in git.
    Just ignore this and remove all of them manually; release notes will be generated
    anyway.

5. Remove every remaining news fragment from ``news`` folder.

6. Commit your changes with message *the-new-hotness schema <version>*.

7. Tag a release with ``git tag -s <version>`` with description *the-new-hotness-schema <version>*.

8. Don't forget to ``git push --tags``.

9. Build the Python packages with ``python setup.py sdist bdist_wheel``.

10. Upload the packages with ``twine upload dist/<dists>``.

11. Create new release on `GitHub releases <https://github.com/fedora-infra/the-new-hotness/releases>`_.
