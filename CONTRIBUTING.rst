How to contribute
=================

Thank you for thinking of contributing!


Reporting issues
----------------

Include the following information in your post:

-   Describe what you expected to happen.
-   If possible, include a `minimal reproducible example`_ to help us
    identify the issue. This also helps check that the issue is not with
    your own code.
-   Describe what actually happened. Include the full traceback if there
    was an exception.

.. _minimal reproducible example: https://stackoverflow.com/help/minimal-reproducible-example


Submitting patches
------------------

If there is not an open issue for what you want to submit, prefer
opening one for discussion before working on a PR. You can work on any
issue that doesn't have an open PR linked to it or a maintainer assigned
to it. These show up in the sidebar. No need to ask if you can work on
an issue that interests you.

Include the following in your patch:

-   Include tests if your patch adds or changes code. Make sure the test
    fails without your patch.
-   Update any relevant docs pages and docstrings. Docs pages and
    docstrings should be wrapped at 72 characters.
-   Add an entry in ``CHANGES.rst``. Use the same style as other
    entries.


Setting up
----------

-   Download and install `Git`_.

-   Make sure you have a `GitHub account`_.
-   Configure git with your `username`_ and `email`_.

    .. code-block:: text

        > git config --global user.name "<your full name>"
        > git config --global user.email <your email address>

-   Clone the repository:

    .. code-block:: text

        > git clone https://github.com/cr3/taramx
        > cd taramx

-   Install dependencies:

    .. code-block:: text

        > make setup

.. _git: https://git-scm.com/download
.. _username: https://docs.github.com/en/github/using-git/setting-your-username-in-git
.. _email: https://docs.github.com/en/github/setting-up-and-managing-your-github-user-account/setting-your-commit-email-address
.. _GitHub account: https://github.com/join


Starting to code
----------------

-   Create a branch to identify the issue you would like to work on.

    .. code-block:: text

        > git fetch origin
        > git checkout -b <your branch name> origin/main

-   Using your favorite editor, make your changes,
    `committing as you go`_.
-   Push your commits to your branch on GitHub and
    `create a pull request`_. Link to the issue being addressed with
    ``fixes #123`` in the pull request.

    .. code-block:: text

        > git push origin <your branch name>

.. _committing as you go: https://afraid-to-commit.readthedocs.io/en/latest/git/commandlinegit.html#commit-your-changes
.. _create a pull request: https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request


Checking the syntax
-------------------

Check syntax of docker compose files:

.. code-block:: text

    > make check
