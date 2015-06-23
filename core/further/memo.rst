Memoization
===========

Although in the topics we have covered so far (if you're reading this guide in order) we haven't seen a case where memoization was needed, we will introduce it here, as it is supplied by the ``Freya.Core`` library and is universally applicable.

When building web applications it is often useful to be able to call a function and be confident that the function will only ever be executed once in the context of a request, regardless of how many times it's called.

Imagine a *create* function, or a *delete* function. It may simplify our programming if we can simply call these functions whenever we wish to know the result of them. Rather than having to come up with our own method of carrying around the results of functions, we can simply *memoize* the function and avoid worrying about how many times it has been called.

Freya makes this facility available simply, with the ``Freya.memo`` function:

.. code-block:: fsharp

   // This function will only be executed once per request
   let dangerousFunction =
       freya {
           ... } |> Freya.memo

As you can see, we simply pipe any ``Freya<'a>`` function to ``Freya.memo`` (or wrap it) to take advantage of this. When used with care, this can simplify our code dramatically, removing the need to track the order or validity of a function
call.

Summary
-------

We've seen the important ``Freya.memo`` function, along with a brief explanation of usage. It will become significantly clearer in value when seen as part of the :doc:`/tutorials/index`.

.. code-block:: fsharp

   // Memoize the result of a function in the current state
   Freya.memo : Freya<'a> -> Freya<'a>
   
