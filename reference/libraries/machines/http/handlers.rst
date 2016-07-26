Handlers
========

Handlers are called as the final step in an HTTP Machine execution -- they are used to set properties of the response, usually a payload (where applicable) and potentially additional headers. Handlers are always optional -- when a handler is not defined, the response will simply be returned without a payload being set (and associated headers). This is a valid approach in many situations, especially in HTTP cases where a payload is not common for the response.

Representations
---------------

The HTTP Machine allows you to declare handlers in three ways, allowing you to use a simpler formulation when it is all that's required. All handlers must return a ``Representation``, which defines the content and associated metadata (such as media type, language, etc. where relevant), but this may be done in the following ways:

Dynamic Negotiated
^^^^^^^^^^^^^^^^^^

The most comprehensive case allows for the selection of a representation according to the content negotiation which the HTTP Machine may have performed (if you have configured the Machine to do so -- see the section on Properties). In this case, the handler defined within the Machine must be of the form ``Acceptable -> Freya<Representation>``, where ``Acceptable`` is a type giving the results of content negotiation. The handler may use this information to select the most appropriate representation.

Dynamic
^^^^^^^

A simpler case occurs when content negotiation is not realistically required -- this may often be the case for simpler APIs, or where the API only offers limited options (an all JSON API for example). In this case the handler must be of the form ``Freya<Representation>`` -- the handler may work with the request, response, and other data, but is not given information regarding content negotiation.

Static
^^^^^^

The simplest case is for instances when the representation is known at compile time, and a handler of the form ``Representation`` can be defined. This is useful for simple static content, etc.

Status Codes
------------

Handlers are defined and named according to the following tables, categorised by response status code (these codes, along with other commonly recommended/required headers will be set automatically). Note that some status codes may be returned by more than one handler, such as the handlers for a normal **200** response for OPTIONS requests, and for other requests.

2xx
^^^

+--------+---------------+
| Status | Handler       |
+========+===============+
| 200    | handleOk      |
+--------+---------------+
| 200    + handleOptions |
+--------+---------------+
