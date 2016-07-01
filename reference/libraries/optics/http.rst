HTTP
====

The **Freya.Optics.Http** library provides optics from the ``State`` to the various aspects of the request and response, modelled using the types from **Freya.Types.Http** (and other Freya.Types.* libraries where needed). These optics are usable directly within a ``freya`` computation expression, working with the optic functions -- see :doc:`/reference/libraries/core/optics`.

The HTTP optics are likely to be the most commonly used optics dealing with request and response data. To use the optic the following modules should be opened:

.. code-block:: fsharp

   // Working with Freya optics
   open Freya.Optics.Http

   // Working with the Freya types (maybe required)
   open Freya.Types.Http

The optics are all provided under the ``Request`` and ``Response`` modules (e.g. ``Request.path_``), along with sub-modules for headers (e.g. ``Request.Headers.accept_``).
