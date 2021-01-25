.. much more subtle headings between TOC headers
.. raw:: html

  <style>
    div.body h2 {
      background: inherit;
      border: none;
      font-size: 110%;
    }
  </style>

Introduction
============

This document describes the schema for Common Build Specification files. A Common Build Specification file (hereafter "CBS") is a mechanism for describing how users may build and distribute a package. "User" here refers to a build tool, not an end user. CBS deals with building and installing software.

CBS is largely based on the Common Package Specification (or `CPS`_).

Contents
========

General Information
'''''''''''''''''''

.. toctree::
   :maxdepth: 2

   overview
   development

Core Specification
''''''''''''''''''

.. toctree::
   :maxdepth: 2

   schema
   features
   components
   configurations

Appendices
''''''''''

.. toctree::
   :maxdepth: 2

   schema-supplement
   recommendations

* :ref:`search`

.. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. ..

.. _CPS: https://github.com/mwoehlke/cps

.. kate: hl reStructuredText
