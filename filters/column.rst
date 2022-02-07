``column``
==========

.. versionadded:: 2.8

    Twig 2.8中添加了 ``column`` 过滤器。

``column`` 过滤器返回输入数组中指定列的值。

.. code-block:: twig

    {% set items = [{ 'fruit' : 'apple'}, {'fruit' : 'orange' }] %}

    {% set fruits = items|column('fruit') %}

    {# fruits 现在包含 ['apple', 'orange'] #}

.. note::

    在内部，Twig使用PHP的 `array_column`_ 函数。

参数
---------

* ``name``: 要提取的列名

.. _`array_column`: https://www.php.net/array_column
