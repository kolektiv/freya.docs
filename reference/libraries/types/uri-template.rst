URI Template
============

The **Freya.Types.Uri.Template** library implements types which represent the semantics of the following standard:

* `RFC 6570 -- URI Template <http://tools.ietf.org/html/rfc6570>`_

Strongly typed representations and parsers are given, along with matching and rendering logic.

No optics are given as a corresponding **Freya.Optics.*** library as these types are not present directly within HTTP messages. These types are used extensively in the Uri Template Routing library, and in work on the representation of Hypermedia standards.

.. note::

   Full documentation for the individual type designs within Freya.Types.Uri.Template is not currently available, but will be added at a later stage. Inspecting the values returned however should be straightforward and logical, and all typed representations map very closely to the logical design/grammar defined within the appropriate RFC or Recommendation.

To use the types:

.. code-block:: fsharp

   // Working with the types
   open Freya.Types.Uri.Template
