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

Optional Dependencies
~~~~~~~~~~~~~~~~~~~~~

Some of these can be made optional - just remove the unneeded packages
from the ``eggs`` and the ``zcml`` sections in the ``buildout.cfg``
file.

If CellML API support is included

* cmake (2.8 and above)
* libgsl0-dev
* omniidl4 (or similar; omniidl on Debian 7.5)

If you wish to flip on the `enable-telicems` flag for this, you will
also need:

* bison-2.5 (bison-3.0 will *NOT* work).
* flex

If virtuoso integration (for the semantic reasoning, i.e. support for
the RICORDO related packages) is to be included:

* unixodbc-dev

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

After buildout completes, you may start the development site::

    $ bin/zeoserver-testing start  # start the zeoserver in background
    $ bin/instance-testing fg      # run test server in the foreground

Or run tests associated with PMR2::

    $ bin/test -s pmr2 -s cellml -s fieldml


Staging or Deployment
---------------------

This is achieved using the staging or development buildout
configuration.  You may specific a configuration file when running
``bin/buildout``.  For example, if this is a deployment instance, this
may be issued::

    $ bin/buildout -c deploy-instance.cfg

This will not use mr.developer to clone the repository, instead the
packaged versions of PMR2 related eggs will be fetched instead.  The
version fetched is specified in ``version.cfg``.


Quick start on using the testing instance
-----------------------------------------

Assuming the `Develop` section was followed, the test server should have
successfully been installed and running in the foreground.  Point your
browser to the location (should be http://localhost:8280/).  Follow the
instructions on that page to create the default Plone site (if default
credentials are used, it should be admin/admin for login/password).

Once the default Plone site is added, select `Site Setup` using the user
drop-down menu on the upper-right corner of the site, then `Add-ons`.

On that page you will see a list of products available for installation.
Select `Physiome Model Repository 2`, then select `Install`.  You may
pick any other relevant add-ons to install from that screen.

Once `Physiome Model Repository 2` is installed, under the `Site Setup`
menu there should be a new menu item called `PMR2 Core Configuration`,
under the `Add-on Product Configuration`.  This should also be shown
under the Site Setup sidebar.

You may review the settings there.  The default directories are fine,
but for any serious work please change it to an alternative location as
the parts directory can become overwritten by the installer/buildout
scripts.  Once you are happy, select `Apply and Create Objects`.  The
Workspace and Exposure containers will be created at their respective
locations.

Go back to the home page.  You should see two new tabs, 'workspace' and
'exposure' respectively. You can now go into 'workspace' and select 'Add
new...' then 'PMR2 Workspace'.  From that screen you can create a new
workspace.

Once you create a new workspace, you can view the sharing tab.  If you
have other users created you can grant them permissions to push to the
workspace. As admin you should be able to push changes in without issues
from an existing Mercurial repository, or clone from new workspaces from
which you can ``git clone`` with (or ``hg clone``, if mercurial is
enabled).
