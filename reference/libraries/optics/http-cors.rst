HTTP CORS
=========

The **Freya.Optics.Http.Cors** library provides optics from the ``State`` to the various aspects of the request and response modelled using the types from **Freya.Types.Http.Cors** (and other Freya.Types.* libraries where needed). These optics are usable directly within a ``freya`` computation expression, working with the optic functions detailed in :doc:`/reference/libraries/core/optics`.

These optics are probably not likely to be commonly used, especially when relying on some of the higher level abstractions available in the Freya stack, but they can be useful for writing new low-level code.

.. code-block:: fsharp

   // Working with Freya optics
   open Freya.Optics.Http.Cors

   // Working directly with the types if required
   open Freya.Types.Http.Cors

The optics are all provided under the ``Request.Headers`` and ``Response.Headers`` modules (e.g. ``Request.Headers.accessControlAllowOrigin_``).
