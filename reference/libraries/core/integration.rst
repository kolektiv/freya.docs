Integration
===========

As previously detailed, Freya is built on the OWIN open standard for interoperability between .NET web servers and web frameworks (or stacks).

.. attention::

   GitHub issues will need updating.

You should be able to use Freya with any OWIN compatible web server -- if you can't, please `raise an issue <https://github.com/freya-fs/issues>`_ and we'll help you however we can.

Functions
---------

For reference for integrating with specific servers, see the :doc:`/reference/integration/index` documentation. In a general sense however, it is usually only needed to use one function to bridge the Freya and OWIN worlds. We need to be able to turn a Freya function in to an OWIN Application Function, or AppFunc.

.. code-block:: fsharp

   let freyaSystem =
       freya {
          ... }

   let freyaAppFunc =
       OwinAppFunc.ofFreya freyaSystem

The ``OwinAppFunc`` module contains functions to take a ``Freya<'a>`` function and turn it in to an ``OwinAppFunc``. Note that the return value of the function converted will be discarded when it's run, as there is no use for it in this case (this does mean that any ``Freya<'a>`` function can be used).

.. hint::
   
   OWIN specifies signatures for middleware functions -- OwinMidFunc -- as well as application functions -- OwinAppFunc. The OwinMidFunc module contains useful functions, but it is currently considered experimental and is not yet documented.

Summary
-------

The basic conversion of a Freya function to an OWIN compatible function has been demonstrated.

.. code-block:: fsharp

   // Convert any Freya<_> function to an OwinAppFunc
   OwinAppFunc.ofFreya : Freya<_> -> OwinAppFunc
