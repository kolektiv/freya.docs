Kestrel
=======

The Kestrel server is the newer HTTP server built by Microsoft. The **Freya.Polyfills.Kestrel** polyfill should be used whenever you are taking one of these hosting approaches. The polyfill should be used by inserting it in front of your Freya application in a pipeline composition:

.. code-block:: fsharp

   open Freya.Core
   open Freya.Core.Operators
   open Freya.Polyfills.Kestrel

   // Some pre-existing Freya pipeline (could be a router, function, etc.)
   let myApp =
       freya {
           ...
           return Next }

   // Compose a polyfill with the pre-existing pipeline
   let composedApp =
           Polyfill.kestrel
       >?= myApp

   // Use as normal...
   let owinApp =
       OwinAppFunc.ofFreya composedApp

   ...

The polyfill currently adds additional data to the OWIN environment which enables routing to work more accurately (giving routers access to the raw, encoded form of the path and query).
   

