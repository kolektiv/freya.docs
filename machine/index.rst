Machine
=======

The parts of Freya that we've looked at so far in this Guide, such as the :doc:`/core/index` and :doc:`/types-and-lenses/index` libraries, have provided some powerful ways to work directly with HTTP while retaining a reasonable level of control, type safety, and expressiveness.

However, HTTP is a fairly involved set of standards, and to properly implement the semantics of HTTP is not a trivial matter. Dealing with the correct logical approach to negotiating content types, working out the right approach to cache control and correctly expressing the state of the underlying resources, can be quite a lot to think about. Let's face it, what would be really welcome is a way to define resources and only implement the parts of HTTP that we care about, and leave a library to handle anything we haven't specified for us.

That's where ``Freya.Machine`` comes in. The Machine library lets you define just the properties of a resource you care about, and the Machine library handles the rest. You can specify as much or as little as you wish, all in a type safe manner, and be confident that HTTP semantics will remain consistent and correct.

.. toctree::
   :maxdepth: 2

   resources
   extension
   extensions/index
   advanced/index
