URI
===

The **Freya.Types.Uri** library implements types which represent the semantics of the following standard:

* `RFC 3986 -- Uniform Resource Identifier (URI): Generic Syntax <http://tools.ietf.org/html/rfc4647>`_

Strongly typed representations and parsers are given.

No optics are given as a corresponding **Freya.Optics.*** library as these types are not present directly within HTTP messages, but they are used within some types in the HTTP and HTTP CORS libraries. They are also used in higher levels of abstraction within the Freya stack.

.. note::

   Full documentation for the individual type designs within Freya.Types.Uri is not currently available, but will be added at a later stage. Inspecting the values returned however should be straightforward and logical, and all typed representations map very closely to the logical design/grammar defined within the appropriate RFC or Recommendation.

To use the types:

.. code-block:: fsharp

   // Working with the types
   open Arachne.Uri
