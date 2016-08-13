OWIN
====

OWIN (Open Web Interface for .NET - see `owin.org <owin.org>`_) is a simple, community driven, standard interface between web servers and web applications. With a simple abstraction in place, people can concentrate on writing servers or applications, confident that they will run happily with a server/application that also supports the OWIN standard.

Interface
---------

The OWIN standard interface is not a particularly complex one, as it's designed to be a fairly "lowest common denominator" approach, with the aim of being applicable to the broadest possible ecosystem (so no relying on .NET features only accessible from specific languages, minority versions or frameworks, etc.)

The standard, in basic form, looks like this (straying in to C# for a moment):

.. code-block:: c#

   using AppFunc = Func<IDictionary<string, object>, Task>;

In effect it's simply saying "I'm going to give you a dictionary containing request and response data. You muck about with that, and return me a Task when you're done with it." The server is then responsible for taking the eventual dictionary (called the *Environment*) and making sure that it gets turned in to a valid HTTP response, and sent back to the client.

Data
----

The obvious question now is "Where's the request and response data?" Well, it's in the *Environment*, boxed up with string keys. Some of the values are boxed strings, some boxed dictionaries, some boxed streams... It isn't elegant, but it is workable, and making it this low level is one of the only ways to give an interface that multiple languages can actually work with.

As an example, the key **owin.RequestPath** in the *Environment* will get (or set, as it's just a dumb dictionary) the path of the request. The key **owin.ResponseBody** is the stream used to write the response body, and so on.

As you'll note -- if you've got an F# hat on (our merchandise store is coming, we assure you) -- this is all based on mutation of the *Environment*. That's not a lovely thing to see in F#, and Freya does quite a lot of work to tidy this up (or at least hide it where you can pretend it doesn't happen). You can see more about how it does so in the Core Reference section -- see :doc:`/reference/libraries/core/index`.

Servers
-------

As you can see from the simplicity of the interface (known as the *AppFunc* or *OwinAppFunc*), there's not much that a library needs to be able to do to work with a compatible OWIN server. Different OWIN servers, however, have different ways of asking for that *AppFunc*, so you may need to peruse the documentation for your specific server.

In the examples and tutorials we'll see later, we'll often use the `Katana <https://katanaproject.codeplex.com/>`_ server, and we'll show how simple it is to host Freya using that. If you're using something else, it may be documented -- see :doc:`/reference/integration/servers`. If not, feel free to either ask for guidance or even submit documentation -- see :doc:`/meta/contributing`!

