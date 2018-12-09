``range``
=========

返回一个包含整数算术级数（arithmetic progression）的列表：

.. code-block:: jinja

    {% for i in range(0, 3) %}
        {{ i }},
    {% endfor %}

    {# 输出： 0, 1, 2, 3, #}

当第三个参数被传递时，它指定了增量（或负值的减量）：

.. code-block:: jinja

    {% for i in range(0, 6, 2) %}
        {{ i }},
    {% endfor %}

    {# 输出： 0, 2, 4, 6, #}

.. note::

    请注意，如果开头大于结尾，则 ``range`` 使用 ``-1`` 作为步数(step)：

    .. code-block:: jinja

        {% for i in range(3, 0) %}
            {{ i }},
        {% endfor %}

        {# outputs 3, 2, 1, 0, #}

Twig内置的 ``..`` 运算符只是 ``range`` 函数的语法糖（步数为 ``1``，如果开头大于结尾，则为 ``-1``）：

.. code-block:: jinja

    {% for i in 0..3 %}
        {{ i }},
    {% endfor %}

.. tip::

    ``range`` 函数使用的是原生的用作本机PHP `range`_  函数。

参数
---------

* ``low``:  序列的第一个值。
* ``high``: 序列的最高值。
* ``step``: 序列元素之间的增量。

.. _`range`: https://secure.php.net/range
