Introduction:
-------------

The buildout has been broken up like it is supposed to for ease of
development and deployment.

Default buildout action is testing, which is for development.


Startup:
--------
bin/zeoserver-testing start
bin/paster serve paster_testing.ini


Other random notes:
-------------------

libxml2/libxslt MUST be installed.

VirtualSiteRoot can be set like so:

    http://localhost:8380/VirtualHostBase/http/models.cellml.org:80/cellml/models/VirtualHostRoot/
    http://localhost:8380/VirtualHostBase/https/models.cellml.org:443/cellml/models/VirtualHostRoot/
