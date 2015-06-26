Using Matched Values
====================

As seen in :doc:`/router/routes`, it's quite simple to map routes (the combination of method and path specification) to ``FreyaPipeline`` functions. But once you're running a pipeline, how do you get access to the data that was matched as part of that route?

Route Lenses
------------

Let's revisit one of our URI Templates from the previous section:

.. code-block:: fsharp

    let routeA =
        UriTemplate.Parse "/a/{id}"

Quite simple and easy to understand -- and of course, we're going to want to know what the value of ``{id}`` is at some point when handling a request. Handily, the approach to doing so uses only tools we've already seen.

The Freya router treats matched data (in this case the set of matches as part of matching the URI Template) as part of the state, pushing it in to the underlying OWIN *Environment*, where it can be read by any suitable code. And, just as with Request and Response data, we can provide lenses to this data -- in this case, in the ``Route`` module.

The method of obtaining data from the route is just the same as obtaining data from the request -- simply use the lens functions and a suitable lens.

Syntax
------

What does that look like in this case? Well, here's a function which will return that ``{id}`` value from our example:

.. code-block:: fsharp

   let readId =
       freya {
           return! Freya.getLensPartial (Route.atom "id") }

There are several things to note here. The first is that route lenses are partial. We can't statically be sure that a value will be present (even though intuitively we know that it will be in certain contexts), so route lenses are partial. Additionally, route lenses are parameterised (as we can see, we need to pass it the name of the value we expect to exist, in this case "id").

Another key point to note is that there are three available lenses available (all within the ``Route`` module). In this case, we see ``Route.atom``, but ``Route.list`` and ``Route.keys`` are also available. Briefly, this is due to the nature of data within the URI Template specification. It is possible to render and match more complex data structures with URI Templates (see `the URI Template RFC <http://tools.ietf.org/html/rfc6570>`_ for a sense of how URI Templates can be used in more advanced ways). Simple values will only usually require the use of the ``Route.atom`` lens, but see :doc:`/router/examples` for some useful samples of using URI Templates to enable more powerful routing.

Summary
-------

We've seen the simple approach to extracting data from matched routes, using simple lens based techniques.

.. code-block:: fsharp

   open Freya.Router

   // Lens for extracting string values from a matched route
   Route.atom : string -> PLens<FreyaState, string>

   // Lens for extracting string list values from a matched route
   Route.list : string -> PLens<FreyaState, string list>

   // Lens for extracting string pair values from a matched route
   Route.keys : string -> PLens<FreyaState, (string * string) list>
