Introduction
============

This is the buildout for PMR2.  This set of configuration is used to
build the complete PMR2 project for development and the staging and
production sites for the `Physiome Model Repository`_.

.. _Physiome Model Repository: https://models.physiomeproject.org


Usage
=====

To ease maintenance, the buildout configuration is split up into
separate files for specific tasks, such as development, staging or
deployment.  Please refer to the specific configuration file for further
configuration options.


Develop
-------

To get the standard development site set up, just clone this repoistory,
call bootstrap and run the buildout as normal; this will just use the
default `buildout.cfg` and the development site will be built.

Example::

    $ git clone ${REPO_URI}/pmr2.buildout.git
    Initialized empty Git repository in /.../pmr2.buildout/.git/
    ...
    Resolving deltas: 100% (...), done.
    $ cd pmr2.buildout
    $ python2.6 bootstrap.py  # Python 2.6 is required
    Downloading http://pypi.python.org/.../setuptools-0.6c11-py2.6.egg
    Creating directory '/.../pmr2.buildout/bin'.
    Creating directory '/.../pmr2.buildout/parts'.
    Creating directory '/.../pmr2.buildout/eggs'.
    Creating directory '/.../pmr2.buildout/develop-eggs'.
    Getting distribution for 'zc.buildout==1.4.4'.
    Got zc.buildout 1.4.4.
    Generated script '/.../pmr2.buildout/bin/buildout'.
    $ bin/buildout

The development buildout uses mr.developer, which will then clone the
repositories with the packages that make up PMR2 into the `src`
directory.  Other dependencies will then be fetched, this will take some
time.

After buildout completes, you may start the development site::

    $ bin/zeoserver-start  # start the zeoserver
    $ bin/paster-testing serve paster_testing.ini  # starts Plone

Or run tests associated with PMR2::

    $ bin/test -s pmr2 -s cellml -s fieldml


Staging or Deployment
---------------------

This is achieved using the staging or development buildout
configuration.  You may specific a configuration file when running
`bin/buildout`.  For example, if this is a deployment instance, this may
be issued::

    $ bin/buildout -c deploy-instance.cfg

This will not use mr.developer to clone the repository, instead the
packaged versions of PMR2 related eggs will be fetched instead.  The
version fetched is specified in `version.cfg`.
