Language
========

The Arachne ``Arachne.Language`` library implements types which represent the semantics of the following standards:

* `RFC 4647 -- Language Ranges (Matching of Language Tags) <http://tools.ietf.org/html/rfc4647>`_
* `RFC 5646 -- Language Tags (Tags for Identifying Languages) <http://tools.ietf.org/html/rfc5646>`_

Strongly typed representations and parsers are given.

No optics are given as these types are not present directly within HTTP messages, but they are used within some types in the HTTP and HTTP CORS libraries, and may be used directly when working with some of the higher levels of abstraction in the Freya stack which expect strongly typed Language Tags/Ranges as configuration values.

.. code-block:: fsharp

   // Working directly with the types
   open Arachne.Language

.. note::

   Full documentation for the individual type designs within Arachne is not currently available, but will be added at a later stage. Inspecting the values returned however should be straightforward and logical, and all typed representations map very closely to the logical design/grammar defined within the appropriate RFC or Recommendation.
