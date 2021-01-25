Overview
========

The **Common Build Specification** (CBS) is a mechanism for describing how to build and distribute a software package. A CBS file provides a declarative description of a package that is intended to be consumed by build tools. By providing a detailed, flexible, and language-agnostic description using widely supported JSON grammar, CBS aims to make it easy to portably build software, regardless of build systems used.

CBS files are intended to be produced by the package provider, and included in the package's sources.

CBS is *not* yet another build tool. While CBS includes support to build and distribute arbitrary artifacts (typically used for code generation), CBS does not provide flexible mechanisms for toolchain configuration and invocation. It is left to build tools to know how to do this, and/or to provide additional, tool-specific utilities for this purpose.

Advantages of CBS
'''''''''''''''''

CBS takes after `CPS`_. It is nonetheless worth mentioning a few of its advantages.

**Tool Agnosticism**

  CBS has a strong focus on using high level semantic abstractions that reduce coupling to a particular build tool or compiler. This helps build tools make intelligent decisions about such issues as what compile flags to use and how to specify link libraries. The use of JSON_ as a grammar lowers the entry barrier for tools to consume CBS.

**Language and Platform Agnosticism**

  CBS is designed to be cross platform, with support for both Windows and POSIX-like operating systems, and extensible to multiple languages. While the core specification focuses on C and C++, provisions are made for Fortran and Java, as well as allowances for tools to add additional support. Contributions to improve the core support for other languages are also welcomed and encouraged.

**Flexible Configurations**

  CBS allows a package to provide targets in multiple configurations no multiple platforms, with no restriction on the naming of configurations.

**Relocatable Installs**

  CBS allows and encourages packages to specify distribution locations relative to the package's install prefix, which allows the user to choose where to install a pre-built package with no need to modify the installed files, or even to move an already-installed package to a different location.

.. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. ..

.. _CPS: https://github.com/mwoehlke/cps

.. _JSON: http://www.json.org/

.. kate: hl reStructuredText
