Introduction
============

Overview
--------

Freya is a bit different. If you're used to MVC-style frameworks -- ASP.NET MVC, Rails, etc. -- it'll probably feel a bit peculiar, but with that peculiarity comes the ability to work more accurately with HTTP and find the right level of abstraction for the problem you're trying to solve. (Or so we hope.)

Freya allows you to work in a very simple way, just reading data from requests and writing data to responses. You can take a step up the abstraction ladder and start to read and write data in a strongly typed way, adding some compile-time safety to your interactions with HTTP.

Freya also provides a state-machine style approach to handling HTTP requests, no longer explicitly handling a request but simply answering questions. Does this resource exist? Can the client handle the representations I can send? Does the client have an older version than I do? Ok - let's send the data. Hooray, 200 OK!

You can combine the tools that Freya provides and create your own abstractions, too. Create reusable functions that you can apply to provide fine grained security, new standards support, or anything else you like. Freya aims to be a great toolbox, as well as a great tool.

Deployment
----------

Freya is built on the `OWIN <http://owin.org>`_ standard, taking great care to remain compatible with open standards. This means that it can run on any OWIN compatible server, whether that's self-hosted, on IIS, on Azure, Windows or Linux, etc. You can get Freya running easily, and integrate it well with existing technology when you need to. 

Focus
-----

Most of the effort building Freya was applied to making it as a tool for accurate API design and construction. It doesn't currently include a templating language or client-side framework. Those things could absolutely be built on Freya (and perhaps you will?) but they're not there today. We'd love to see what you can build on Freya, though. If you start to take it off in new directions, please show us!

Status
------

Freya recently went 1.0! That means we're pretty confident that people can start using it for "real" things -- and that we're happy to try and help with that. We've got big plans and ideas for the post-1.0 roadmap too (great introspection tools, hypermedia abstractions, and more).

If you want to keep an eye on what's coming up, we track everything in the open on GitHub -- head over to our `milestones <https://github.com/freya-fs/freya/milestones>`_.
