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


Prerequisite 
------------

Before this buildout will build correctly, a number of packages and
dependencies must be provided.  If this is to be done on a Debian/Ubuntu
based system, these packages must be installed:

* build-essential
* libxml2-dev
* libxslt1-dev
* python2.6-dev or python2.7-dev (depending on version of system python)

Debian 8 specific
~~~~~~~~~~~~~~~~~

* zlib1g-dev (for lxml)
* libjpeg-dev (for Pillow)

Optional Dependencies
~~~~~~~~~~~~~~~~~~~~~

Some of these can be made optional - just remove the unneeded packages
from the ``eggs`` and the ``zcml`` sections in the ``buildout.cfg``
file.

If CellML API support is included

* gcc-4, as gcc-5+ results in compilation error.
* cmake-2.8 and above, but 3.7.2 and above may fail to generate the
  header files and results in compilation error
* libgsl0-dev
* omniidl4 (or packages that offer that; omniidl on Debian 7.5; some
  distributions may require the whole omniORB package).

If the `enable-telicems` flag is to be enabled, the following will also
need to be present:

* bison-2.5, <3.0; >=bison-3.0 was reported to *NOT* work.
* flex

If virtuoso integration (for the semantic reasoning, i.e. support for
the RICORDO related packages) is to be included:

* unixodbc-dev
* may need to compile virtuoso for the odbc bridge shipped with Ubuntu
  has reportedly cause segfaults.

Develop
-------

To get the standard development site set up, just clone this repoistory,
call bootstrap, with the specific version 1.5.2 as buildout 2 no longer
isolate the system python in the way that it used to work.

After that, run the buildout as normal; this will just use the default
``buildout.cfg`` and the development site will be built.

Example::

    $ git clone ${REPO_URI}/pmr2.buildout.git
    Initialized empty Git repository in /.../pmr2.buildout/.git/
    ...
    Resolving deltas: 100% (...), done.
    $ cd pmr2.buildout
    $ python bootstrap.py
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

After buildout completes, the development site may be started like so::

    $ bin/zeoserver-testing start  # start the zeoserver in background
    $ bin/instance-testing fg      # run test server in the foreground

Or run tests associated with PMR2::

    $ bin/test -s pmr2 -s cellml -s fieldml


Staging or Deployment
---------------------

This is achieved using the staging or development buildout
configuration.  A specific a configuration file may be specified for
``bin/buildout``.  For example, if this is a deployment instance, this
may be issued::

    $ bin/buildout -c deploy-instance.cfg

This will not use mr.developer to clone the repository, instead the
packaged versions of PMR2 related eggs will be fetched instead.  The
version fetched is specified in ``version.cfg``.


Quick start on using the testing instance
-----------------------------------------

Assuming the `Develop` section was followed, the test server should have
successfully been installed and running in the foreground.  Point a
browser to the location (should be http://localhost:8280/).  Follow the
instructions on that page to create the default Plone site (if default
credentials are used, it should be admin/admin for login/password).

Once the default Plone site is added, select `Site Setup` using the user
drop-down menu on the upper-right corner of the site, then `Add-ons`.

On that page a list of products available for installation will be
presented.  Select `Physiome Model Repository 2`, then select `Install`.
Any other relevant add-ons to install from that screen may be selected.

Once `Physiome Model Repository 2` is installed, under the `Site Setup`
menu there should be a new menu item called `PMR2 Core Configuration`,
under the `Add-on Product Configuration`.  This should also be shown
under the Site Setup sidebar.

The settings present should be reviewed; while the default directories
are usable, production users should change it to an alternative location
outside of Zope to avoid potential issues where the Zope/Plone installer
and/or buildout prune or overwrite the contents within the parts
directory.  When the options are configured select `Apply and Create
Objects` to create the Workspace and Exposure containers along with the
relevant filesystem locations.

Retruning back the the home page, two new tabs should become visible,
which are 'workspace' and 'exposure' respectively.   Select 'workspace'
from the main content tabs, then using the 'Add new...' menu, select
'PMR2 Workspace' to create a new workspace.

User specific permissions to the newly created workspace can be granted
with the sharing tab.  If other users requires push permission that can
be granted.  As the owner has push permissions by default, the contents
can be accessed using the respective DVCS that is backed by the current
workspace.
