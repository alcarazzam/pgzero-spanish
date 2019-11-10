Ejecutando Pygame Zero en IDLE y otros IDE
==========================================

.. versionadded:: 1.2

Pygame Zero se suele ejecutar usando un comando como::

    pgzrun mi_programa.py

Algunos programas, como los entornos de desarrollo integrado como IDLE y
Edublocks, ejecutarán solo ``python``, no ``pgzrun``.

Pygame Zero incluye una manera de escribir un programa completo en Python que
puede ser ejecutado usando ``python``. Para hacerlo, pon::

    import pgzrun

en la primera línea del programa de Pygame Zero, y pon::

    pgzrun.go()

en la última línea.


Ejemplo
-------
Este es un programa de Pygame Zero que dibuja un círdulo. Puedes ejecutarlo
pegándolo en IDLE::

    import pgzrun


    WIDTH = 800
    HEIGHT = 600

    def draw():
        screen.clear()
        screen.draw.circle((400, 300), 30, 'white')


    pgzrun.go()

