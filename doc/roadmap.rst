Mapa de ruta
============

Pygame Zero es un proyecto de código abierto, y como con cualquier otro 
proyecto, el mapa de ruta de desarrollo es sujeto de cambio.

Este documento únicamente establece algunas metas para futuras versiones, pero 
**no hay garantía** de que esos objetivos se cumplan.


Traducciones
------------

Pygame Zero está dirigido a usuarios jóvenes, cuyas habilidades con el Inglés 
podrían no ser lo suficiente para leer la documentación si no está en su 
lenguaje.

Añadir traducciones a la documentación ayudaría a llevar Pygame Zero a nuevos 
usuarios. Esto es algo que necesita contribuidores para ayudar.

Por favor mira :ref:`la guía de traducción <translating>` si piensas que puedes 
ayudar.


Gamepad Support
---------------

Github Issue: `#70 <https://github.com/lordmauve/pgzero/issues/70>`_

SNES-style gamepads are now extremely cheap. For example, they are sold for
a few pounds from the `Pi Hut`_, in packs of 2 at Amazon_, and even in some
Raspberry Pi bundles.

Gamepad support should not be limited to these specific models; rather, we
should treat this as a lowest-common-denominator across modern gamepads, as
nearly all more modern gamepads have at least as many buttons and axes.

This feature needs to be added in a way that will not **require** a gamepad to
play any Pygame Zero game, in order to follow the principle of
:ref:`accessibility`.

.. _`Pi Hut`: https://thepihut.com/products/raspberry-pi-compatible-usb-gamepad-controller-snes-style
.. _Amazon: https://www.amazon.co.uk/s/ref=nb_sb_noss_2?url=search-alias%3Delectronics&field-keywords=usb+snes


Surface juggling
----------------

Github Issue: `#71 <https://github.com/lordmauve/pgzero/issues/71>`_

Pygame experts make lots of use of off-screen surfaces to create interesting
effects.

Pygame Zero chose to consider only the screen surface, which we wrap with
a richer ``Screen`` API for drawing, etc.

The problem is that there is no easy path to using additional surfaces -
Pygame Zero immediately becomes dead weight as you start to look past that
curtain.

We should look to smooth out this path to make Pygame Zero Actors and Screen
work better with custom surfaces.


Storage
-------

Github Issue: `#33 <https://github.com/lordmauve/pgzero/issues/33>`_

It would be useful for users to be able to save and load data.

The obvious application is save games, but saving and loading whole games can
be pretty hard to get right. The simpler application would just be saving
settings, customisations, high scores, or the highest level reached.

Python of course has APIs for reading and writing files, but this has
additional complexity that teachers might not want to teach immediately.
