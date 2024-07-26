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
* Generate network-diagram from dot like text (basic feature).
* Multilingualization for node-label (utf-8 only).

You can get some examples and generated images on 
`blockdiag.com <http://blockdiag.com/en/nwdiag/nwdiag-examples.html>`_ .

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
`blockdiag.com`_ .

simple.diag
------------

``simple.diag`` is simply define nodes and transitions by dot-like text format::

    nwdiag {
      network dmz {
          address = "210.x.x.x/24"

          web01 [address = "210.x.x.1"];
          web02 [address = "210.x.x.2"];
      }
      network internal {
          address = "172.x.x.x/24";

          web01 [address = "172.x.x.1"];
          web02 [address = "172.x.x.2"];
          db01;
          db02;
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
