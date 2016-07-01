Optics
======

In the previous section, you saw that the underlying ``State`` instance, and elements of it, could be accessed within a Freya computation expression. While this is workable, it is an untidy and weak approach from the perspective of a strongly typed language.

The solution to this in Freya is to use optics -- see the :doc:`/topics/general/optics` topic for a general introduction -- to enable a safer and more functional approach.

Approach
--------

You saw in the previous example (and it's defined in the OWIN specification) that the data is held within dictionary structures within the state. Here are two ways to retrieve data, the first using the previous naive approach, the second using a pre-defined optic to access the data:

.. code-block:: fsharp

   // The previous way, using raw access to the state
   let readPathRaw =
       freya {
           let! state = Freya.Optic.get id_
           return state.Environment.["owin.RequestPath"] :?> string }

   // The optics way
   let readPath =
       freya {
           return! Freya.Optic.get Request.path_ }

The optic approach is clearer and simpler, and also safer -- the optic gives type-safe access to the data. Here the ``Request.path_`` optic, defined in the ``Freya.Optics.Http`` library, is used to focus on the correct part of the state, and ensure that the data contained is present and correctly mapped to an appropriate type.

The same optic can be used to set the data, or to map a function over the data -- the optics are bi-directional. Here's an example of a function which instead writes to the request path.

.. code-block:: fsharp

   let writePath path =
       freya {
           do! Freya.Optic.set Request.path_ path }

The same lens can be used, the only difference is the function used -- ``Freya.Optic.set`` versus ``Freya.Optic.get``.

Lenses and Prisms
-----------------

The OWIN specification makes it clear that not all data in the OWIN *Environment* will always be present. Some data is optional, so you might find yourself trying to read data that doesn't exist. In a more general sense, you can construct optics that may not always be able to traverse a data structure to the data that you want. These lenses are often called prisms, as opposed to the simpler optics -- lenses -- you saw in the previous section. A lens is an optic to a value of ``'a`` -- a prism to a value of ``'a option``.

In versions of Freya prior to 3.0 (due to the way that earlier versions of Aether worked), you needed to use different functions to work with lenses and prisms (then termed lenses and partial lenses). However, from 3.0 onwards, you can use the same methods to work with lenses or prisms, giving a smaller and more consistent API.

Here's an example using a prism:

.. code-block:: fsharp

   let readStatusCode =
       freya {
           return! Freya.Optic.get Response.statusCode_ }

This function is of type ``Freya<int option>``, as the prism returns an option of the value (the response status code is not a required data element in the OWIN specification).

Morphisms and Types
-------------------

You might be wondering about the description of optics as providing *typed* access to the data here. In the underlying data, it is defined in the OWIN specification that this data is stored as an obj (boxed) in a dictionary. How can you work with it as a string, or an int (and in the case of more complex elements of HTTP as a full typed representation of a header, for example)?

The optics defined are composed with morphisms -- functions which can convert a data structure to and from another form. In the case above, the response status code is being converted to and from an ``int`` transparently as part of the optic access.

This is an important (and powerful) feature of Freya -- you can work with strongly typed, expressive representations of data, even though underneath the surface the data is the old string-based web world.

Here's a quick example, retrieving a header value from the request and receiving a strongly typed representation of that header back, which can be used with all of the type-based F# techniques and tools:

.. code-block:: fsharp

   let readAccept =
       freya {
           return! Freya.Optic.get Request.Headers.accept_ }

   // Might return something like...

   Some (Accept [
       AcceptableMedia (
           Open (Parameters (Map.empty)),
           Some (AcceptParameters (Weight 0.3, Extensions (Map.empty))))
       AcceptableMedia (
           Partial (Type "text", Parameters (Map.empty)),
           Some (AcceptParameters (Weight 0.9, Extensions (Map.empty)))) ])

Here a strongly typed representation of the "Accept" header is retrieved if it's present -- and you'll receive a fully decomposed, typed representation of that header which you can pattern match, inspect and work with -- see :doc:`/reference/libraries/types/index` for more on the type system that Freya uses.

Summary
-------

The Freya approach to working with stateful data has been defined, giving the common functions for working with data, and some optics that are provided with Freya.

.. code-block:: fsharp

   // Get a value from the state using an optic
   Freya.Optic.get : optic 'a -> Freya<'a>

   // Set a value in the state using an optic
   Freya.Optic.set : optic 'a -> 'a -> Freya<unit>

   // Map a function over a value in the state using an optic
   Freya.Optic.map : optic 'a -> ('a -> 'a) -> Freya<unit>

   // Aditionally, common Freya provided optics are available in:
   open Freya.Optics.Http
   open Freya.Optics.Http.Cors

