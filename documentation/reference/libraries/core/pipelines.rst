Pipelines
=========

Functional programming makes much of the ability to compose functions simply -- one of the joys of functional programming is being able to compose complex functionality from simpler parts with confidence.

The common ``Freya<'a>`` functions are quite simple to use together in various ways (the computation expression syntax being the most obvious -- calling another ``Freya<'a>`` function within a computation expression is as simple as using the ``let!`` or ``do!`` syntax. However, it turns out to be useful to introduce another building block, which makes it easier to build systems which compose at a more coarse-grained level.

Definition
----------

Freya introduces the concept of a ``Pipeline`` function, which is defined like this:

.. code-block:: fsharp

   type Pipeline =
       Freya<PipelineChoice>

    and PipelineChoice =
       | Next
       | Halt

It's simply a normal ``Freya<'a>`` function, where ``'a`` is constrained to be ``PipelineChoice``. This is used as a building block throughout various Freya libraries.

Composition
-----------

The simplest example of this is the basic composition of some ``Pipeline`` functions, composed. The ``Pipeline.compose`` function can be used to do this. This function has a basic logical model -- compositions of pipeline functions will halt when the first value of ``Halt`` is returned:

.. code-block:: fsharp

   let yes =
       freya {
           return Next }

   let no =
       freya {
           return Halt }

   // Both of these functions will be run
   Pipeline.compose yes no

   // Only the first of these functions will be run
   Pipeline.compose no yes

This becomes a useful technique when you want to stop processing a request in a certain situation -- for example, you may have written a function which should halt processing if the user making the request is not authorized.

Operators
---------

Freya always provides named functions for every piece of functionality, but some parts of libraries lend themselves well to the use of custom operators to make definition more concise and readable. As an alternative to the ``Pipeline.compose`` function, the infix syntax ``>?=`` is also available. This has the advantage that chains of composition become significantly simpler to read and write:

.. code-block:: fsharp

   open Freya.Core.Operators

   let functionComposed =
      Pipeline.compose (Pipeline.compose yes no) yes

   let operatorComposed =
      yes >?= no >?= yes

   // or if you prefer...

          yes
      >?= no
      >?= yes

It's a matter of taste and is definitely subjective -- but if you find the operator approach clearer, Freya makes them available.

Summary
-------

The ``Pipeline`` function concept has been defined, and a simple approach to composing them.

.. code-block:: fsharp

   // Types
                
   type Pipeline =
       Freya<PipelineChoice>

    and PipelineChoice =
       | Next
       | Halt

   // Composition

   Pipeline.compose : Pipeline -> Pipeline -> Pipeline

   // Composition (Operator)

   open Freya.Core.Operators

   (>?=) : Pipeline -> Pipeline -> Pipeline
