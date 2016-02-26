Defining Routes
===============

The Freya router is based on mapping requests to ``FreyaPipeline`` functions (see :doc:`/core/pipelines` if you're not familiar with the Freya pipeline concept). It uses a custom computation expression (simpler and more limited than the common ``freya`` computation expression) tailored specifically for defining routing -- the ``freyaRouter`` computation expression.

Syntax
------

The computation expression uses a custom operation to let you define routes in a simple, typed way (the router is also extended by other Freya stack libraries, as you will see in other sections, but we will concentrate on the simple, common case here).

By default, we define routes by specifying a requirement for the HTTP method (or verb), a requirement for the path, and the ``FreyaPipeline`` function to call if the route is matched. Route matching effectively happens in definition order precedence, although the router internally converts the route definitions to an optimised trie, so this is not literally true!

We use strongly typed values for the requirements, using types taken from the :doc:`/types-and-optics/arachne` libraries, in this case ``Arachne.Http`` and ``Arachne.Uri.Template``, as well as types from ``Freya.Router``. Here's an annotated example of setting up a router, including opening appropriate modules:

.. code-block:: fsharp

   open Arachne.Http
   open Arachne.Uri.Template
   open Freya.Core
   open Freya.Router

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

There's quite a lot going on here, let's look at the parts in turn to break down what's going on. First of all, our two handlers are simply ``FreyaPipeline`` functions:

.. code-block:: fsharp
   
   let handlerA : FreyaPipeline =
       freya {
           return Next }

   let handlerB : FreyaPipeline =
       freya {
           return Next }

Next we come to the first critical aspect of routing -- the use of URI Templates. URI Templates are generally used to turn a template and some content in to a URI (or some part of a URI, as the specification does not actually specify that a rendered template must be a well-formed URI).

However, they can also be used for matching (with some caveats) and in Freya we use URI Templates for both matching and rendering (this will be useful in higher levels of abstraction, such as Hypermedia generation).

Here we define two URI Templates, which will match the paths shown. The syntax for a match is quite obvious here -- we're doing a simple match, capturing the braced terms (e.g. ``{id}``).

.. code-block:: fsharp

   let routeA : UriTemplate =
       UriTemplate.parse "/a/{id}"

   let routeB : UriTemplate =
       UriTemplate.parse "/b/{name}"

Finally, we define the routes themselves, using the ``route`` keyword in the computation expression. This takes three arguments:

* A ``FreyaRouteMethod``, which may be ``All`` -- matching any method, or ``Methods`` which takes a list of ``Method`` values which are allowed. In the example, the first route will match any method, the second only GET or OPTIONS requests.
* A ``UriTemplate`` which will be matched against the request path.
* A ``FreyaPipeline`` which will be called if the method and the path are matched.

.. code-block:: fsharp

   let routes =
       freyaRouter {
           route Any routeA handlerA
           route (Methods [ GET; OPTIONS ]) routeB handlerB }

The router will call the ``FreyaPipeline`` function of the first matched route. If no route matches, no pipeline will be called.

Type Inference
--------------

Freya 3.0 introduced a more extensive use of static type inference to give more concise and flexible APIs. One of the places where this is used is in the ``freyaRouter`` computation expression. Rather than only taking a literal ``FreyaRouteMethod`` as the first parameter, the ``route`` function can actually take any value which has a static ``FreyaRouteMethod`` member. By default, this is defined for a few different types, all of which can be used interchangeably. That means that the folloing are all valid routes:

.. code-block:: fsharp

   let routes =
       freyaRouter {
           route Any routeA handlerA // Any
           route (Methods [ GET; POST ]) routeA handlerA // Methods
           route [ GET; POST ] routeA handlerA // Method list, inferred
           route GET routeA handlerA } // Method, inferred 

As we can see, this can make things more concise and more readable! In addition to the Methods, both the template and the handler are also inferred - anything which has ``UriTemplate`` and anything which has ``FreyaPipeline`` can be used. By default, this means that you can just use strings and they will be statically inferred as templates, and any Freya<_> function can be inferred as a Pipeline. This can mean that a previously more complex configuration can be made much simpler:

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

In earlier versions of Freya, it was neccessary to call an explicit ``toPipeline`` function to use a Router as a pipeline. This is no longer needed in 3.0+. (FreyaRouter implements ``FreyaPipeline`` and thus anything which expects to be able to infer a pipeline can accept a FreyaRouter).

URI Templates
-------------

In our router example here, we have defined our URI Templates separately from the router, and referred to them. We could of course do this inline, saving space. However, it is often useful for multiple parts of a program to be able to refer to the URI Template as a first class item, so we commonly define them outside of the router itself.

This becomes especially useful when you wish to return the URI of a resource as part of a response. You can use the same URI Template for routing and generating linking URIs, which prevents the two ever becoming unsynchronised, using the typed approach to prevent a class of error.

Of course, to do so we will need to know how to work with URI Template data, both for rendering, and for interpreting routing matches in our handlers, which we will see in the next section.
