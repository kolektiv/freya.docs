Integration with OWIN
=====================

As noted in the :doc:`/introduction/index`, Freya is built on the OWIN open standard for interoperability between .NET web servers and web frameworks (or stacks!)

You should be able to use Freya with any OWIN compatible web server -- if you can't, please `raise an issue <https://github.com/freya-fs/issues>`_ and we'll help you however we can. Most of our examples and tutorials that we should will use the Katana web server, an open source server from Microsoft, as it's easy to obtain (via your NuGet management mechanism of choice) and run -- you can fire up a server as a simple F# console app.

Functions
---------

We only really need one concept when it comes to integrating with an OWIN compatible server in a simple way -- the ability to give the server an OWIN AppFunc (see :doc:`/concepts/owin` if you're not familiar with this). All servers are likely to have some mechanism by which this can be used, so it's very easy to take a system built with Freya and expose it as an OWIN AppFunc:

.. code-block:: fsharp

   let freyaSystem =
       freya {
          ... }

   let freyaAppFunc =
       OwinAppFunc.ofFreya freyaSystem

The ``OwinAppFunc`` module contains functions to take a ``Freya<'a>`` function and turn it in to an ``OwinAppFunc``. Note that the return value of the function converted will be discarded when it's run, as there is no use for it in this case (this does mean that any ``Freya<'a>`` function can be used).

.. hint::
   
   OWIN specifies sigantures for middleware functions (``OwinMidFunc``) as well as application functions (``OwinAppFunc``). The ``OwinMidFunc`` module contains useful functions, but it is currently considered experimental and is not yet documented.


Katana
------

As mentioned, we will assume the use of the Katana server (though Freya should work identically in operation with any OWIN compatible server). It's useful to provide a quick guide to how to get a simple Katana server up and running though -- here's a small example of getting a quick console server running, which you may find useful when experimenting with Freya:

.. code-block:: fsharp

   open Freya.Core
   open Microsoft.Hosting

   // Freya

   let application =
      freya {
         ... }

   // Katana

   type Application () =
       member __.Configuration () =
           OwinAppFunc.ofFreya application

   // Main

   [<EntryPoint>]
   let main _ =
       let _ = WebApp.Start<Application> ("http://localhost:8080")
       let _ = System.Console.ReadLine ()
       0

This will give you a quick Katana-based server running Freya-delivered content in a console application. Simple!

.. note::

   We would welcome pull requests to this document providing quick-starts for other servers or hosting approaches. Alternatively please feel free to raise an issue if there's something you're struggling with and would like help.

Summary
-------

We've covered a very quick and basic way to get started with serving Freya content using a method which should work with OWIN compatible servers, and also shown a quick-start for Katana-based hosting.

.. code-block:: fsharp

   // Convert any Freya<'a> function to an OwinAppFunc
   OwinAppFunc.ofFreya : Freya<_> -> OwinAppFunc

   // Convert any OwinAppFunc to a Freya<unit> function
   OwinAppFunc.toFreya : OwinAppFunc -> Freya<unit>
