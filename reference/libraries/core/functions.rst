Functions
=========

The main abstraction in Freya is an abstraction over the OWIN state -- see :doc:`/topics/standards/owin`. As this is functional programming, you need to pass the state around to functions which require it (and return it from functions which have modified it).

Doing such a common thing manually would become a chore very quickly. The solution is the ``Freya<'a>`` function, which has the following type:

.. code-block:: fsharp

   type Freya<'a> =
       State -> Async<'a * State>

In the case of the Hopac variant of the Freya stack -- see :doc:`/topics/general/hopac` -- the type is instead defined as:

.. code-block:: fsharp

   type Freya<'a> =
       State -> Job<'a * State>

The ``State`` type is a wrapper around the OWIN **Environment** - it holds a few additional data structures, the relevant part is the OWIN Environment.

A function of type ``Freya<'a>`` takes the state, and asynchronously (or as a Hopac Job) returns a value of type ``'a`` and the state. The concurrency options are made available here as one or other of these abstractions is commonly used in web programming. This makes it easy to integrate Freya with existing libraries and software.

Syntax
------

The ``Freya<'a>`` type would be awkward to implement manually throughout applications, and F# allows for useful syntactic extension in the form of computation expressions.

A ``freya`` computation expression is provided to make it simpler to write code using Freya, and the functions available throughout Freya are easily usable with this syntax.

.. note::

   This syntax is not compulsory -- it is possible to use a more operator-based style when writing Freya code if you prefer.

The computation expression syntax looks like this:

.. code-block:: fsharp

   let double x =
       freya {
           return x * 2 }

The signature of this function is ``int -> Freya<int>``, showing a simple way to work with functions which now have the State threaded through them. Of course, this function doesn't do anything with the State that it has available -- the next section shows how State can be used.

State
-----

The important part of the Freya approach to programming is to make programming with state transparent and functional. The most basic way to use information contained within the state is to get the ``State`` instance and to use some element of it, as seen below:

.. code-block:: fsharp

   let readPath =
       freya {
           let! state = Freya.Optic.get id_
           return state.Environment.["owin.RequestPath"] :?> string }

The first part of this function gets the ``State`` instance, and the second part extracts some data from it and returns it. Note that the first line uses a ``Freya<_>`` function -- requiring the use of the ``let!`` syntax for computation expressions.

This very basic usage works, but is not a compelling approach -- it can be improved significantly, as detailed in the next section:

* :doc:`optics` -- using Optics in Freya for safe, typed data manipulation. 

Summary
-------

The basic abstraction of Freya has been introduced, along with the Freya computation expression.

.. code-block:: fsharp

                
   // Freya<'a> (State is described)
   type Freya<'a> =
       State -> Async<'a * State> // (or State -> Job<'a * State>)

   // Freya computation expression
   freya { ... }
