Arachne
=======

Arachne is a project to provide idiomatically F# typed representations of standards encountered in web programming. It was originally developed as a core part of Freya, combining the types themselves, along with lenses from the ``FreyaState`` to the typed data.

After some discussion, it was decided to split the types (and the accompanying parsing, formatting, and logic code) in to a separate project, so that other projects could take advantage of the work on types without having to take a dependency on a part of the Freya stack (we welcome collaboration with any projects which wish to use or extend the Arachne libraries). The Arachne libraries now live at `freya-fs/arachne <https://github.com/freya-fs/arachne>`_, and welcomes contributions.

The other sections in :doc:`/types-and-optics/index` deal with both the optics provided by the ``Freya.Lenses.*`` libraries, and the types themselves, provided by the ``Arachne.*`` libraries.
