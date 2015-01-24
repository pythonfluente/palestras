==========================
Customização de ``format``
==========================

Função ``format`` versus método ``str.format``
==============================================

Dica: domine a função ``format`` antes de estudar o método ``str.format``.

::

    >>> brl = 1/2.43
    >>> brl
    0.4115226337448559
    >>> format(brl, '0.4f')
    '0.4115'
    >>> '1 BRL = {rate:0.2f} USD'.format(rate=brl)
    '1 BRL = 0.41 USD'


Códigos de formatação são vinculados a tipos específicos de objetos.
Mesmo tipos embutidos simples como ``int`` e ``float`` tem seus códigos
específicos.

::

    >>> format(42, 'b')
    '101010'
    >>> format(2/3, '.1%')
    '66.7%'


Códigos de formatação customizados por classe
=============================================

Cada classe pode implementar ``__format__`` e interpretar o argumento ``format_spec`` como quiser. Por exemplo, ``datetime`` usa a notação familiar de ``strftime``.

::

    >>> from datetime import datetime
    >>> now = datetime.now()
    >>> format(now, '%H:%M:%S')
    '18:49:05'
    >>> "It's now {:%I:%M %p}".format(now)
    "It's now 06:49 PM"

A classe ``object`` implementa ``__format__`` invocando ``__str__``. Mas o ``object.__format__`` não aceita códigos de formatação.


::

    >>> v1 = Vector2d(3, 4)
    >>> format(v1)
    '(3.0, 4.0)'
    >>> format(v1, '.3f')
    Traceback (most recent call last):
      ...
    TypeError: non-empty format string passed to object.__format__


Exemplo de customização
=======================

Aplicar códigos de formatação de ``float`` a cada componente do ``Vector``.

::

    >>> v1 = Vector2d(3, 4)
    >>> format(v1)
    '(3.0, 4.0)'
    >>> format(v1, '.2f')
    '(3.00, 4.00)'
    >>> format(v1, '.3e')
    '(3.000e+00, 4.000e+00)'


Implementação do ``__format__`` em ``vector2d_v2.py``.

::

    class Vector2d:

        # vários métodos omitidos [...]

        def __format__(self, fmt_spec=''):
            components = (format(c, fmt_spec) for c in self)
            return '({}, {})'.format(*components)

Formato de coordenadas polares.

::

    >>> format(Vector2d(1, 1), 'p')
    '<1.4142135623730951, 0.7853981633974483>'
    >>> format(Vector2d(1, 1), '.3ep')
    '<1.414e+00, 7.854e-01>'
    >>> format(Vector2d(1, 1), '0.5fp')
    '<1.41421, 0.78540>'


Implementação do formato de coordenadas polares.

::

    class Vector2d:

        # vários métodos omitidos [...]

        def __format__(self, fmt_spec=''):
            if fmt_spec.endswith('p'):
                fmt_spec = fmt_spec[:-1]
                coords = (abs(self), self.angle())
                outer_fmt = '<{}, {}>'
            else:
                coords = self
                outer_fmt = '({}, {})'
            components = (format(c, fmt_spec) for c in coords)
            return outer_fmt.format(*components)

Módulo completo: `vector2d_v2.py`_

.. _vector2d_v2.py: vector2d_v2.py
