Instalando Pygame Zero
======================

Incluido con Mu
---------------

El `IDE Mu <https://codewith.mu>`_, el cual está diseñado para principiantes,
incluye una versión de Pygame Zero.

Necesitarás `cambiar el modo <https://codewith.mu/en/tutorials/1.0/modes>`_ a
Pygame Zero para usarlo. Entonces escribe un programa y
`usa el botón Play <https://codewith.mu/en/tutorials/1.0/pgzero>`_ para
ejecutarlo con Pygame Zero.

.. note::

    La versión de Mu incluida con Pygame Zero puede no ser la última versión.
    Puedes buscar qué versión está instalada ejecutando este código en Mu::

        import pgzero
        print(pgzero.__version__)


Instalación estándar
--------------------

Primero de todo, necesitas **Python 3** instalado. Este está normalmente
instalado si estás usando **Linux** o una **Raspberry Pi**. Puedes descargarlo
desde `python.org <https://www.python.org/>`_ en otros sistemas.


Windows
'''''''

Para instalar Pygame Zero, usa **pip**. En el `intérprete de comandos`__,
introduce lo siguiente

.. __: https://www.solvetic.com/tutoriales/article/2341-formas-de-abrir-consola-de-comandos-en-windows-10/

::

    pip install pgzero


Mac
'''

En una ventana de terminal, introduce

::

   pip install pgzero

Nota que no hay actualmente paquetes precompilados (Wheels) para Pygame que
soporten python 3.4 para Mac, necesitarás actualizar Python a una versión >=3.6
para poder instalar Pygame. Para ver los Wheels disponibles, visita `pypi`__.

.. __: https://pypi.org/project/Pygame/#files

Linux
'''''

En una ventana de terminal, escribe

::

   sudo pip install pgzero


Algunos sistemas Linux lo llaman ``pip3``; si el comando anterior devuelve algo
como ``sudo: pip: command not found`` entonces intenta::

    sudo pip3 install pgzero

Algunas veces pip no está instalado y necesita estar instalado. Si es esto,
intenta esto antes de ejecutar los comandos anteriores.::

    sudo python3 -m ensurepip


.. _install-repl:

Instalando el  REPL
-------------------

:doc:`El REPL de Pygame Zero <repl>` es una característica opcional. Esto puede
ser habilitado al instalarlo con ``pip`` añadiendo ``pgzero[repl]`` al comando::

    pip install pgzero[repl]

Si no estás seguro si tienes el REPL instalado, puedes ejecutar este comando
(no estropeará nada si está instalado).

