Package Schema
==============

Objects
'''''''

:object:`Package`\ :hidden:`(Object)`
-------------------------------------

The root of a CBS document is a |package| object. A |package| object describes a single package.

:object:`Platform`\ :hidden:`(Object)`
--------------------------------------

A |platform| describes a platform in which a package's components may be built and distributed.

:object:`Requirement`\ :hidden:`(Object)`
-----------------------------------------

A |requirement| describes the specifics of a package dependency.

:object:`Component`\ :hidden:`(Object)`
---------------------------------------

A |component| is a consumable part of a package. Typical components include libraries and executables.

:object:`Configuration`\ :hidden:`(Object)`
-------------------------------------------

A |configuration| holds attributes that are specific to a particular configuration of a |component|.

:object:`Assembly`\ :hidden:`(Object)`
--------------------------------------

An |assembly| holds attributes that are specific to the build of a |component|.

:object:`Distribution`\ :hidden:`(Object)`
------------------------------------------

A |distribution| holds attributes that are specific to the distribution of a |component| for downstream consumption.

Attributes
''''''''''

An optional attribute may have the value |null|. This shall be equivalent to omitting the attribute.

Attribute names are case insensitive, although it is recommended that ``.cbs`` files use the capitalization as shown.

Attribute values may contain substitution expressions. Some common substitution expressions that apply to all attributes are:

- ``$(location component-name)`` to be substituted by the :attribute:`Location` of the associated artifacts of a component;
- ``$(link-location component-name)`` to be substituted by the :attribute:`Link-Location` of the associated artifacts of a component;
- ``$(shell cmd args...)`` to be substituted by the output of arbitrary command run in the default shell.

:attribute:`Artifacts`
----------------------

:Type: |string-list|
:Applies To: |assembly|, |configuration|
:Required: Depends

Specifies the artifacts associated with the component.

This attribute is only required for |component|\ s that are of :string:`"generic"` :attribute:`Type`.

:attribute:`Assembly`
---------------------

:Type: |assembly|
:Applies To: |component|
:Required: Yes

Specifies the build process of the component, according to its :attribute:`Type`.

:attribute:`C-Runtime-Vendor`
-----------------------------

:Type: |string|
:Applies To: |platform|
:Required: No

Specifies that the package's CABI components require the specified C standard/runtime library. Typical (case-insensitive) values include :string:`"bsd"` (libc), :string:`"gnu"` (glibc), :string:`"mingw"` and :string:`"microsoft"`.

:attribute:`C-Runtime-Version`
------------------------------

:Type: |string|
:Applies To: |platform|
:Required: No

Specifies the minimum C standard/runtime library version required by the package's CABI components.

:attribute:`Clr-Vendor`
-----------------------

:Type: |string|
:Applies To: |platform|
:Required: No

Specifies that the package's CLR (.NET) components require the specified `Common Language Runtime`_ vendor. Typical (case-insensitive) values include :string:`"microsoft"` and :string:`"mono"`.

:attribute:`Clr-Version`
------------------------

:Type: |string|
:Applies To: |platform|
:Required: No

Specifies the minimum `Common Language Runtime`_ version required to use the package's CLR (.NET) components.

:attribute:`Command`
--------------------

:Type: |string|
:Applies To: |assembly|, |configuration|
:Required: Depends

Specifies the shell command line that carries out the build process of the component. Substitution expressions may be used to expand |assembly| attributes such as ``$(sources)``.

Command execution occurs in the package working directory

If not specified, a build tool is free to choose the most suitable build process for the component. Thus, this attribute is only required for |component|\ s that are of :string:`"generic"` :attribute:`Type`, and it is discouraged for all other types.

:attribute:`Compat-Version`
---------------------------

:Type: |string|
:Applies To: |package|
:Required: No

Specifies the oldest version of the package with which this version is compatible. This information is used when a consumer requests a specific version. If the version requested is equal to or newer than the :attribute:`Compat-Version`, the package may be used.

If not specified, the package is not compatible with previous versions (i.e. :attribute:`Compat-Version` is implicitly equal to :attribute:`Version`).

:attribute:`Compile-Features` :applies-to:`(Assembly)`
------------------------------------------------------

:Type: |string-list|
:Applies To: |assembly|, |configuration|
:Required: No

Specifies a list of `Compiler Features`_ that must be enabled or disabled when compiling the component.

:attribute:`Compile-Features` :applies-to:`(Distribution)`
----------------------------------------------------------

:Type: |string-list|
:Applies To: |distribution|, |configuration|
:Required: No

Specifies a list of `Compiler Features`_ that must be enabled or disabled when compiling code that consumes the component.

:attribute:`Compile-Flags` :applies-to:`(Assembly)`
---------------------------------------------------

:Type: |language-string-list|
:Applies To: |assembly|, |configuration|
:Required: No

Specifies a list of additional flags that must be supplied to the compiler when compiling the component. Note that compiler flags may not be portable.

A map may be used instead to give different values depending on the language of the consuming source file. Handling of such shall be the same as for `Definitions`_.

:attribute:`Compile-Flags` :applies-to:`(Distribution)`
-------------------------------------------------------

:Type: |language-string-list|
:Applies To: |distribution|, |configuration|
:Required: No

Specifies a list of additional flags that must be supplied to the compiler when compiling code that consumes the component. Note that compiler flags may not be portable; use of this attribute is discouraged.

A map may be used instead to give different values depending on the language of the consuming source file. Handling of such shall be the same as for `Definitions`_.

:attribute:`Components` :applies-to:`(Package)`
-----------------------------------------------

:Type: |map| to |component|
:Applies To: |package|
:Required: Yes

Specifies the components which the package provides. Keys are the component names.

:attribute:`Components` :applies-to:`(Requirement)`
---------------------------------------------------

:Type: |string-list|
:Applies To: |requirement|
:Required: No

Specifies a list of components which must be present in the required package in order for the requirement to be satisfied. Although the build tool will generally produce an error if a consumer uses a component which in turn requires a component that was not found, early specification via this attribute may help build tools to diagnose such issues earlier and/or produce better diagnostics.

This may also be used to specify dependencies that are not expressed in component level dependencies, such as a package's requirement that a dependency includes a certain symbolic component, or if a dependency is only expressed at run-time.

:attribute:`Configuration`
--------------------------

:Type: |string|
:Applies To: |package|
:Required: Special

Specifies the name of the configuration described by a configuration-specific ``.cbs`` (see `Configuration Merging`_). This attribute is required in a configuration-specific ``.cbs``, and ignored otherwise.

:attribute:`Configurations` :applies-to:`(Package)`
---------------------------------------------------

:Type: |string-list|
:Applies To: |package|
:Required: No

Specifies the configurations that are available. See `Package Configurations`_ for a description of how configurations are used.

:attribute:`Configurations` :applies-to:`(Assembly)`
----------------------------------------------------

:Type: |map| to |configuration|
:Applies To: |assembly|
:Required: No

Specifies a set of platform and configuration-specific attributes to build the component. Keys follow the ``platform-name[configuration-name]`` syntax. Either the platform name or the configuration name may be left unspecified, but not both. An unspecified platform name implies that attributes are agnostic to the package's supported platforms. An unspecified configuration name implies that attributes are agnostic to package's configurations.

:attribute:`Configurations` :applies-to:`(Distribution)`
--------------------------------------------------------

:Type: |map| to |configuration|
:Applies To: |distribution|
:Required: No

Specifies a set of platform and configuration-specific attributes to distribute the component. Keys follow the ``platform-name[configuration-name]`` syntax. Either the platform name or the configuration name may be left unspecified, but not both. An unspecified platform name implies that attributes are agnostic to the package's platforms. An unspecified configuration name implies that attributes are agnostic to package's configurations.

:attribute:`Cpp-Compiler`
-------------------------

:Type: |string|
:Applies To: |platform|
:Required: No

Specifies the C++ compiler that the package's components can be built and consumed with. Typical (case-insensitive) values include :string:`"gnu"`, :string:`"clang"`, and :string:`"msvc"`.

:attribute:`Cpp-Runtime-Vendor`
-------------------------------

:Type: |string|
:Applies To: |platform|
:Required: No

Specifies that the package's CABI components require the specified C standard/runtime library. Typical (case-insensitive) values include :string:`"gnu"` (libstdc++), :string:`"llvm"` (libc++) and :string:`"microsoft"`.

:attribute:`Cpp-Runtime-Version`
--------------------------------

:Type: |string|
:Applies To: |platform|
:Required: No

Specifies the minimum C standard/runtime library version required by the package's CABI components.

:attribute:`Cbs-Path`
---------------------

:Type: |string|
:Applies To: |package|
:Required: No

Specifies the directory portion location of the ``.cps`` file. This shall be an "absolute" path which starts with ``@prefix@``. This provides an additional mechanism by which the tool may deduce the package's prefix, since the absolute location of the ``.cps`` file will be known by the tool. (See also `Prefix Determination`_.)

:attribute:`Cbs-Version`
------------------------

:Type: |string|
:Applies To: |package|
:Required: Yes

Specifies the version of the CBS to which this ``.cbs`` file conforms. This may be used by tools to provide backwards compatibility in case of compatibility-breaking changes in the CBS.

CBS version numbering follows |semver|_. That is, tools that support CBS version ``<X>.<Y>`` are expected to be able to read files with :attribute:`Cbs-Version` ``<X>.<Z>``, even for Z > Y (with the understanding that the tool may miss non-critical information that the CBS provided in such cases).

:attribute:`Default-Components`
-------------------------------

:Type: |string-list|
:Applies To: |package|
:Required: No

Specifies a list of components that should be inferred if a consumer specifies a dependency on a package, but not a specific component.

:attribute:`Definitions` :applies-to:`(Assembly)`
--------------------------------------------------

:Type: |language-string-list|
:Applies To: |assembly|, |configuration|
:Required: No

Specifies a list of compile definitions that must be defined when compiling the component. Definitions should be in the form :string:`"FOO"` or :string:`"FOO=BAR"`. Additionally, a definition in the form :string:`"!FOO"` indicates that the specified symbol (``FOO``, in this example) shall be explicitly undefined (e.g. ``-UFOO`` passed to the compiler).

A map may be used instead to give different values depending on the language of the consuming source file. In this case, the build tool shall select the list from the map whose key matches the (case-insensitive) language of the source file being compiled. Recognized languages shall include :string:`"C"`, :string:`"C++"`, and :string:`"Fortran"`.

:attribute:`Definitions` :applies-to:`(Distribution)`
-----------------------------------------------------

:Type: |language-string-list|
:Applies To: |distribution|, |configuration|
:Required: No

Specifies a list of compile definitions that must be defined when consuming the component.

Equivalent to `Definitions (Assembly)`_.

:attribute:`Distribution`
--------------------------

:Type: |distribution|
:Applies To: |component|
:Required: No

Specifies the procedure to distribute a component, according to its :attribute:`Type`. If not specified, the component is not distributed. This is helpful for components that are a instrumental for building, but not consuming, other components.

:attribute:`Hints`
------------------

:Type: |string-list|
:Applies To: |requirement|
:Required: No

Specifies a list of paths where a required dependency might be located. When given, this will usually provide the location of the dependency as it was consumed by the package when the package was built, so that consumers can easily find (correct) dependencies if they are in a location that is not searched by default.

:attribute:`Include-Location`
-----------------------------

:Type: |string-list-map|
:Applies To: |distribution|, |configuration|
:Required: No

Specifies the locations of the header files which are needed to compile code that consumes the component. This attribute typically applies to components distributed separately from their sources.

Keys are paths to include directories. If a path starts with ``@prefix@``, the package's install prefix is substituted. This is recommended, as it allows packages to be relocatable.

Values are lists of paths to header files. Header files are copied into the include directory specified by the key, preserving directory structure.

:attribute:`Include-Requires` :applies-to:`(Assembly)`
------------------------------------------------------

:Type: |string-list|
:Applies To: |assembly|, |configuration|
:Required: No

Specifies additional components required to compile the component which are needed only at the pre-processing stage. Unlike `Requires (Assembly)`_, only the required components' include paths should be applied transitively; additional properties such as compile and link attributes of the required component(s) should be ignored.

:attribute:`Include-Requires` :applies-to:`(Distribution)`
----------------------------------------------------------

:Type: |string-list|
:Applies To: |distribution|, |configuration|
:Required: No

Specifies additional components required when consuming the component which are needed only at the pre-processing stage. Unlike `Requires (Distribution)`_, only the required components' include paths should be applied transitively; additional properties such as compile and link attributes of the required component(s) should be ignored.

:attribute:`Includes` :applies-to:`(Assembly)`
----------------------------------------------

:Type: |language-string-list|
:Applies To: |assembly|, |configuration|
:Required: No

Specifies a list of directories which should be added to the include search path when compiling the component.

A map may be used instead to give different values depending on the language of the consuming source file. Handling of such shall be the same as for `Definitions`_.

:attribute:`Includes` :applies-to:`(Distribution)`
--------------------------------------------------

:Type: |language-string-list|
:Applies To: |distribution|, |configuration|
:Required: No

Specifies a list of directories which should be added to the include search path when compiling code that consumes the component. If a path starts with ``@prefix@``, the package's install prefix is substituted.

A map may be used instead to give different values depending on the language of the consuming source file. Handling of such shall be the same as for `Definitions`_.

:attribute:`Isa`
----------------

:Type: |string|
:Applies To: |platform|
:Required: No

Specifies that the package's CABI components require the specified `Instruction Set Architecture`_. The value is case insensitive and should follow the output of ``uname -m``.

:attribute:`Jvm-Vendor`
-----------------------

:Type: |string|
:Applies To: |platform|
:Required: No

Specifies that the package's Java components require the specified Java_ vendor. Typical (case-insensitive) values include :string:`"oracle"` and :string:`"openjdk"`.

:attribute:`Jvm-Version`
------------------------

:Type: |string|
:Applies To: |platform|
:Required: No

Specifies the minimum Java_ Virtual Machine version required to use the package's Java components.

:attribute:`Kernel`
-------------------

:Type: |string|
:Applies To: |platform|
:Required: No

Specifies the name of the operating system kernel required by the package's components. The value is case insensitive and should follow the output of ``uname -s``. Typical values include :string:`"windows"`, :string:`"cygwin"`, :string:`"linux"` and :string:`"darwin"`.

:attribute:`Kernel-Version`
---------------------------

:Type: |string|
:Applies To: |platform|
:Required: No

Specifies the minimum operating system kernel version required by the package's components.

:attribute:`Link-Features` :applies-to:`(Assembly)`
---------------------------------------------------

:Type: |string-list|
:Applies To: |assembly|, |configuration|
:Required: No

Specifies a list of `Linker Features`_ that must be enabled or disabled when compiling the component.

:attribute:`Link-Features` :applies-to:`(Distribution)`
-------------------------------------------------------

:Type: |string-list|
:Applies To: |distribution|, |configuration|
:Required: No

Specifies a list of `Linker Features`_ that must be enabled or disabled when compiling code that consumes the component.

:attribute:`Link-Flags` :applies-to:`(Assembly)`
------------------------------------------------

:Type: |string-list|
:Applies To: |assembly|, |configuration|
:Required: No

Specifies a list of additional flags that must be supplied to the linker when linking the component. Note that linker flags may not be portable.

:attribute:`Link-Flags` :applies-to:`(Distribution)`
----------------------------------------------------

:Type: |string-list|
:Applies To: |distribution|, |configuration|
:Required: No

Specifies a list of additional flags that must be supplied to the linker when linking code that consumes the component. Note that linker flags may not be portable; use of this attribute is discouraged.

:attribute:`Link-Languages`
---------------------------

:Type: |string-list|
:Applies To: |distribution|, |configuration|
:Required: No

Specifies the ABI language or languages of a static library (`Type`_ :string:`"archive"`). Officially supported (case-insensitive) values are :string:`"C"` (no special handling required) and :string:`"C++"` (consuming the static library also requires linking against the C++ standard runtime). The default is :string:`"C"`.

:attribute:`Link-Libraries` :applies-to:`(Assembly)`
----------------------------------------------------

:Type: |string-list|
:Applies To: |assembly|, |configuration|
:Required: No

Specifies a list of additional libraries that must be linked against when linking the component. (Note that packages should avoid using this attribute if at all possible. Use `Requires (Assembly)`_ instead whenever possible.)

:attribute:`Link-Libraries` :applies-to:`(Distribution)`
--------------------------------------------------------

:Type: |string-list|
:Applies To: |distribution|, |configuration|
:Required: No

Specifies a list of additional libraries that must be linked against when linking code that consumes the component. (Note that packages should avoid using this attribute if at all possible. Use `Requires (Distribution)`_ instead whenever possible.)

:attribute:`Link-Location`
--------------------------

:Type: |string|
:Applies To: |distribution|, |configuration|
:Required: No

Specifies an alternate location of the component that should be used when linking against the component. This attribute typically applies only to :string:`"dylib"` components on platforms where the library is separated into multiple file components. For example, on Windows, this attribute shall give the location of the ``.lib``, while `Location`_ shall give the location of the ``.dll``.

If the path starts with ``@prefix@``, the package's install prefix is substituted. This is recommended, as it allows packages to be relocatable.

:attribute:`Link-Requires` :applies-to:`(Assembly)`
---------------------------------------------------

:Type: |string-list|
:Applies To: |assembly|, |configuration|
:Required: No

Specifies additional components required to compile the component which are needed only at the link stage. Unlike `Requires (Assembly)`_, only the required components' link dependencies should be applied transitively; additional properties such as compile and include attributes of the required component(s) should be ignored.

:attribute:`Link-Requires` :applies-to:`(Distribution)`
-------------------------------------------------------

:Type: |string-list|
:Applies To: |distribution|, |configuration|
:Required: No

Specifies additional components required by a component which are needed only at the link stage. Unlike `Requires (Distribution)`_, only the required components' link dependencies should be applied transitively; additional properties such as compile and include attributes of the required component(s) should be ignored.

:attribute:`Location`
---------------------

:Type: |string|
:Applies To: |distribution|, |configuration|
:Required: No

Specifies the location of the component. The exact meaning of this attribute depends on the component type, but typically it provides the path to the directory where the component's primary artifact, such as a ``.so`` or ``.jar``, is located. (For Windows DLL components, this should be the location of the ``.dll``. See also `Link-Location`_.)

If the path starts with ``@prefix@``, the package's install prefix is substituted. If the path starts with ``@python-prefix@``, Python site-packages prefix under the package's install prefix is substituted. This is recommended, as it allows packages to be relocatable.

If not specified, the build tool is free to choose a suitable location under the package's install prefix and according to the component type.

:attribute:`Name` :applies-to:`(Package)`
-----------------------------------------

:Type: |string|
:Applies To: |package|
:Required: Yes

Specifies the canonical name of the package. In order for searching to succeed, this must exactly match the name of the CBS file without the ``.cbs`` suffix.

:attribute:`Name` :applies-to:`(Assembly)`
------------------------------------------

:Type: |string|
:Applies To: |assembly|, |configuration|
:Required: No

Specifies the base name of the artifact associated to a component once built. If not set, the component name is used.

:attribute:`Name` :applies-to:`(Distribution)`
----------------------------------------------

:Type: |string|
:Applies To: |distribution|, |configuration|
:Required: No

Specifies the base name of the artifact associated to a component. If not set, the component name is used.

:attribute:`Platforms`
----------------------

:Type: |map| to |platform|
:Applies To: |package|
:Required: No

Specifies the platforms on which a package's components may be built and distributed. This allows tools to ignore packages which target a different platform than the platform that the consumer targets. It also allows for platform-specific configurations. Keys are the platform names.

Any platform attribute not specified implies that the platform definition is agnostic to that attribute. If this attribute is not specified, the package is implied to be platform agnostic. (This might be the case for a "library" which consists entirely of C/C++ headers. Note that JVM/CLR versions are platform attributes, so packages consisting entirely of Java and/or CLR components will still typically use this attribute.)

:attribute:`Requires` :applies-to:`(Assembly)`
----------------------------------------------

:Type: |string-list|
:Applies To: |assembly|, |configuration|
:Required: No

Specifies additional components required to build the component. Relative component names are interpreted relative to the current package. Absolute component names must refer to a package required by this package (see `Requires (Package)`_). Compile and link attributes should be applied transitively, as if the component also directly consumed the components required by the component being consumed.

See also `Include-Requires (Assembly)`_ and `Link-Requires (Assembly)`_.

:attribute:`Requires` :applies-to:`(Distribution)`
--------------------------------------------------

:Type: |string-list|
:Applies To: |distribution|, |configuration|
:Required: No

Specifies additional components required to consume the component. This is used, for example, to indicate transitive dependencies. Relative component names are interpreted relative to the current package. Absolute component names must refer to a package required by this package (see `Requires (Package)`_). Compile and link attributes should be applied transitively, as if the consuming component also directly consumed the components required by the component being consumed.

See also `Include-Requires (Distribution)`_ and `Link-Requires (Distribution)`_.

:attribute:`Sources` :applies-to:`(Assembly)`
---------------------------------------------

:Type: |string-list|
:Applies To: |assembly|, |configuration|
:Required: Depends

Specifies the paths to source files to build the component, if any.

:attribute:`System-Includes` :applies-to:`(Assembly)`
-----------------------------------------------------

:Type: |language-string-list|
:Applies To: |assembly|, |configuration|
:Required: No

Specifies a list of directories which should be added to the include search path as system include directories when compiling the component. The effect of specifying an include path as a system include path (as opposed to an ordinary include path, see `Includes (Assembly)`_) is compiler specific.

A map may be used instead to give different values depending on the language of the consuming source file. Handling of such shall be the same as for `Definitions (Assembly)`_.

:attribute:`Requires` :applies-to:`(Package)`
---------------------------------------------

:Type: |map| to |requirement|
:Applies To: |package|
:Required: No

Specifies additional packages that are required by this package. Keys are the name of another required package. Values are a valid |requirement| object or |null| (equivalent to an empty |requirement| object) describing the package required.

:attribute:`Type`
-----------------

:Type: |string| (restricted)
:Applies To: |component|
:Required: Yes

Specifies the type of a component. The component type affects how the component may be built and distributed. Officially supported values are :string:`"executable"` (any artifact which the target platform can directly execute), :string:`"archive"` (CABI static library), :string:`"dylib"` (CABI shared library), :string:`"module"` (CABI plugin library), :string:`"jar"` (Java Archive), :string:`"pymodule"` (Python module), :string:`"cpyext"` (CPython extension library), :string:`"generic"` (file collection), :string:`"interface"` and :string:`"symbolic"`. If the type is not recognized by the parser, the component shall be ignored. (Parsers are permitted to support additional types as a conforming extension.)

A :string:`"dylib"` is meant to be linked at compile time, whereas a :string:`"module"` is meant to be loaded at run time with :code:`dlopen` or similar.

A :string:`cpyext` implicitly depends on CPython development libraries. It also respects extension naming conventions.

A :string:`generic` component is meant to build and distribute arbitrary artifacts. It is intended for, but not limited to, source file generation.

An :string:`"interface"` component is a special case; it may have the usual attributes of a component, but does not have a location. This can be used to create "virtual" components that do not have an associated artifact.

A :string:`"symbolic"` component is even more special, as it has no (required) attributes at all, and the meaning of any attributes or configurations assigned to such a component is unspecified. A :string:`"symbolic"` component is intended to be used as a form of feature testing; a package that has a feature that is meaningful to users but does not otherwise map directly to a component may use a symbolic component to indicate availability of the feature to users.

:attribute:`Version` :applies-to:`(Package)`
--------------------------------------------

:Type: |string|
:Applies To: |package|
:Required: No

Specifies the version of the package. The format of this string is determined by `Version-Schema`_.

If not provided, the CBS will not satisfy any request for a specific version of the package.

:attribute:`Version` :applies-to:`(Requirement)`
------------------------------------------------

:Type: |string|
:Applies To: |requirement|
:Required: No

Specifies the required version of a package. If omitted, any version of the required package is acceptable. Semantics are the same as for the :attribute:`Version` attribute of a |package|.

:attribute:`Version-Schema`
---------------------------

:Type: |string|
:Applies To: |package|
:Required: No

Specifies the structure to which the package's version numbering conforms. Tools may use this to determine how to perform version comparisons. Officially supported (case-insensitive) values are :string:`"semver"` (|semver|_) and :string:`"custom"` (:string:`"rpm"` or :string:`"dpkg"` should be used where applicable, but may not be supported by all tools). If a package uses :string:`"custom"`, version numbers may be compared, but version ordering is not possible. The default is :string:`"semver"`.

Needless to say, changing a package's version scheme between releases is *very strongly discouraged*.

Note that this attribute determines only how version numbers are *ordered*. It does not also imply that a package actually maintains and breaks compatibility as specified by |semver|. See also `Compat-Version`_.

Notes
'''''

