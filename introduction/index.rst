Introduction
============

Overview
--------

`Freya <http://freya.io>`_ is a functional-style web programming stack for F#.
Freya builds on top of the `Open Web Interface for .NET <http://owin.org/>`_
(OWIN) and provides several layers of abstraction, each useful in its own right
for building web applications.

Freya allows you to work in a very simple way, just reading data from requests
and writing data to responses. You can take a step up the abstraction ladder and
start to read and write data in a strongly typed way, adding some compile-time
safety to your interactions with HTTP.

Freya.Core
    Provides an implementation of OWIN using a functional style. Rather than
    allowing direct mutation of the OWIN environment dictionary, Freya.Core
    provides functional properties, or lenses, that provide a means of accessing
    and updating values in the environment dictionary.

    Freya.Core also provides a pipleline composition model, which differs
    slightly from the standard OWIN Russian-doll model. Freya will work with
    either style.

    Where Freya differs from standard OWIN implementations,
    Freya.Core provides adapter functions to bridge the gap, either allowing
    OWIN apps and middleware to be used within Freya or allowing Freya apps to
    work as standard OWIN apps and middleware. (If these terms are unfamiliar,
    see the section on :doc:`OWIN <../guides/concepts/owin>`.)

    Read more in the :doc:`Core guide <../guides/core/index>`.

Freya.Lenses.*
    Freya.Core retains the untyped nature of the OWIN environment dictionary.
    Freya.Lenses provides types around the various HTTP and related RFCs through
    the use of the `Arachne <https://github.com/freya-fs/arachne>`_ lens library.
    The Freya.Lenses.* libraries extend the typed Arachne lenses to better work
    with OWIN and Freya.Core.

    Read more in the :doc:`Types and Lenses guide <../guides/types-and-lenses/index>`.

Freya.Router
    Web applications typically dispatch requests to handlers by means of URI
    pattern matching. Freya.Router provides a dispatching mechanism that uses
    the `URI Templates RFC <http://tools.ietf.org/html/rfc6570>`_ to match
    request URLs and invoke relevant handlers.

    Read more in the :doc:`Router guide <../guides/router/index>`.

Freya.Machine
    If you're used to MVC-style frameworks -- ASP.NET MVC, Rails, etc. --
    Freya.Machine may feel a bit peculiar. However, that peculiarity comes with
    the ability to work more accurately with HTTP and find the right level of
    abstraction for the problem you're trying to solve.

    Freya.Machine provides a state machine approach to handling HTTP requests, no
    longer explicitly handling a request but simply answering questions. Does this
    resource exist? Can the client handle the representations I can send? Does the
    client have an older version than I do? Okay, send the data. Hooray, 200 OK!

    The machine abstraction avoids the typical problems of having to re-implement
    HTTP in each of your applications like you do with MVC-style frameworks. Wonder
    which HTTP method to use, how to group your handlers, or which headers you should
    send? The machine specifies all these things for you based on the implicit
    state machine defined in the HTTP RFCs. You need merely tell Freya which pieces
    you wish to use. ::

        freya {
            using http
            using httpCors
            // ...
        }

    Read more in the :doc:`Machine guide <../guides/machine/index>`.

You can combine the tools that Freya provides and create your own abstractions, too.
Create reusable functions that you can apply to provide fine grained security,
new standards support, or anything else you like. Freya aims to be a great toolbox,
as well as a great tool.

Deployment
----------

Since Freya is built on the `OWIN <http://owin.org>`_ standard, it can run on any
OWIN-compatible server. That means Freya applications can run self-hosted, on IIS,
on Azure, Windows or Linux, etc. You can get Freya running easily, and integrate it
well with existing technology when you need. 

Focus
-----

Most of the effort building Freya was applied to making it a tool for accurate API
design and construction. It doesn't currently include a templating language or
client-side framework. Those things could absolutely be built on Freya (and perhaps
you will?) but they're not there today. We'd love to see what you can build on Freya,
though. If you start to take it off in new directions, please show us!

Status
------

Freya recently went 1.0! That means we're pretty confident that people can start
using it for "real" things -- and that we're happy to try and help with that. We've
got big plans and ideas for the post-1.0 roadmap too (great introspection tools,
hypermedia abstractions, and more).

If you want to keep an eye on what's coming up, we track everything in the open on
GitHub. See our `milestones <https://github.com/freya-fs/freya/milestones>`_.
