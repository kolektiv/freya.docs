Values
======

As seen in :doc:`/reference/libraries/routers/uri-template/routes`, it's quite simple to map routes (the combination of method and path specification) to ``Pipeline`` functions. Once a route is matched however, you will likely need the handler to have access to the data that was matched as part of the URI Template.

Here is one of the URI Templates from the previous section:

.. code-block:: fsharp

    let routeA =
        UriTemplate.parse "/a/{id}"

You can see that if this route has been matched, a value for ``{id}`` should now exist. Freya aims for a consistent model of programming throughout, and so the approach to accessing this data is aligned to accessing any other data -- it is considered to be part of the state.

Optics
------

As with accessing data from the request or response, accessing data from the route requires the use of a suitable set of optics. These are provided under the ``Route`` module. Here's a function which will return the ``{id}`` value from the  example:

.. code-block:: fsharp

   let readId =
       freya {
           return! Freya.Optic.get (Route.atom_ "id") }

There are several things to note here. The first is that route optics are prisms. You can't statically be sure that a value will be present (even though intuitively you can know that it will be in certain contexts), so the optics must be prisms. Additionally, route optics are parameterised -- as shown, it is passed the name of the value sought, in this case "id").

Another key point to note is that there are three available prisms available (all within the ``Route`` module). In this case, ``Route.atom_`` is used, but ``Route.list_`` and ``Route.keys_`` are also available. Briefly, this is due to the nature of data within the URI Template specification. It is possible to render and match more complex data structures with URI Templates (see `the URI Template RFC <http://tools.ietf.org/html/rfc6570>`_ for a sense of how URI Templates can be used in more advanced ways). Simple values will only usually require the use of the ``Route.atom_`` optic, but much more is possible -- see the relevant recipes for more information.

Summary
-------

Techniques for accessing matched values using optics have been shown.

.. code-block:: fsharp

   open Freya.Routers.Uri.Template

   // Optic for extracting string values from a matched route
   Route.atom_ : string -> Prism<State, string>

   // Optic for extracting string list values from a matched route
   Route.list_ : string -> Prism<State, string list>

   // Optic for extracting string pair values from a matched route
   Route.keys_ : string -> Prism<State, (string * string) list>

Recipes
-------

See also:

* :doc:`/recipes/routing` -- as well as the library reference, a growing collection of routing recipes covering various techniques is maintained.

