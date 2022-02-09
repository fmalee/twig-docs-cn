``sort``
========

``sort`` 过滤器对一个数组进行排序：

.. code-block:: twig

    {% for user in users|sort %}
        ...
    {% endfor %}

.. note::

    在内部，Twig使用PHP的 `asort`_ 函数来维护索引关联。它通过将Traversable转换为数组来支持它们。

您可以传递一个箭头函数来对数组进行排序：

.. code-block:: html+twig

    {% set fruits = [
        { name: 'Apples', quantity: 5 },
        { name: 'Oranges', quantity: 2 },
        { name: 'Grapes', quantity: 4 },
    ] %}

    {% for fruit in fruits|sort((a, b) => a.quantity <=> b.quantity)|column('name') %}
        {{ fruit }}
    {% endfor %}

    {# 按此顺序输出：Oranges, Grapes, Apples #}

请注意使用 `spaceship`_ 操作符来简化比较。

参数
---------

* ``arrow``: 箭头函数

.. _`asort`: https://www.php.net/asort
.. _`spaceship`: https://www.php.net/manual/en/language.operators.comparison.php
