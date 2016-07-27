HTTP CORS
=========

The **Freya.Types.Http.Cors** library implements types which represent the semantics of the following standards:

* `W3C Recommendation on CORS <http://www.w3.org/TR/2014/REC-cors-20140116>`_
* `RFC 6454 -- The Web Origin Concept <http://tools.ietf.org/html/rfc6454>`_
  
The implementation of the Recommendation consists of a set of typed headers. Additionally the Origin header is implemented as defined in RFC 6454. Strongly typed representations and parsers are given.

.. note::

   Full documentation for the individual type designs within Freya.Types.Http.Cors is not currently available, but will be added at a later stage. Inspecting the values returned however should be straightforward and logical, and all typed representations map very closely to the logical design/grammar defined within the appropriate RFC or Recommendation.
   
To use the types:

.. code-block:: fsharp

   // Working with the types
   open Freya.Types.Http.Cors
