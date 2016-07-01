Polyfills
=========

The OWIN standard is a good base point for developing interoperable servers and frameworks, but like any standard it requires iteration and development. Sometimes it lacks features which Freya requires to work as well as possible -- and in those cases Freya provides polyfills for particular server implementations which will "extend" the OWIN standard with additional features which Freya code can then test for and use where present.

This enables Freya to drive forward and improve at a fast pace.

For documentation on the polyfills currently available for Freya see the library reference.

* :doc:`/reference/libraries/polyfills/index` -- available polyfills for Freya, providing extensions to standards such as OWIN.
