Routes
======

The URI Template router is based on mapping requests to ``Pipeline`` functions -- see :doc:`/reference/libraries/core/pipelines` if you're not familiar with the Freya pipeline concept. It maps the request by matching the method and the path and query against a URI Template.

Syntax
------

The URI Template Router defines a custom computation expression (simpler and more limited than the ``freya`` computation expression. The computation expression uses a custom operation to let you define routes in a simple, expressive and typed way. The computation expression is named ``freyaUriTemplateRouter`` but is also aliased to ``freyaRouter`` for convenience!

Definitions
-----------

Routes are defined by specifying a requirement for the HTTP method (or verb), a requirement for the path, and the ``Pipeline`` function to call if the route is matched. Route matching effectively happens in definition order precedence, although the router internally converts the route definitions to an optimised trie for performance reasons.

Strongly typed values are used for the requirements, using types taken from the :doc:`/reference/libraries/types/index` libraries, in this case **Freya.Types.Http** and **Freya.Types.Uri.Template**, as well as types from **Freya.Routers.Uri.Template**. Here's an annotated example of setting up a router, including opening appropriate modules/namespace:

.. code-block:: fsharp

   open Freya.Core
   open Freya.Routers.Uri.Template
   open Freya.Types.Http
   open Freya.Types.Uri.Template

   // Handlers
   
   let handlerA =
       freya {
           return Next }

   let handlerB =
       freya {
           return Next }

   // Routing

   let routeA =
       UriTemplate.parse "/a/{id}"

   let routeB =
       UriTemplate.parse "/b/{name}"

   let routes =
       freyaRouter {
           route Any routeA handlerA
           route (Methods [ GET; OPTIONS ]) routeB handlerB }

Breaking this down, the first thing you will notice is the definition of two handlers, which are simple ``Pipeline`` functions:

.. code-block:: fsharp
   
   let handlerA : Pipeline =
       freya {
           return Next }

   let handlerB : Pipeline =
       freya {
           return Next }

Next in the code you will see two simple URI Templates defined. URI Templates can be used to generate URIs given a template and suitable data -- Freya also allows to match on URI Templates, extracting the data to then be used. There are some caveats to matching URI Templates, but in general it is a good fit for many applications.

Two URI Templates are defined which will match the paths shown. The syntax for a match in this case is likely familiar -- a simple match, capturing the braced terms (e.g. ``{id}``).

.. code-block:: fsharp

   let routeA : UriTemplate =
       UriTemplate.parse "/a/{id}"

   let routeB : UriTemplate =
       UriTemplate.parse "/b/{name}"

Finally the router itself with the routes defined. The routes are defined using the ``route`` keyword in the computation expression. This takes three arguments:

* A ``UriTemplateRouteMethod``, which may be ``All`` -- matching any method, or ``Methods`` which takes a list of ``Method`` values which are allowed. In the example, the first route will match any method, the second only GET or OPTIONS requests.
* A ``UriTemplate`` which will be matched against the request path and query.
* A ``Pipeline`` which will be called if the method, and the path and query are matched.

.. code-block:: fsharp

   let routes =
       freyaRouter {
           route Any routeA handlerA
           route (Methods [ GET; OPTIONS ]) routeB handlerB }

The router will call the ``Pipeline`` function of the first matched route. If no route matches, no pipeline will be called.

Type Inference
--------------

Freya 3.0 introduced a more extensive use of statically resolved type parameters (don't worry if these are not familiar) to give more concise and flexible APIs. One of the places where this is used is in this computation expression. Rather than only taking a literal ``UriTemplateRouteMethod`` as the first parameter, the ``route`` function can actually take any value which has a static ``UriTemplateRouteMethod`` member. By default, this is defined for a few different types, all of which can be used interchangeably. That means that the folloing are all valid routes:

.. code-block:: fsharp

   let routes =
       freyaRouter {
           route Any routeA handlerA // Any
           route (Methods [ GET; POST ]) routeA handlerA // Methods
           route [ GET; POST ] routeA handlerA // Method list, inferred
           route GET routeA handlerA } // Method, inferred 

You will see that this potentially makes things clearer and more readable, allowing for simpler expressions of the same concepts. In addition to the Methods, both the template and the handler are also inferred - anything which has ``UriTemplate`` and anything which has ``Pipeline`` can be used. By default, this means that you can just use strings and they will be statically inferred as templates, and any Freya<_> function can be inferred as a Pipeline. This can mean that a previously more complex configuration can be made much simpler:

.. code-block:: fsharp

   let routes1 =
       freyaRouter {
           route (Methods [ GET ]) (UriTemplate.parse "/hello") handlerA }

   // is the same as...

   let routes2 =
       freyaRouter {
           route GET "/hello" handlerA }
           
Pipeline
--------

In earlier versions of Freya, it was neccessary to call an explicit ``toPipeline`` function to use a router as a pipeline. This is no longer needed in 3.0+ -- the router implements ``Pipeline`` and thus anything which expects to be able to infer a pipeline can accept a router.

URI Templates
-------------

in this example the URI Templates have been defined separately from the router. This could be done inline, saving space. However, it is often useful for multiple parts of a program to be able to refer to the URI Template as a first class item, so they are commonly defined outside of the router itself.

This becomes especially useful when you wish to return the URI of a resource as part of a response. You can use the same URI Template for routing and generating linking URIs, which prevents the two ever becoming unsynchronised, using the typed approach to prevent a class of error.
