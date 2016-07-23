Machines
========

Freya Machines are a powerful tool, but the programming model can be new to many, especially given the prevalence of web frameworks (particularly MVC-style frameworks) where the logic is spread throughout the whole framework. Machines can therefore look daunting at first, but they actually represent a conceptually simpler and more straightforward approach.

The following references give a directed explanation of Machine design and implementation in Freya, using an HTTP Machine as a convenient example where relevant.

* :doc:`machines/decisions` -- decisions are the basis of the Machine programming model. Structuring logic as well-defined decisions gives more concision and clarity while (when combined with a suitable type model) also giving opportunities for powerful optimisations.
* :doc:`machines/configuration` -- configuration of a Machine is the means by which it can be accurately tailored to a specific purpose. Configuration can be simple and static or dynamic -- enabling a surprisingly powerful execution model.
* :doc:`machines/optimisation` -- a structured decision model combined with suitable type information enables a useful and well-defined optimisation approach to the eventual logic. Machines can be optimised to only execute the minimum neccesary execution path, giving efficient implementations of complex logic.

.. toctree::
   :hidden:
   
   machines/decisions
   machines/configuration
   machines/optimisation
