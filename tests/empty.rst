``empty``
=========

.. versionadded:: 2.3

    在Twig 2.3中添加了对 ``__toString()`` 魔术方法的支持。

``empty`` 检查一个变量是否为空字符串、空数组、空散列、确切地 ``false``、确切地 ``null``。

对于实现 ``Countable`` 接口的对象，``empty`` 将检查 ``count()`` 方法的返回值。

对于实现 ``__toString()`` 魔术方法（而不是 ``Countable``）的对象，它将检查是否返回一个空字符串。

.. code-block:: jinja

    {% if foo is empty %}
        ...
    {% endif %}
