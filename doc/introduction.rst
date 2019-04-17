Introducción a Pygame Zero
==========================

.. highlight:: python
    :linenothreshold: 5

Creando una ventana
-------------------

Primero, crea un archivo vacío con el nombre ``intro.py``.

Verifica que esto ejecuta y crea una ventana en negro ejecutando::

    pgzrun intro.py

¡Todo en Pygame Zero es opcional; un fichero en blanco es un programa de Pygame
Zero válido!

Puedes salir del juego pulsando en el botón de cerrar de la ventana o pulsando
``Ctrl-Q`` (``⌘-Q`` en Mac). Si el juego deja de responder por alguna razón,
quizás necesites cerrarlo pulsado ``Ctrl-C`` en la ventana de Terminal.


Dibujando un fondo
------------------

Después, vamos a añadir una función :func:`draw` y especificar las dimensiones
de la ventana. Pygame Zero llamará a esta función cuando necesite pintar la
pantalla.

En ``intro.py``, añade lo siguiente::

    WIDTH = 300
    HEIGHT = 300

    def draw():
        screen.fill((128, 0, 0))

Re-ejecuta ``pgzrun intro.py`` y ¡la pantalla debe ser ahora un cuadrado rojizo!

¿Qué está haciendo este código?

``WIDTH`` y ``HEIGHT`` controlan el ancho y altura de tu ventana. El código fija
el tamaño de la ventana a 300 píxeles para cada lado.

.. Traducción: ¿Cuál es la traducción más exacta de 'sprite'?

``screen`` es un objeto incorporado que representa la pantalla. Tiene
:ref:`una serie de funciones para dibujar objetos y formas <screen>`. La llamada
a la función ``screen.fill()`` rellena la pantalla con un color sólido,
especificado como  una lista de colores ``(red, green, blue)``. ``(128, 0, 0)``
será un rojo medio oscuro. Intenta cambiar esos valores con números entre 0 y
255 y mira qué colores puedes crear.

Vamos a crear un objeto que podemos animar.

Dibuja un objeto
----------------

