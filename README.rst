Structure of the Repository
---------------------------

It's Important.
:::::::::::::::

Just as Code Style, API Design, and Automation are essential for a
healthy development cycle, Repository structure is a crucial part of
your project's architecture

    Dress for the job you want, not the job you have.

Of course, first impressions aren't everything. You and your colleagues
will spend countless hours working with this repository, eventually
becoming intimately familiar with every nook and cranny. The layout of
it is important.

This repository is `available on
GitHub <https://github.com/triglex/py-boilerplate>`__.

::

    .
    |____docs
    | |____index.rst
    | |____conf.py
    | |____Makefile
    |____tests
    | |____test_basic.py
    | |______init__.py
    | |____test_advanced.py
    | |____context.py
    |____sample
    | |____helpers.py
    | |____core.py
    | |______init__.py
    |____.gitignore
    |____requirements.txt
    |____README.rst
    |____setup.py
    |____Makefile


Let's get into some specifics.




The Source
:::::::::::::::::

.. csv-table::
   :widths: 20, 40

   "Location", "``./sample/`` or ``./sample.py``"
   "Purpose", "The code of interest"


Your project is the core focus of the repository. It should not
be tucked away:

::

    ./sample/

If your project consists of only a single file, you can place it directly
in the root of your repository:

::

    ./sample.py

Your project does not belong in an ambiguous src or python subdirectory.

Setup.py
::::::::

.. csv-table::
   :widths: 20, 40

   "Location", "``./setup.py``"
   "Purpose", "Package and distribution management."

Requirements File
:::::::::::::::::

.. csv-table::
   :widths: 20, 40

   "Location", "``./requirements.txt``"
   "Purpose", "Development dependencies."


A `pip requirements
file <https://pip.pypa.io/en/stable/user_guide/#requirements-files>`__
should be placed at the root of the repository. It should specify the
dependencies required to contribute to the project: testing, building,
and generating documentation.

If your project has no development dependencies, or you prefer
development environment setup via ``setup.py``, this file may be
unnecessary.

Documentation
:::::::::::::


.. csv-table::
   :widths: 20, 40

   "Location", "``./docs/``"
   "Purpose", "Package reference documentation."

There is little reason for this to exist elsewhere.

Test Suite
::::::::::

.. csv-table::
   :widths: 20, 40

   "Location", "``./test_sample.py`` or ``./tests``"
   "Purpose", "Package integration and unit tests."

Starting out, a small test suite will often exist in a single file:

::

    ./test_sample.py

Once a test suite grows, you can move your tests to a directory, like
so:

::

    tests/test_basic.py
    tests/test_advanced.py

Obviously, these test modules must import your packaged module to test
it. You can do this a few ways:

-  Expect the package to be installed in site-packages.
-  Use a simple (but *explicit*) path modification to resolve the
   package properly.

I highly recommend the latter. Requiring a developer to run
``setup.py develop`` to test an actively changing
codebase also requires them to have an isolated environment setup for
each instance of the codebase.

To give the individual tests import context, create a tests/context.py
file:

::

    import os
    import sys
    sys.path.insert(0, os.path.abspath(os.path.join(os.path.dirname(__file__), '..')))

    import sample

Then, within the individual test modules, import the module like so:

::

    from .context import sample

This will always work as expected, regardless of installation method.

Some people will assert that you should distribute your tests within
your module itself -- I disagree. It often increases complexity for your
users; many test suites often require additional dependencies and
runtime contexts.

Makefile
::::::::


.. csv-table::
   :widths: 20, 40

   "Location", "``./Makefile``"
   "Purpose", "Generic management tasks."


**Sample Makefile:**

::

    init:
        pip install -r requirements.txt

    test:
        py.test tests

    .PHONY: init test

Other generic management scripts (e.g. ``manage.py``
or ``fabfile.py``) belong at the root of the repository as well.

TODO:
::::::::

Virtualenv https://virtualenv.pypa.io/
Virtual environments allow you to work on or deploy multiple projects in 
isolation from one another; essential for your sanity.

Virtualenvwrapper https://virtualenvwrapper.readthedocs.org 
Provides some nice convenience features that make it easier to
spin up, use, and work with virtual environments. Not essential,
but nice.

Pytest http://pytest.org is a unit testing framework much like Nose 
but with some extra features

Mock http://www.voidspace.org.uk/python/mock Lightweight mock objects 
and patching functionality make it easier to isolate and test your code.

Pylint http://www.pylint.org The linter for Python; helps you detect 
bad style, various coding errors, and opportunities for refactoring. 
Pair with CI/pre-commit hook.