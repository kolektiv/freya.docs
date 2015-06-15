Installation
============

Freya can be installed via `NuGet <http://nuget.org>`_. While Freya is composed of multiple packages (as it's designed to be a *modular stack* rather than a *monolithic framework*) most people will simply want to grab "all" of the latest Freya release. For this there is a meta-package called Freya, which includes all of the commonly used components of Freya.

.. code-block:: shell

    PM> Install-Package Freya

If you want to just use a part of Freya, don't worry -- simply grab the main package that you need, all required dependencies will be installed automatically.
    
.. tip::

   If you're a `Paket <https://fsprojects.github.io/Paket/>`_ user -- and if you're not, it's definitely worth considering -- Freya will play nicely. We make every effort to properly respect `SemVer <http://semver.org>`_ so that you can get the most from Paket.

