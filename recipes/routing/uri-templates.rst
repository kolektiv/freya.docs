URI Templates
=============

The URI Template type library, combined with the **Freya.Routers.Uri.Template** library, gives a concise yet powerful approach to routing. URI Templates in routing can sometimes be slightly surprising in their behaviour (the specification has some interesting corner cases) but certain techniques are quite generically useful in routing. Some of those will be added here.

.. note::

   Pull requests suggesting new techniques for routing with URI Templates are very welcome!

.. important::

   The specification of URI Templates is aimed at parts of URIs outside of the path (specific formulations for query strings, fragments, etc.) This means that Freya expects to match a full path and query string. You can make sure this works when you may have a query string by defining a catch-all query string on the end of your route URI Templates.

Lists
-----

Matching simple lists of values in URIs can be useful. Using simple URI Template matching, this is trivial:

.. code-block:: fsharp

   // This uses simple matching and list syntax to match lists of values
   // separated by commas

   let listTemplate =
       UriTemplate.Parse "/{values*}"

   // Path: /one,two,three
   Freya.Optic.get (Route.list_ "values") // -> Some [ "one"; "two"; "three" ]

Pairs
-----

Matching key/value pairs can also be a useful technique:

.. code-block:: fsharp

   // This uses simple matching and keys syntax to match lists of key/value
   // pairs (x=y) separated by commas

   let listTemplate =
       UriTemplate.Parse "/{pairs*}"

   // Path: /one=a,two=b,three=c
   Freya.Optic.get (Route.keys_ "pairs") // -> Some [ ("one", "a"); ("two", "b"); ("three", "c") ]

Paths
-----

It is often useful to want to match a whole path, regardless of what it might be -- for example, a virtual file server of some kind may map a seemingly "physical" path to a different underlying abstraction.

.. code-block:: fsharp

   // This uses URI Template path segment matching and list syntax to
   // match paths separated by "/" characters
                
   let pathTemplate =
       UriTemplate.Parse "{/segments*}"

   // Path: /one/two/three
   Freya.Optic.get (Route.list_ "segments") // -> Some [ "one"; "two"; "three" ]
       
   let prefixedPathTemplate =
       UriTemplate.Parse "/one{/segments*}"

   // Path: /one/two/three
   Freya.Optic.get (Route.list_ "segments") // -> Some [ "two"; "three" ]
   

   
