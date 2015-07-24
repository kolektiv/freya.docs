State Operations with Lenses
============================

We've seen how to work with the raw state within a ``freya`` computation expression, but that's not what we'd normally want. We're more likely to want some way of working with the data contained within the OWIN *Environment* within the state, data like the request and response data and the data that makes those up.

We know from the OWIN specification that the data is held within those data structures as string/object dictionaries, so we know that we could write code to manual extract and cast data to forms we'd like to work with. But, we also know that this is likely to be exhausting and error-prone. Not all of the data is guaranteed to be present, and the guarantees we have are quite loose.

With that in mind, we base our approach on *lenses*.

Lenses
------

Lenses are a functional technique to enable us to work with complex data structures more easily. As data structures are generally immutable, modifying a data structure (or returning a new instance with the changes reflected) can be quite onerous if the data structure is large and complex, and the area we wish to change is deep within the data structure.

Lenses, as their name implies, enable us to *focus* on a particular part of a data structure, treating it as if it were a normal top level instance of some data. We can think of doing our work *through a lens*, which will handle the mechanics (or optics, perhaps) of data structure modification for us.

Aether
^^^^^^

Freya uses the `Aether <https://github.com/xyncro/aether>`_ lens library. If you'd like to read more about the lens library in isolation, there is an `introductory guide <kolektiv.github.io/fsharp/aether/2014/08/13/aether-guide/>`_ blog post available. The source is also quite readable. For use within Freya however, it is probably most useful to begin by seeing the use of lenses in action.

Usage
-----

We've seen previously that we could pull data out of our state and manually convert it, so let's see what that looks like compared with a lens based approach:

.. code-block:: fsharp

   // The previous way, using raw access to the state
   let readPathRaw =
       freya {
           let! state = Freya.getState
           return state.Environment.["owin.RequestPath"] :?> string }

   // The lens way
   let readPathLens =
       freya {
           return! Freya.getLens Request.Path_ }
     
As we can see, the lens approach is certainly simpler. We use the ``Freya.getLens`` function, and pass it the lens ``Request.Path_`` (defined in ``Freya.Lenses.Http``). That lens is the equivalent of focusing in to the correct part of our state dictionary, and extracting the data as a string. So, it's simpler -- and it's also safer. The lens gives us typed access to the data structure -- it is both accessor and "converter".

Of course, we can use the same lens to read and write (and therefore to map). If we wanted to *write* to the request, overwriting the request path, we could do so (whether we would want to is a valid question!) Here's what that would look like:

.. code-block:: fsharp

   let writePath path =
       freya {
           do! Freya.setLens Request.Path_ path }

We use the ``Freya.setLens`` function to write -- and notably, we can simply use the same lens. This is because lenses encapsulate operations in both directions -- effectively a getter and a setter, in old school language.

Total and Partial Lenses
^^^^^^^^^^^^^^^^^^^^^^^^

We referred briefly earlier to the concept that not all data in the OWIN *Environment* will always be present. Some data is optional, so we might find ourselves trying to read data that doesn't exist. In a more general sense, we can construct lenses that may not always be able to traverse a data structure to the data that we want. These lenses are partial lenses, as opposed to the simpler -- total -- lenses we saw before. A total lens is a lens to a value of ``'a`` -- a partial lens to a value of ``'a option``.

These subtly different lenses require different lens functions, and we find the additional functions in the ``Freya`` module, our equivalents being ``Freya.getLensPartial`` and ``Freya.setLensPartial``. Depending on the data that you wish to work with, you may find yourself using a total or a partial lens. If the compiler complains -- check you have the right match of function and lens types!

Here's an example using a lens which is a partial lens:

.. code-block:: fsharp

   let readStatusCode =
       freya {
           return! Freya.getLensPartial Response.StatusCode_ }

This function is of type ``Freya<int option>``, as the partial lens returns an option of the value (the response status code is not a required data element in the specification).

Isomorphisms and Types
^^^^^^^^^^^^^^^^^^^^^^

We have also glossed over an important point when we've said that lenses provide *typed* access to the data here. In the underlying data, we know that this data is stored as an obj (boxed) in a dictionary, so how are we able to work with it as a string, or an int (and in the case of more complex elements of HTTP as a full typed representation of a header, for example)?

The lenses we work with are composed with isomorphisms -- functions which can convert a data structure to and from another form. In the case above, the response status code is being converted to and from an ``int`` transparently as part of the lens access.

This is an important (and powerful) feature of Freya -- you can work with strongly typed, expressive representations of a data, even though underneath the surface the data is the old string-based web world.

Here's a quick example, where we are retrieving a header value from the request, and receiving a strongly typed representation of that header back, which we can use with all of our F# techniques and tools:

.. code-block:: fsharp

   let readAccept =
       freya {
           return! Freya.getLensPartial Request.Headers.Accept_ }

   // Might return something like...

   Some (Accept [
       AcceptableMedia (
           Open (Parameters (Map.empty)),
           Some (AcceptParameters (Weight 0.3, Extensions (Map.empty))))
       AcceptableMedia (
           Partial (Type "text", Parameters (Map.empty)),
           Some (AcceptParameters (Weight 0.9, Extensions (Map.empty)))) ])

Here we retrieve a strongly typed representation of the "Accept" header if it's present -- and we'll receive a fully decomposed, typed representation of that header which we can pattern match, inspect and work with (see :doc:`/types-and-lenses/arachne` for more on the type system that Freya uses by default).

Summary
-------

We've covered the recommended Freya approach to reading, writing and modifying data in the OWIN *Environment*, using lenses and associated lens functions.

.. code-block:: fsharp

   // Get a value from the state using a total lens
   Freya.getLens : Lens<FreyaState,'a> -> Freya<'a>

   // Get a value option from the state using a partial lens
   Freya.getLensPartial : PLens<FreyaState,'a> -> Freya<'a option>

   // Set a value in the state using a total lens
   Freya.setLens : Lens<FreyaState,'a> -> 'a -> Freya<unit>

   // Set a value in the state using a partial lens
   Freya.setLensPartial : PLens<FreyaState,'a> -> 'a -> Freya<unit>

   // Map a function over a value in the state using a total lens
   Freya.mapLens : Lens<FreyaState,'a> -> ('a -> 'a) -> Freya<unit>

   // Map a function over a value in the state using a partial lens
   Freya.mapLensPartial : PLens<FreyaState,'a> -> ('a -> 'a) -> Freya<unit>

   // Aditionally, common Freya provided lenses are available in...

   open Freya.Lenses.Http
   open Freya.Lenses.Http.Cors

   // Lenses are currently grouped under the "Request" and "Response" modules

   let accept : PLens<FreyaState,Accept> =
       Request.Headers.accept
