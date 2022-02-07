``length``
==========

.. versionadded:: 2.3

    在Twig2.3中添加了对 ``__toString()`` 魔术方法的支持。

``length`` 过滤器返回一个序列或映射的项的数量，或一个字符串的长度。

对于实现了 ``Countable`` 接口的对象，``length`` 过滤器将使用 ``count()`` 方法返回的值。

对于实现了 ``__toString()`` 魔术方法（而不是 ``Countable``）的对象，它将返回该方法提供的字符串的长度。

对于实现了 ``Traversable`` 接口的对象，``length`` 过滤器将使用 ``iterator_count()`` 方法的返回值。

.. code-block:: twig

    {% if users|length > 10 %}
        ...
    {% endif %}
