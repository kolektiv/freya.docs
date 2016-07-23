HTTP
====

Freya lets you work with HTTP implicitly, using the typed but low-level tools found in :doc:`/reference/libraries/core/index` combined with types and optics, such as the provided :doc:`/reference/libraries/optics/index`. These are expressive and effective, but provide little in the way of structure.

HTTP is a fairly involved set of standards, and to properly implement the semantics of HTTP is not a trivial matter. Dealing with the correct logical approach to negotiating content types, working out the right approach to cache control and correctly expressing the state of the underlying resources, can be quite a lot to design correctly.

**Freya.Machines.Http** is designed to solve this. The Machine lets you define just the properties of a resource you care about, and the Machine library handles the rest. You can specify as much or as little as you wish, all in a type safe manner, and be confident that HTTP semantics will remain consistent and correct. The following sections give an overview of the design and usage of the HTTP Machine.

.. toctree::
   
   configuration
   
