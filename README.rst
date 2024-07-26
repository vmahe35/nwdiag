`nwdiag` generate network-diagram image file from spec-text file.

.. image:: https://drone.io/bitbucket.org/blockdiag/nwdiag/status.png
   :target: https://drone.io/bitbucket.org/blockdiag/nwdiag
   :alt: drone.io CI build status

.. image:: https://pypip.in/v/nwdiag/badge.png
   :target: https://pypi.python.org/pypi/nwdiag/
   :alt: Latest PyPI version

.. image:: https://pypip.in/d/nwdiag/badge.png
   :target: https://pypi.python.org/pypi/nwdiag/
   :alt: Number of PyPI downloads


Features
========
* This project is a fork of the `nwdiag <https://github.com/blockdiag/nwdiag>`_ project that adds the following features:

   * Integrate the 7 commits from `this Pull Request <https://github.com/blockdiag/nwdiag/pull/15>`_ in order to be able compute routes between 2 network nodes.
   * Be able to define a default network trunk diameter.

You can get some examples and generated images in folder ``./examples/nwdiag``.

Setup of the dev env
====================

Use Makefile target ``env``::

   $ make env

Build the Python package locally
================================

Use Makefile target ``build``::

   $ make build


spec-text setting sample
========================

Few examples are available.
You can get more examples at
`blockdiag.com <http://blockdiag.com/en/blockdiag/examples.html>`_ .

simple.diag
------------

``simple_with_custom_trunk_diameter`` shows an example where you can customize the diameter of the network trunk::

   nwdiag {
      node_width = 128;  // default value is 128
      node_height = 100;  // default value is 40
      network_trunk_diameter = 47  // default network trunk diameter for the whole diagram

      network dmz {
         trunk_diameter = 12  // you can also override the trunk diameter with a specific value for a given network
         address = "210.x.x.x/24"

         web01 [address = "210.x.x.1"];
         web02 [address = "210.x.x.2"];
      }
      network internal {
         address = "172.x.x.x/24";
         color = "red";
         //color = "palegreen";
         style = dotted;

         web01 [address = "172.x.x.1", shape = "ellipse", height = 150, style = dotted];
         web02 [address = "172.x.x.2"];
         beginpoint [shape = beginpoint];
         endpoint [shape = endpoint];

         db01;
         db02;
      }

      // Test a group because Network derivates from NodeGroup
      group {
         db01;
         db02;
         height = 120
         //thick
         label = "third group";
         color = "#FF0000";

         // Set group shape to 'line group' (default is box)
         shape = line;

         // Set line style (effects to 'line group' only)
         style = dashed;
      }
   }

diag_with_route.diag
--------------------

``diag_with_route.diag`` is an example that draws some routes::

   diagram {
      inet [shape = cloud];
      inet -- router;

      network internal {
         address = "192.168.0.0/24";

         router [address = "192.168.0.1"];
         client01 [address = "192.168.0.101"];
         client02 [address = "192.168.0.102"];
      }

      route {
         client02 -> router -> inet;
         inet -> router -> client01 [path = "rb", color = "blue"];
      }
   }


Launch in Docker
================

Use Makefile target ``docker-build-slim``::

   $ make docker-build-slim

Once the Docker image is built, you can also launch a specific ``nwdiag`` command::

   $ docker run --rm -v $PWD/examples/nwdiag:/app -t nwdiag-slim:latest nwdiag simple.diag
   $ ls -al $PWD/examples/nwdiag

To output an SVG file, use option ``-T svg``::

   $ docker run --rm -v $PWD/examples/nwdiag:/app -t nwdiag-slim:latest nwdiag -T svg simple.diag
   $ ls -al $PWD/examples/nwdiag

To enter inside the container::

   $ make docker-build
   $ docker run --rm -v $PWD/examples/nwdiag:/app -t nwdiag:latest bash

Usage in shell
==============

Execute ``nwdiag`` command::

   $ nwdiag simple.diag
   $ ls simple.png
   simple.png

Known issues
============

* If you change the default ``network_trunk_diameter`` value, the routes layout is not correct.
* If you override ``trunk_diameter`` for a specific network, the connectors to this network are still computed with the default ``network_trunk_diameter`` value

Requirements
============
* Python 3.7 or later
* blockdiag 3.0.0 or later
* funcparserlib 0.3.6 or later
* reportlab (optional)
* wand and imagemagick (optional)
* setuptools


License
=======
Apache License 2.0
