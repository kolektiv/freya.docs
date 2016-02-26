URI Template
============

The Arachne ``Arachne.Uri.Template`` library implements types which represent the semantics of the following standard:

* `RFC 6570 -- URI Template <http://tools.ietf.org/html/rfc6570>`_

Strongly typed representations and parsers are given, along with matching and rendering logic.

No optics are given as these types are not present directly within HTTP messages. These types are used extensively in Routing, and in work on the representation of Hypermedia standards.

.. code-block:: fsharp

   // Working directly with the types
   open Arachne.Uri.Template

.. note::

   Full documentation for the individual type designs within Arachne is not currently available, but will be added at a later stage. Inspecting the values returned however should be straightforward and logical, and all typed representations map very closely to the logical design/grammar defined within the appropriate RFC or Recommendation.
