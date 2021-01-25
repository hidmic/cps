Usage and Implementation Recommendations
========================================

Configurations as Flags
'''''''''''''''''''''''

Let's say your package includes several configurations of a library, where the configuration is logically specified as a set of orthogonal attributes (e.g. debug/release, static/shared). What's the best way to provide these to your users?

This is best accomplished via interface components. For example:

.. code-block:: javascript

  "Components": {
    "foo-static": {
      "Type": "archive",
      "Configurations": {
        "Release": { ... },
        "Debug": { ... }
      }, ...
    },
    "foo-shared": {
      "Type": "dylib",
      "Configurations": {
        "Release": { ... },
        "Debug": { ... }
      }, ...
    },
    "foo": {
      "Type": "interface",
      "Configurations": {
        "Static": { "Requires": [ "foo-static" ] },
        "Shared": { "Requires": [ "foo-shared" ] }
      }
    },
  },
  ...

This pattern allows the user to specify their set of preferred configurations like ``"Static", "Release"`` rather than ``"ReleaseStatic"``. When consuming the ``foo`` component, the build tool will select on the static/shared axis, which will bring in a component whose configurations all match that choice. The tool will then select the transitive component on the remaining debug/release axis. Longer chains can be constructed to support configuration choices of arbitrary complexity. Where appropriate (and assuming that the package does not wish to support the user naming the "real" components directly), the component graph can be constructed in a way that allows shared attributes to be specified at the higher level interface components.

Link-Only Requirements and Link Order
'''''''''''''''''''''''''''''''''''''

In most cases, the order in which "full" and link-only dependencies are linked will not matter. When an exception occurs, it can be addressed in one of two manners:

- If a "full" dependency merely needs to be linked *after* a link-only dependency, the dependency can simply be listed twice; once in :attribute:`Requires`, and again in :attribute:`Link-Requires`. (Tools are encouraged to add link-only dependencies after "full" dependencies.) This is redundant, but often satisfactory.

- If strict linking order without duplication is required, the link-only dependency may be wrapped in an :string:`"interface"` component which is listed as a "full" dependency. (Note that tools are expected to expand requirements depth-first rather than breadth-first.)

Transitive Dependencies
'''''''''''''''''''''''

When a package is located, it is intended that the tool would also locate any `Requires (Package)`_ mentioned by the package. In some cases, however, a user may want to use only some components of a package, which may have a more limited set of dependencies than the package as a whole when every component of the package is considered.

.. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. ..

.. kate: hl reStructuredText
