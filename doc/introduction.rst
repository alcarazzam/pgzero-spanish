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
    
    Puedes buscar un montón de objetos gratuitos, incluido este, en `kenney.nl
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

Vamos a poner el alien fuera de la pantalla; cambia la líne ``alien.pos`` para
que sea::

    alien.topright = 0, 10

Nota como puedes asignar a ``topright`` para mover el alien por su esquina
derecha. Si el borde de la mano derecha del alien está en ``0``, el alien está
justo fuera de la pantalla por la izquierda. Ahora vamos a hacerlo mover. Añade
el siguiente código al final del archivo::

    def update():
        alien.left += 2
        if alien.left > WIDTH:
            alien.right = 0

Pygame Zero llamará a tu función :func:`update` en cada frame. Moviendo el alien
un número pequeño de píxeles cada frame hará que se desplace por la pantalla.
Una vez que se sale del lado derecho de la pantalla, lo colocamos a la izquierda.

Tus funciones ``draw()`` y ``update()`` trabajan de maneras parecidas pero están
diseñadas para dos propósitos diferentes. La función ``draw()`` dibuja en la
posición actual el alien mientras que la función ``update()`` es usada para 
mover al alien en la pantalla.


Manejando clicks
----------------

Vamos a hacer que el juego haga algo cuando pulsas en el alien. Para esto 
necesitamos definir una función llamada :func:`on_mouse_down`. Añade esto al 
código fuente::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            print("Eek!")
        else:
            print("You missed me!")

Debes ejecutar el juego e intentar pulsar dentro y fuera del alien.

Pygame Zero es astuto sobre cómo llamar tus funciones. Si no defines tu función
con el parámetro ``pos``, Pygame Zero lo llamará sin una posición. Hay también 
un parámetro ``button`` para ``on_mouse_down``. Podíamos haber escrito::

    def on_mouse_down():
        print("You clicked!")

o::

    def on_mouse_down(pos, button):
        if button == mouse.LEFT and alien.collidepoint(pos):
            print("Eek!")



Sonidos e imágenes
------------------

Vamos a hacer que el alien parezca herido. Guarda estos archivos:

* `alien_hurt.png <_static/alien_hurt.png>`_ - guarda esto como 
  ``alien_hurt.png`` en el directorio ``images``.
* `eep.wav <_static/eep.wav>`_ - crea un directorio llamado ``sounds`` y guarda 
  esto como ``eep.wav`` en ese directorio.

Tu proyecto debe verse así ahora:

.. code-block:: none

    .
    ├── images/
    │   └── alien.png
    │   └── alien_hurt.png
    ├── sounds/
    │   └── eep.wav
    └── intro.py

``sounds/`` es el directorio estándar en el que Pygame Zero mirará para buscar 
tus archivos de sonido.

Vamos a cambiar la función ``on_mouse_down`` para que use esos nuevos archivos::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            alien.image = 'alien_hurt'
            sounds.eep.play()

Cuando ahora pulsas al alien, debes escuchar un sonido, y el objeto se cambiará 
a un alien triste.

Hay un fallo en este juego; el alien no vuelve a cambiar al alien feliz (pero 
el el sonido se escuchará en cada click). Vamos a corregirlo ahora.


Reloj
-----

Si estás familizrizado con Python fuera la programación de juegos, deberías 
conocer la función ``time.sleep()`` que realiza una pausa. Podrías estar 
tentado a escribir un código como este::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            alien.image = 'alien_hurt'
            sounds.eep.play()
            time.sleep(1)
            alien.image = 'alien'

Desafortunadamente, esto no es del todo válido para usarlo en un juego. 
``time.sleep()`` bloquea toda la actividad; queremos que el juego vaya 
funcionando y animando. De hecho, necesitamos salir de ``on_mouse_down``, y 
dejar que el juego funcione cuando reseteamos el alien como parte del procesado 
normal, todo mientras ejecutamos tus funciones ``draw()`` y ``update()``

Esto no es difícil con Pygame Zero, porque tiene la función implícita 
:class:`Clock` que puede programar funciones para que sean llamadas después

Primero, vamos a "refractar" (reorganizar el código). Podemos crear funciones 
para poner el alien como herido y para que vuelva a estar normal::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            set_alien_hurt()


    def set_alien_hurt():
        alien.image = 'alien_hurt'
        sounds.eep.play()


    def set_alien_normal():
        alien.image = 'alien'

Esto no va a haver nada diferente todavía. ``set_alien_normal()`` no será 
llamado. Pero vamos a cambiar ``set_alien_hurt()`` para usar el reloj, para que
``set_alien_normal()`` sea llamado un poco después::

    def set_alien_hurt():
        alien.image = 'alien_hurt'
        sounds.eep.play()
        clock.schedule_unique(set_alien_normal, 0.5)

``clock.schedule_unique()`` hará que ``set_alien_normal()`` sea llamado ``0,5`` 
segundos después. ``schedule_unique()`` también previene que la misma función 
sea llamada más de una vez, como cuando pulsas muy rápido.

Inténtalo, y verás el alien volver a estar normal después de 0,5 segundos. 
Intenta pulsar rápidamente y comprueba que el alien no vuelve estar normal 
después de 0,5 segundos después del primer click.

``clock.schedule_unique()`` acepta tanto enteros y números decimales para el 
intervalo de tiempo. En el tutorial estamos usando números decimales para 
mostrarlo, pero siéntete libre de usar ambos para ver las diferencias y efectos 
de distintos valores.


Resumen
-------

Hemos visto como cargar y dibujar objetos, tocar sonidos, manejar eventos de 
entrada, y usar el reloj interno.

Quizás quieras expandir el juego para mantener una puntuación, o hacer que el 
alien se mueva más erráticamente.

Hay muchas más características hechas para hacer Pygame Zero fácil de usar. 
Compruébalo en :doc:`los objetos internos <builtins>` para aprender cómo usar 
el resto de la API.
