Katana
======

It is very simple to integrate Freya applications with the Katana server, especially using the SelfHost options. A simple example is show below:

.. code-block:: fsharp

   // Freya

   open Freya.Core

   let application =
      freya {
         ... }

   let owinApplication =
       OwinAppFunc.ofFreya application

   // Katana

   open Microsoft.Hosting

   type Application () =
       member __.Configuration () =
           owinApplication

   // Main

   [<EntryPoint>]
   let main _ =
       let _ = WebApp.Start<Application> ("http://localhost:8080")
       let _ = System.Console.ReadLine ()
       0

This will give a Katana based server running Freya-delivered content in a console application.
