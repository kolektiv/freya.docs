Machine Design Overview
=======================

Background
----------

Machine is quite a conceptually simple model, but it can be quite a big shift in thinking for those who are new to using it. It works differently to how people are using to programming for the web, which is commonly quite imperative and often focuses primarily on the mechanical nature of web programming.

Even within the most commonly used "high level" frameworks such as ASP.NET MVC, while the mechanics of dealing with requests and responses are quite significantly abstracted, the `logic` of handling requests and deciding on appropriate responses is largely left to the programmer to implement.

For example, it's not uncommon to see the following kind of logic ASP.NET MVC Controllers:

.. code::

   Is the query valid?
   No:
       Return an Error code
   Yes:
       Could the data be found given the query?
       No:
           Return an Not Found code
       Yes:
           Did the user indicate that they already have some copy of this resource? (Perhaps ETag?)
           Yes:
               Does that ETag match the ETag for the data I've found?
               Yes:
                   Return a Not Modified code
               No:
                   ...
           No:
               ...

Of course, this is only if people even think about things like content caching, or negotiation, or any of the other concepts which HTTP includes! And while it's possible to factor some of these things out in to reusable elements (perhaps some Action Filters or similar in ASP.NET MVC for example) the challenge is now to compose and use all of these factored elements correctly in combination!

Given this situation, alternative ways of defining resources programatically are attractive. One of these approaches (which we will term the `machine` approach) was pioneered by the `WebMachine <https://github.com/webmachine/webmachine>`_ project in the Erlang world, and has been adopted and adapted/extended by other implementations in various languages (usually functional languages, as it's an excellent fit).

Freya Machine is one of those implementations, and it also adds some new features and thinking to the approach which make Freya Machine unique (at least that we know of).

Concept
-------

The basic concept at the heart of the `machine` approach is to treat the handling of a request as a series of decisions. Each decision will get us closer to being able to send the most appropriate reponse to the client. So for example, the logical example given in the Background section above can be seen quite easily as a series of questions (or decisions) - Is the request valid? Does the resource exist? etc.

These decisions when chained together form a **decision graph** (it isn't quite a tree, as some of the branches join back up again). When a request is received, the `machine` implementation will start at the first node in the decision graph, and will navigate through the graph based on the answer to the decision it finds at each node.

So if the first node was the decision `"Is the request valid?"`, a value of **true** would mean following the edge from the first node which had a value of **true** -- to the next node, which has the decision `"Does the resource exist?"` and so on. If the answer was **false** however, we would follow a different path -- to a node which represented the concept `"Finish with an Error response"`.

In this way, the complete logic of HTTP can be modelled as a series of decisions, each leading towards the correct semantic response to a given request.

Implementation
--------------

Decisions
`````````

One of the first things to note about this model is of course that there are `many` decisions made as part of an HTTP request. Forcing the programmer to implement logic for every decision for every resource would be laborious -- so every decision has a `default` function which is evaluated, unless that function is overridden by the programmer providing their own function. For example, the decision `"Does the resource exist?"` in our example has a default function which simply returns **true**. In situations where that might not always be the case, the programmer can choose to override that decision.

In this way, resources can be defined by only overriding the subset of decisions which are relevant for that resource. In practice, this is usually not a particularly large set -- and many decisions are the same for multiple resources, and so the logic can be re-used conveniently.

Properties
``````````

Of course, some of the **decisions** which must be made in processing a request are quite complex. While the decision must result in a **true/false** answer, the logic is fixed -- it simply depends on properties of the resource in question. In these cases, the decisions themselves are defined internally -- they are not available for overriding, as the logic is essentially standardised. However, the logic often relies on properties of the resource, which may be defined by the programmer.

As with decisions, all properties have default functions defined, which return a default value for the property. For example, a decision `"Does the resource allow the requested HTTP method?"` is implemented by an internal decision, using a property which the programmer may override -- returning a list of allowable methods. By default, this property returns a list containing only **GET** and **HEAD**, so this is overridden whenever a resource should respond to other methods.

Handlers
````````

With decisions, and internal decisions using properties, the `machine` approach has enough information to correctly handle a request for a defined resource, but there still remains the need to return the actual response to the client. This is accomplished with handlers. A handler is the final function called when processing a request, and it provides the data (if any) to send as part of the response. It may also modify the response in other ways, if wished.

There is a handler defined for every possible response the resource may return to the client, so for example, a normally handled **GET** request will respond with **200 OK**. There is a handler for this (in Freya Machine, this handler is called ``handleOk``). If a request resulted in sending a **404 Not Found**, the handler ``handleNotFound`` would be used, and so on. There is a handler for each response that the resource may return.

As with **decisions** and **properties**, handlers have default implementations, which may be overridden. You only need to override the appropriate handlers (so for example if your resource supports **GET** as a method, you probably want to implement ``handleOk``, to send the data that has been requested). The default handlers are usually logical, and most only need to be overridden in special circumstances.

Actions
```````

The final component of the `machine` style framework is a way to carry out the processing that a request may imply. **GET** requests imply no change to the system, but a **POST** request may result (if the **POST** request is found acceptable according to the `machine` logic) in making a change to the system.

These changes can be implemented by overriding the `actions`. For example, to implement the logic that should occur when a **POST** is accepted for a resource, you should override the ``doPost`` action.

All `actions` have defaults, which in every case are simply no-ops -- you will generally wish to provide actions in cases where your resources accept modification.

Summary
-------

We've covered a high level view of the components of a `machine` style resource. Some of the concepts are probably not too clear yet though, and they are explored more fully in the following sections, along with the introduction of other features specific to the Freya Machine implementation.

For now, remembering that the `machine` approach involves a sequence of decisions which you can override, and handlers which you can provide to implement your own content is probably enough to start reading further, and understanding in more detail through the examples in the following sections.
