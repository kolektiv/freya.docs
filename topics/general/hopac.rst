Hopac
=====

The "default" implementation of Freya uses F# async functions for most operations, and the internals are built as async. However, there is an alternative implementation available using the **Hopac** concurrent programming library available.

The Hopac version of Freya is effectively identical in functionality and usage, but uses a different underlying concurrency model -- see :doc:`/reference/libraries/core/functions`. This can make Freya easier to integrate with existing code where Hopac is in use, and also potentially gives some performance/resource usage gains (the Freya Benchmarks project is beginning to measure these reliably).

Packages
--------

Packages which use the Hopac concurrency model are suffixed with **.Hopac**. If you are using the Freya meta-package for example, rather than taking a dependency on **Freya**, you would take a dependency on **Freya.Hopac**. This convention applies to all packages available in both variants.

.. note::

   Not all packages are dependent on a concurrency abstraction, so not all packages come in both "default" and Hopac variants. Packages which do cannot be mixed and matched -- attempting to use one package built for async and one built for Hopac together will result in errors!
   
Reference
---------

For reference and more information on Hopac:

* `Hopac <https://github.com/Hopac>`_ -- the Hopac concurrency library for F#, based on ConcurrentML.
* `Hopac Documentation <http://hopac.github.io/Hopac/Hopac.html>`_ -- reference documentation on Hopac.
