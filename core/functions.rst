Working with Freya Functions
============================

Abstraction
-----------

The main abstraction in Freya is the abstraction over the OWIN state (see our quick guide to :doc:`/concepts/owin` if you're not familiar with this). As we're programming functionally, we need to pass the state around to functions which require it (and pass it back from functions which have modified it), but doing so manually, when it'll be the most common thing we do in Freya, would become a chore very quickly.

For this reason, we define the concept of a ``Freya<'a>`` function, which has the following signature:

.. code-block:: fsharp

   type Freya<'a> = FreyaState -> Async<'a * FreyaState>

``FreyaState`` is a wrapper around the OWIN *Environment* - it holds a few more things, but for now the relevant part is the OWIN *Environment*.

As you can see, a ``Freya<'a>`` function takes the state, and asynchronously returns a value of type ``'a`` and the state. We make this function ``async`` as it's very common in web programming to be calling APIs exposed as ``async`` or similar, and we aim to make this as simple to integrate as possible.

Syntax
------

Even given our ``Freya<'a>`` function signature, it would still be awkward to force people to implement this manually all the time, and it wouldn't tidy things up much at all. For this reason, we've implemented the ``freya`` computation expression. Using F# computation expression syntax, you can write functions which implement this signature trivially and safely, with the ability to add more features that we'll see in later sections.

The computation expression syntax looks like this:

.. code-block:: fsharp

   let double x =
       freya {
           return x * 2 }

The signature of this function will be ``int -> Freya<int>``, showing us a nice way to work with functions which now have the OWIN state threaded through them. Of course, this function doesn't do anything with the OWIN state that it has available -- how can we get at that state if we need it?

State
-----

There would clearly be no point in passing the state to and from functions unless we can access it. We accomplish this in Freya by a set of functions, accessible through the ``Freya`` module. Here's an example of getting the state within the ``freya`` computation expression, and reading a value from it (don't worry -- there will be nicer and safer ways to do this!)

.. code-block:: fsharp

   let readPath =
       freya {
           let! state = Freya.State.get
           return state.Environment.["owin.RequestPath"] :?> string }

We can see that we're using a computation expression method first here in ``Freya.State.get`` (note the ``let!`` syntax). The state that we get back is simply our ``FreyaState``, within which we can access our basic OWIN *Environment*. From this we pull a value and cast it to a type that we (hopefully) know it inhabits.

There are two related state methods in the ``Freya`` module, ``Freya.State.map`` and ``Freya.State.set``. They do exactly what you think they do -- applying a function over the state, and setting the state to a provided value respectively.

Of course, anyone used to programming in a helpfully-typed language will be looking at this function with it's risky-looking string-based dictionary access and casting, and wondering whether there might not be a better way. Well in Freya there is (in our opinion at least). The next thing to talk about is :doc:`/core/lenses`...

Summary
-------

We've covered the basic abstraction over state in Freya, the basic type and some simple functions for working with it.

.. code-block:: fsharp

   // Freya<'a> (FreyaState is described)
   type Freya<'a> = FreyaState -> Async<'a * FreyaState>

   // Gets the current FreyaState within a freya computation expression
   Freya.State.get : Freya<FreyaState>

   // Sets the current FreyaState within a freya computation expression
   Freya.State.set : FreyaState -> Freya<unit>

   // Maps the function provided over the current FreyaState within a freya computation expression
   Freya.State.map : (FreyaState -> FreyaState) -> Freya<unit>
