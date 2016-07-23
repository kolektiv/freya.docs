Machines
========

Freya provides powerful ways to interact with the web which are safe, expressive, and relatively low-level. However, Freya also provides some higher level abstractions to enable more effective and concise programming when interacting with complex standards.

Machines are one way of approaching this, and provide a different model of web programming, based around decision trees and declarative programming (don't worry -- this sounds complex and scary, but it isn't!)

Understanding the way that Machines are built and used helps make the most of this powerful Freya feature, and the design of Machines is covered in some depth as a guide:

* :doc:`/topics/design/machines` -- design and implementation of a theoretical Machine.

Machine libraries follow conventions to help distinguish available Machines and extensions:

* :doc:`conventions` -- naming conventions for Freya Machine (and extension) libraries.

Freya currently includes one machine (with extensions) for working with HTTP -- additions may be made in future for complementary protocols, etc.

* :doc:`http/index` -- a Freya Machine for working with HTTP in a safe and semantically correct way, based on a purely functional declarative approach.

.. toctree::
   :hidden:

   conventions
   http/index