Antes de que podamos dibujar algo, necesitaremos guardar el objeto *alien* para
usarlo. Puedes pulsar con el botón derecho en este y guardarlo. ("Guardar imagen
como..." o similar).

.. image:: _static/alien.png

(Esta imagen tiene una capa transparente (o "canal alfa"), ¡que es genial para
juegos! Pero está diseñado para un fondo negro, puedes no ser capaz de ver el
casco hasta que no se muestre en el juego).

.. tip::
    
    Puedes buscar un montón de objeto gratuitos, incluido este, en `kenney.nl
    <https://kenney.nl/assets?q=2d>`_. Este procede de `Platformer Art Deluxe
    pack <https://kenney.nl/assets/platformer-art-deluxe>`_.

Necesitas guardar el archivo en lugar adecuado para que Pygame Zero lo pueda
encontrar. Crea un directorio llamado ``images`` y guarda la imagen en él como
``alien.png``. Los dos nombre deben de estar en minúsculas. Pygame Zero avisará
si no, para alertarte del grave fallo entre distintas plataformas.

Si has hecho esto, tu proyecto debe verse así:

.. code-block:: none

    .
    ├── images/
    │   └── alien.png
    └── intro.py

``images/`` es el directorio estándar donde Pygame Zero mirará para buscar tus
imágenes.

Hay una clase incorporada llamada :class:`Actor` que puedes usar para
representar un gráfico a dibujar en la pantalla.

Vamos a definir uno ahora. Cambia el archivo ``intro.py`` para que sea::

    alien = Actor('alien')
    alien.pos = 100, 56

    WIDTH = 500
    HEIGHT = alien.height + 20

    def draw():
        screen.clear()
        alien.draw()

¡Tu alien debe aparecer ahora en la pantalla! Pasando la cadena ``'alien'`` a la
clase ``Actor``, este automáticamente carga el gráfico, y tiene atributos como
posición y dimensión. Esto nos permite especificar la altura de la ventana
basada en la altura del alien.

La función ``alien.draw()`` dibuja el objeto en la pantalla en su posición
actual.

Moviendo el alien
-----------------

Let's set the alien off-screen; change the ``alien.pos`` line to read::

    alien.topright = 0, 10

Note how you can assign to ``topright`` to move the alien actor by its
top-right corner. If the right-hand edge of the alien is at ``0``, the the
alien is just offscreen to the left.  Now let's make it move. Add the following
code to the bottom of the file::

    def update():
        alien.left += 2
        if alien.left > WIDTH:
            alien.right = 0

Pygame Zero will call your :func:`update` function once every frame. Moving the
alien a small number of pixels every frame will cause it to slide across the
screen. Once it slides off the right-hand side of the screen, we reset it back
to the left.

Your functions ``draw()`` and ``update()`` work in similar ways but are designed for two different purposes.
The ``draw()`` function draws the current position of the alien while the ``update()`` function is used to show the alien
moving on the screen.


Handling clicks
---------------

Let's make the game do something when you click on the alien. To do this we
need to define a function called :func:`on_mouse_down`. Add this to the source
code::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            print("Eek!")
        else:
            print("You missed me!")

You should run the game and try clicking on and off the alien.

Pygame Zero is smart about how it calls your functions. If you don't define
your function to take a ``pos`` parameter, Pygame Zero will call it without
a position. There's also a ``button`` parameter for ``on_mouse_down``. So we
could have written::

    def on_mouse_down():
        print("You clicked!")

or::

    def on_mouse_down(pos, button):
        if button == mouse.LEFT and alien.collidepoint(pos):
            print("Eek!")



Sounds and images
-----------------

Now let's make the alien appear hurt. Save these files:

* `alien_hurt.png <_static/alien_hurt.png>`_ - save this as ``alien_hurt.png``
  in the ``images`` directory.
* `eep.wav <_static/eep.wav>`_ - create a directory called ``sounds`` and save
  this as ``eep.wav`` in that directory.

Your project should now look like this:

.. code-block:: none

    .
    ├── images/
    │   └── alien.png
    │   └── alien_hurt.png
    ├── sounds/
    │   └── eep.wav
    └── intro.py

``sounds/`` is the standard directory that Pygame Zero will look in to find
your sound files.

Now let's change the ``on_mouse_down`` function to use these new resources::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            alien.image = 'alien_hurt'
            sounds.eep.play()

Now when you click on the alien, you should hear a sound, and the sprite will
change to an unhappy alien.

There's a bug in this game though; the alien doesn't ever change back to a
happy alien (but the sound will play on each click). Let's fix this next.


Clock
-----

If you're familiar with Python outside of games programming, you might know the
``time.sleep()`` method that inserts a delay. You might be tempted to write
code like this::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            alien.image = 'alien_hurt'
            sounds.eep.play()
            time.sleep(1)
            alien.image = 'alien'

Unfortunately, this is not at all suitable for use in a game. ``time.sleep()``
blocks all activity; we want the game to go on running and animating. In fact
we need to return from ``on_mouse_down``, and let the game work out when to
reset the alien as part of its normal processing, all the while running your
``draw()`` and ``update()`` methods.

This is not difficult with Pygame Zero, because it has a built-in
:class:`Clock` that can schedule functions to be called later.

First, let's "refactor" (ie. re-organise the code). We can create functions to
set the alien as hurt and also to change it back to normal::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            set_alien_hurt()


    def set_alien_hurt():
        alien.image = 'alien_hurt'
        sounds.eep.play()


    def set_alien_normal():
        alien.image = 'alien'

This is not going to do anything different yet. ``set_alien_normal()`` won't be
called. But let's change ``set_alien_hurt()`` to use the clock, so that the
``set_alien_normal()`` will be called a little while after. ::

    def set_alien_hurt():
        alien.image = 'alien_hurt'
        sounds.eep.play()
        clock.schedule_unique(set_alien_normal, 0.5)

``clock.schedule_unique()`` will cause ``set_alien_normal()`` to be called
after ``0.5`` second. ``schedule_unique()`` also prevents the same function
being scheduled more than once, such as if you click very rapidly.

Try it, and you'll see the alien revert to normal after 0.5 second. Try clicking
rapidly and verify that the alien doesn't revert until 0.5 second after the last
click.

``clock.schedule_unique()`` accepts both integers and float numbers for the time interval. in the tutorial we are using
a float number to show this but feel free to use both to see the difference and effects the different values have.


Summary
-------

We've seen how to load and draw sprites, play sounds, handle input events, and
use the built-in clock.

You might like to expand the game to keep score, or make the alien move more
erratically.

There are lots more features built in to make Pygame Zero easy to use. Check
out the :doc:`built in objects <builtins>` to learn how to use the rest of the
API.
