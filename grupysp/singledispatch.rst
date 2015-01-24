=========================================
Funções genéricas com ``@singledispatch``
=========================================

Imagine que estamos criando uma ferramenta para depurar aplicativos Web. Queremos gerar HTML a partir de diferentes tipos de objetos.

Podemos começar com uma função assim:

::

    import html

    def htmlize(obj):
        content = html.escape(repr(obj))
        return '<pre>{}</pre>'.format(content)


Mas queremos gerar HTML customizado para vários tipos de objetos.


``str``
    Substituir quebras de linha por ``<br>\n`` e usar ``<p>`` em vez de ``<pre>``.

``int`
    Mostrar o número em decimal e hexadecimal.

``list``
    Gerar uma lista HTML, formatando cada item de acordo com seu tipo.

::

    >>> htmlize({1, 2, 3})
    '<pre>{1, 2, 3}</pre>'
    >>> htmlize(abs)
    '<pre>&lt;built-in function abs&gt;</pre>'
    >>> htmlize('Heimlich & Co.\n- a game')
    '<p>Heimlich &amp; Co.<br>\n- a game</p>'
    >>> htmlize(42)
    '<pre>42 (0x2a)</pre>'
    >>> print(htmlize(['alpha', 66, {3, 2, 1}]))
    <ul>
    <li><p>alpha</p></li>
    <li><pre>66 (0x42)</pre></li>
    <li><pre>{1, 2, 3}</pre></li>
    </ul>

A solução mais flexível do que sobrecarga de métodos é uma função genérica: `generic.py`_.

.. _generic.py: generic.py
