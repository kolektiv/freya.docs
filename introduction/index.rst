Introduction
============

Overview
--------

Freya is a bit different. If you're used to MVC-style frameworks -- ASP.NET MVC, Rails, those kinds of things -- it'll probably feel a bit peculiar to start with. But with that peculiarity comes the ability to work more accurately with HTTP, and to find the right level of abstraction for the problem you're trying to solve (or so we hope).

With Freya, you can work in a very simple way -- just reading data from requests and writing data to responses. You can take a step up the abstraction ladder and start to read and write data in a strongly typed way, adding some compile-time safety to your interactions with HTTP.

You can take it further, working with a state-machine style approach to handling HTTP requests -- no longer explicitly handling a request, but simply answering questions. Does this resource exist? Can the client handle the representations I can send? Does the client have an older version than I do? Ok - let's send the data. Hooray, 200 OK!

You can combine the tools that Freya provides and create your own abstractions too. Create reusable functions that you can apply to provide fine grained security, new standards support, or anything else you like. Freya aims to be a great toolbox as well as a great tool.

Focus
-----

At the moment, Freya has had most thought applied to it as a tool for accurate API design and construction. It doesn't currently include a templating language, or client-side framework. Those things could absolutely be built on Freya (and perhaps you will?) but they're not there today. We'd love to see what you can build on Freya though -- if you start to take it off in new directions, please show us!

Status
------

At the moment, Freya is in prerelease, and nearing 1.0 rapidly. We've got big plans and ideas for the post-1.0 roadmap (great introspection tools, hypermedia abstractions, and more) but we want to get 1.0 out there for people to play with.

If you want to track us hitting 1.0, follow `this GitHub issue <https://github.com/freya-fs/freya/issues/95>`_ -- it won't be long now!

We think Freya is usable and useful right now though, so please jump in and have a go, especially as we get the first wave of documentation written -- we'd love to know what's clear, what's hazy, and what's just word soup (it isn't always easy to write about things you know inside out!)
