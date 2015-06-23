Composition with Pipelines
==========================

Functional programming makes much of the ability to compose functions simply -- one of the joys of functional programming is being able to compose complex functionality from simpler parts with confidence.

Our ``Freya<'a>`` functions are quite simple to use together in various ways (the computation expression syntax being the most obvious - calling another ``Freya<'a>`` function within a computation expression is as simple as using the ``let!`` or ``do!`` syntax. However, it turns out to be useful to introduce another building block, which makes it easier to build systems which compose at a more coarse-grained level.

Pipelines
---------

Freya introduces the concept of a ``FreyaPipeline`` function, which is defined like this:

.. code-block:: fsharp

   type FreyaPipeline =
       Freya<FreyaPipelineChoice>

   and FreyaPipelineChoice =
       | Next
       | Halt

It's simply a normal ``Freya<'a>`` function, where ``'a`` is constrained to be ``FreyaPipelineChoice``. We use this as a building block throughout various Freya libraries.

Composition
^^^^^^^^^^^

The simplest way we can use this is to define some ``FreyaPipeline`` functions, and then compose them. We can use the ``Freya.pipe`` function to do this, and this function has a little logic built in. In essence, compositions of ``FreyaPipeline`` functions will halt when the first of them returns ``Halt`` as a result. Here's an example:

.. code-block:: fsharp

   let yes =
       freya {
           return Next }

   let no =
       freya {
           return Halt }

   // Both of these functions will be run
   Freya.pipe yes no

   // Only the first of these functions will be run
   Freya.pipe no yes

This becomes a useful technique when we want to stop processing a request -- for example, we may have written a function which should halt processing if the user making the request is not authorized.

Operators
^^^^^^^^^

In Freya we do try to avoid any requirement to use custom operators, but in some places (composition being an obvious example) they are the most concise and readable way to express an operation. As an alternative to the ``Freya.pipe`` function, the infix syntax ``>?=`` is also available. This has the advantage that chains of composition become significantly simpler to read and write:

.. code-block:: fsharp

   open Freya.Core.Operators

   let functionComposed =
      Freya.pipe (Freya.pipe yes no) yes

   let operatorComposed =
      yes >?= no >?= yes

   // or if you prefer...

          yes
      >?= no
      >?= yes

It's a matter of taste and rather subjective, but if you find the operator approach clearer, Freya makes them available.

Summary
-------

We've covered the ``FreyaPipeline`` concept, and a simple approach to composing them. We'll see more parts of Freya that work with pipelines in later sections.

.. code-block:: fsharp

   // Types
                
   type FreyaPipeline =
       Freya<FreyaPipelineChoice>

   and FreyaPipelineChoice =
       | Next
       | Halt

   // Composition

   Freya.pipe : FreyaPipeline -> FreyaPipeline -> FreyaPipeline

   open Freya.Core.Operators

   (>?=) : FreyaPipeline -> FreyaPipeline -> FreyaPipeline