- Unless otherwise specified, a relative file path appearing in a CBS shall be interpreted relative to the ``.cbs`` file.

- Unless otherwise specified, unrecognized attributes shall be ignored. This makes it easier for tools to add tool-specific extensions. (It is *strongly* recommended that the names of any such attributes start with ``X-<tool>-``, where ``<tool>`` is the name of the tool which introduced the extension, in order to reduce the chance of conflicts with newer versions of the CBS.)

- The term "CABI", as used throughout, refers to (typically C/C++/Fortran) code compiled to the machine's native instruction set and using the platform's usual format for such binaries (ELF, PE32, etc.).

.. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. ..

.. _Common Language Runtime: https://en.wikipedia.org/wiki/Common_Language_Runtime

.. _Instruction Set Architecture: https://en.wikipedia.org/wiki/Instruction_set_architecture

.. _Java: https://en.wikipedia.org/wiki/Java_%28programming_language%29

.. _semver: http://semver.org/

.. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. ..

.. |semver| replace:: Semantic Versioning

.. |null| replace:: :keyword:`null`

.. |string| replace:: :type:`string`

.. |list| replace:: :type:`list`

.. |string-list| replace:: |list| of |string|

.. |map| replace:: :type:`map` of |string|

.. |string-list-map| replace:: |map| to |string-list|

.. |language-string-list| replace:: |string-list| :separator:`or` |map| to |string-list|

.. |package| replace:: :object:`package`

.. |platform| replace:: :object:`platform`

.. |requirement| replace:: :object:`requirement`

.. |component| replace:: :object:`component`

.. |configuration| replace:: :object:`configuration`

.. |assembly| replace:: :object:`assembly`

.. |distribution| replace:: :object:`distribution`

.. kate: hl reStructuredText
