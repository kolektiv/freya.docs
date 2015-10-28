Installation
============

Freya can be installed via `NuGet <http://nuget.org>`_. While Freya is composed of multiple packages, you may want to grab the Freya meta-package, includes all of the commonly used components of Freya. Once you get a feel for what's in each package, you will then know which packages you want for various projects.

.. code-block:: shell

    PM> Install-Package Freya

If you want to just use a part of Freya, don't worry -- simply grab the main package that you need, all required dependencies will be installed automatically.
    
.. tip::

   If you're a `Paket <https://fsprojects.github.io/Paket/>`_ user -- and if you're not, it's definitely worth considering -- Freya will play nicely. We make every effort to properly respect `SemVer <http://semver.org>`_ so that you can get the most from Paket.

