PATCH
=====

The **Freya.Types.Patch** library implements types which represent the semantics of the following standard:

* `RFC 5789 -- PATCH Method for HTTP <http://tools.ietf.org/html/rfc5789>`_

Strongly typed representations and parsers are given, along with matching and rendering logic.

.. note::

   Full documentation for the individual type designs within Freya.Types.Patch is not currently available, but will be added at a later stage. Inspecting the values returned however should be straightforward and logical, and all typed representations map very closely to the logical design/grammar defined within the appropriate RFC or Recommendation.

To use the types:

.. code-block:: fsharp

   // Working with the types
   open Freya.Types.Patch
