Optics
======

Optics (often -- and in earlier versions of Freya -- referred to as lenses) are a functional technique to enable you to work with complex data structures more easily. As data structures are generally immutable, modifying a data structure (or returning a new instance with the changes reflected) can be quite onerous if the data structure is large and complex, and the area you wish to change is deep within the data structure.

Optics, as their name implies, enable you to *focus* on a particular part of a data structure, treating it as if it were a normal top level instance of some data. You can think of doing your work *through a lens (an optic)*, which will handle the mechanics (or optics, rather) of data structure modification for you.

Aether
------

Freya uses the Aether optics library. The guides to optics found there give a good introduction to the principles, along with more general information about the library itself (which is used extensively in Freya).

* `Aether Guides <https://xyncro.tech/aether/guides/>`_ -- guides to the principles and usage of optics, with examples and explanation.
* `Aether <https://xyncro.tech/aether>`_ -- the main Aether website.

Freya
-----

Optics are a key part of Freya usage, used to work with data throughout many elements of the Freya stack. They are discussed in the reference documentation, as well as being used throughout any tutorial or recipe documentation.

* :doc:`/reference/libraries/core/optics` -- reference for optic usage in the **Freya.Core** library.
