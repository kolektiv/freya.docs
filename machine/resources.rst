Defining Resources
==================

Freya libraries always try and accurately reflect the underlying design the web where they can, and Machine is no exception. The fundamental building block of HTTP and the Machine library is the `Resource`.

Syntax
------

Much like the :doc:`/router/index` library, the Machine library introduces a computation expression for defining resources, with an associated set of custom operations to make the code clearer and easier to scan. Each computation expression represents a single resource, and can be compiled to a ``FreyaPipeline``, much like the routers we have seen already. This means that resources defined with the Machine library can be mounted within a ``Freya.Router`` router like any other ``FreyaPipeline`` function.

Let's have a first look at a very simple resource. We'll look at some of the elements here, while some we'll expand upon in the following sections.

.. code-block:: fsharp

   open Arachne.Http
   open Freya.Core
   open Freya.Machine
   open Freya.Machine.Extensions.Http

   // Properties

   let mediaTypes =
       freya {
           return [ MediaType.Text ] }
                
   let methods =
       freya {
           return [ GET ] }

   // Decisions

   let available =
       freya {
           return true }
   
   // Handlers

   let content _ =
       freya {
           return {
               Description =
                 { Charset = Some Charset.Utf8
                   Encodings = None
                   MediaType = Some MediaType.Text
                   Languages = None }
               Data = "Hello World!"B } }

   // Resource

   let helloWorld =
       freyaMachine {
           using http
           mediaTypesSupported mediaTypes
           methodsSupported methods
           serviceAvailable available
           handleOk content } |> FreyaMachine.toPipeline

This is a very simple resource, although we have split out configuration which we could have declared inline. This is common, as we will often find that properties of resources are shared by multiple resources, and we will often end up composing sets of properties to form a specific resource -- we'll look at that in more detail in a later section.

We'll have a look at the code here in order.

First we come to some properties, ``mediaTypes`` and ``methods``. It should be fairly clear what we're saying here, that the Media Types supported are defined by a list, in this case only allowing "text/plain" (MediaType.Text is a pre-defined instance of the typed form of "text/plain"), and the Methods supported are just ``GET`` for this resource.

Next we come to a decision, ``available``. In this case this is a very simple decision -- is our resource currently available? We'll say yes (``true``)!

Following on, we define a handler, called ``content``. This will seem rather complicated, but for now we can ignore all of this except the last line, specifying that the data is `"Hello World!"` (the ``B`` suffix indicates that we wish the string to be an array of bytes). For now, we will skip over the meaning of the description section -- we'll look at this in particular when we come to discuss content negotiation. This function will write the contents of the data element to the body of the response.

.. note::

   Note that all of the functions here, whether decisions, properties, etc. are `freya` functions -- they are evaluated at runtime, not at compile time, meaning that the values do not have to be static.

   You could choose to expose different Media Types based on some aspect of the request, or only expose that certain methods are available to authorized clients (for example). There is a lot of power in this dynamic nature, although it is not common to `need` it in general Freya usage!

Finally, we define our actual resource, assigning the values we've already seen to the resource. This should be self-explanatory, with the exception of the first line ``using http``. We'll look more at what that means in the :doc:`/machine/extension` section.

Elements
--------

In this example we can see examples of several different elements of resource definition. Each resource may be defined with any number of these elements (there are many to select from) although most resources only require a few, as the defaults -- which will be used when an element is not defined -- are usually sufficient for many situations.

Properties
``````````

In the example we can see above, there are two properties, ``mediaTypesSupported`` and ``methodsSupported``. Property elements are always ``Freya<'a>`` functions, where the type of ``'a`` is determined by the property. In this case, it is a list of media types, and a list of methods respectively, but each property element requires a specific property value. (See :doc:`/machine/standard-extensions/index` for a reference covering all elements defined as part of the Machines that ship with Freya, and see :doc:`/machine/extensions` for more on how elements are provided by Machine Extensions).

These properties are evaluated at runtime, and so represent dynamic configuration. If you wanted to, you could define the ``mediaTypesSupported`` property to allow different media types for different clients, or different levels of access, for example.

Decisions
`````````

Our next element, ``serviceAvailable``, is an example of a decision. Decisions are always of type ``Freya<bool>``, returning true or false. In this case we return true to say that our service (or resource) is available. Of course, this example is a little contrived -- the default function for ``serviceAvailable``, if we didn't define it ourselves, returns ``true`` anyway, so this is not changing anything, but it's a simple illustration.

Decisions are a big part of Machine. They drive a lot of the underlying logic of handling an HTTP request. You will probably find yourself implementing decisions like ``allowed``, ``authorized`` or ``exists`` quite regularly -- simply defining these functions enables Machine to properly handle access control, or handle whether to return a resource or a 40x.

Handlers
````````

In our example above, ``handleOk`` is an example of a handler. Handler functions are called to get the content (and do any other processing on the request if required) that should be returned to the client -- if any -- right at the end of processing the resource.

Different handler functions map to different HTTP return types -- so ``handleOk`` will be called (if defined) to get the content that should be returned to the client if the result of the request should be a `200 OK` response. If we wanted to send some specific content to a client when a resource doesn't exist (we'll have seen ``exists`` return ``false``), then we could define ``handleNotFound`` -- although we can rely on the default handler doing the right thing if we wish.
