HTTP CORS
=========

The Arachne ``Arachne.Http.Cors`` library implements types which represent the semantics of the following standards:

* `W3C Recommendation on CORS <http://www.w3.org/TR/2014/REC-cors-20140116>`_
* `RFC 6454 -- The Web Origin Concept <http://tools.ietf.org/html/rfc6454>`_
  
The implementation of the Recommendation consists of a set of typed headers. Additionally the Origin header is implemnted as defined in RFC 6454. Strongly typed representations and parsers are given.

The ``Freya.Lenses.Http.Cors`` library provides optics from the ``FreyaState`` to the various aspects of the request and response defined within the Recommendation and RFC. These optics are usable directly within a ``freya`` computation expression, working with the optic functions detailed in :doc:`/core/optics`.

These types and optics are probably not likely to be commonly used, especially when relying on some of the higher level abstractions available in the Freya stack, but they can be useful for writing new low-level code.

.. code-block:: fsharp

   // Working with Freya optics
   open Freya.Lenses.Http.Cors

   // Working directly with the types if required
   open Arachne.Http.Cors

The optics are all provided under the ``Request.Headers`` and ``Response.Headers`` modules (e.g. ``Request.Headers.accessControlAllowOrigin_``).

.. note::

   Full documentation for the individual type designs within Arachne is not currently available, but will be added at a later stage. Inspecting the values returned however should be straightforward and logical, and all typed representations map very closely to the logical design/grammar defined within the appropriate RFC or Recommendation.
