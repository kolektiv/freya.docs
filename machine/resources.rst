Defining Resources
==================

Freya libraries always try and accurately reflect the underlying design the web where they can, and Machine is no exception. The fundamental building block of HTTP and the Machine library is the `Resource`.

Syntax
------

Much like the :doc:`/router/index` library, the Machine library introduces a computation expression for defining routes, with an associated set of custom operations to make the code clearer and easier to scan. Each computation expression represents a single resource, and can be compiled to a ``FreyaPipeline``, much like the routers we have seen already. This means that resources defined with the Machine library can be mounted within a ``Freya.Router`` router like any other ``FreyaPipeline`` function.

Let's have a first look at a very simple resource. We'll look at some of the properties here, while some we'll expand upon in the following sections.

.. code-block:: fsharp

   open Arachne.Http
   open Freya.Core
   open Freya.Machine
   open Freya.Machine.Extensions.Http
                
   // Handler

   let content _ =
       freya {
           return {
               Description =
                 { Charset = Some Charset.Utf8
                   Encodings = None
                   MediaType = Some MediaType.Text
                   Languages = None }
               Data = "Hello World!"B } }
   
   // Configuration

   let mediaTypes =
       freya {
           return [ MediaType.Text ] }
                
   let methods =
       freya {
           return [ GET ] }

   // Resource

   let helloWorld =
       freyaMachine {
           using http
           mediaTypesSupported mediaTypes
           methodsSupported methods
           handleOk content } |> FreyaMachine.toPipeline

This is a very simple resource, although we have split out configuration which we could have declared inline. This is common, as we will often find that properties of resources are shared by multiple resources, and we will often end up composing sets of properties to form a specific resource -- we'll look at that in more detail in a later section.

We'll have a look at the code here in order.

First of all, we define our content function. This will seem very complicated, but for now we can ignore all of this except the last line, specifying that the data is `"Hello World!"` (the ``B`` suffix indicates that we wish the string to be an array of bytes). For now, we will skip over the meaning of the description section -- we'll look at this in particular when we come to discuss content negotiation. This function will write the contents of the data element to the body of the response.

Next we come to some configuration items, ``mediaTypes`` and ``methods``. It should be fairly clear what we're saying here, that the Media Types supported are defined by a list, in this case only allowing "text/plain" (MediaType.Text is a prebuilt instance of the typed form of "text/plain"), and the Methods supported are just ``GET`` for this resource.

.. note::

   Note that the functions here for configuring Media Types and Methods are `freya` functions -- they are evaluated at runtime, not at compile time, meaning that these values do not have to be static.

   You could choose to expose different Media Types based on some property of the request, or only expose that certain methods are avilable to authorized clients (for example). There is a lot of power in this dynamic configuration, although it is not common to `need` it in general Freya usage!

Finally, we define our actual resource, assigning the values we've already seen to `properties` of the resource. This should be self-explanatory, with the exception of the first line ``using http``. We'll look more at what that means in the :doc:`/machine/extension` section.
