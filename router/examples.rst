Examples
========

The URI Template library, combined with the ``Freya.Router`` library, gives a concise yet powerful approach to routing. URI Templates in routing can sometimes be slightly surprising in their behaviour (the specification has some interesting corner cases) but certain techniques are quite generically useful in routing. Some of those will be added here.

.. note::

   Pull requests suggesting new techniques for routing with URI Templates are very welcome!

.. important::

   Some of the specification of URI Templates is aimed at parts of URIs outside of the path (specific formulations for query strings, fragments, etc.) While you can use these features in Freya, the Freya router only matches on the path, and not the combined path and query string. We are considering making matching against query strings possible with the router, but opinions are welcome on whether this would be a useful change.

Lists
-----

Matching simple lists of values in URIs can be useful. Using simple URI Template matching, this is trivial:

.. code-block:: fsharp

   // This uses simple matching and list syntax to match lists of values
   // separated by commas

   let listTemplate =
       UriTemplate.Parse "/{values*}"

   // Path: /one,two,three
   Freya.getLensPartial (Route.List_ "values") // -> Some [ "one"; "two"; "three" ]

Pairs
-----

Matching key/value pairs can also be a useful technique:

.. code-block:: fsharp

   // This uses simple matching and keys syntax to match lists of key/value
   // pairs (x=y) separated by commas

   let listTemplate =
       UriTemplate.Parse "/{pairs*}"

   // Path: /one=a,two=b,three=c
   Freya.getLensPartial (Route.Keys_ "pairs") // -> Some [ ("one", "a"); ("two", "b"); ("three", "c") ]

Paths
-----

It is often useful to want to match a whole path, regardless of what it might be -- for example, a virtual file server of some kind may map a seemingly "physical" path to a different underlying abstraction.

.. code-block:: fsharp

   // This uses URI Template path segment matching and list syntax to
   // match paths separated by "/" characters
                
   let pathTemplate =
       UriTemplate.Parse "{/segments*}"

   // Path: /one/two/three
   Freya.getLensPartial (Route.List_ "segments") // -> Some [ "one"; "two"; "three" ]
       
   let prefixedPathTemplate =
       UriTemplate.Parse "/one{/segments*}"

   // Path: /one/two/three
   Freya.getLensPartial (Route.List_ "segments") // -> Some [ "two"; "three" ]
   

   
