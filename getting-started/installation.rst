Installation
============

Freya can be installed via `NuGet <http://nuget.org>`_. While Freya is composed of multiple packages (as it's designed to be a *modular stack* rather than a *monolithic framework*) most people will simply want to grab "all" of the latest Freya release. For this there is a meta-package called Freya, which includes all of the commonly used components of Freya.

.. code-block:: shell

    PM> Install-Package Freya -Pre

.. tip::

   Freya is currently in prerelease, so the -Pre flag is required. If you want to track Freya hitting 1.0, follow `this issue <https://github.com/freya-fs/freya/issues/95>`_ on GitHub.

If you're a `Paket <https://fsprojects.github.io/Paket/>`_ user -- and if you're not, it's definitely worth considering -- Freya will play nicely. We make every effort to properly respect `SemVer <http://semver.org>`_ so that you can get the most from Paket.

