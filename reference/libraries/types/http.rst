HTTP
====

The **Freya.Types.Http** library implements types which represent the semantics of the following standards:

* `RFC 7230 -- Message Syntax and Routing <http://tools.ietf.org/html/rfc7230>`_
* `RFC 7231 -- Semantics and Content <http://tools.ietf.org/html/rfc7231>`_
* `RFC 7232 -- Conditional Requests <http://tools.ietf.org/html/rfc7232>`_
* `RFC 7233 -- Range Requests <http://tools.ietf.org/html/rfc7233>`_
* `RFC 7234 -- Caching <http://tools.ietf.org/html/rfc7234>`_
* `RFC 7235 -- Authentication <http://tools.ietf.org/html/rfc7235>`_

This set of RFCs covers basic types present in HTTP requests and responses, principally the data found in the headers of HTTP messages. Strongly typed representations and parsers are given.

.. note::

   Full documentation for the individual type designs within Freya.Types.Http is not currently available, but will be added at a later stage. Inspecting the values returned however should be straightforward and logical, and all typed representations map very closely to the logical design/grammar defined within the appropriate RFC or Recommendation.
To use the types:

.. code-block:: fsharp

   // Working with the types
   open Freya.Types.Http

