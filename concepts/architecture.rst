Monoliths vs. Stacks
====================

Reusability
-----------

Freya is designed with reusability in mind. We want to make sure that the code in Freya can be reused as much as possible, and we want to make sure that other code can also work well with Freya. Our adherence to :doc:`/concepts/owin` and our efforts to provide neat functional bridges to OWIN compatible concepts hopefully gains us a lot in terms of reuse potential, but we can do more.

Monoliths
---------

One of the common barriers to reuse in projects such as Freya is the implementation as a single unifying framework or monolith. This often makes it difficult to use just a part of the framework, or to mix and match parts from multiple frameworks. For example, how easy is it to use just the routing from ASP.NET? How easy is it generally to pick features from multiple frameworks and make them play nicely? With Freya, we've tried something else.

Stacks
------

In Freya, the project is built as multiple sets of libraries, starting with a low level abstraction on top of an OWIN compatible server, and then adding functionality and abstractions, building on lower levels. This effectively means that you can mix and match Freya components and your own implementations (or other folks implementations).

You can choose to use just the core abstractions (:doc:`/core/index` - perhaps with :doc:`/types-and-lenses/index` to make life easier), or add the libraries providing a powerful :doc:`/router/index`, or the :doc:`/machine/index` abstraction. As we build more of Freya, we'll be making more libraries which complement each other well and work coherently, while allowing people to swap out abstractions as they move up the stack.

This might not be obvious yet, but hopefully as you use Freya you'll like the approach we've taken.
