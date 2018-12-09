``first``
=========

``first`` 过滤器返回一个序列、映射或字符串的第一个“元素”：

.. code-block:: jinja

    {{ [1, 2, 3, 4]|first }}
    {# 输出： 1 #}

    {{ { a: 1, b: 2, c: 3, d: 4 }|first }}
    {# 输出： 1 #}

    {{ '1234'|first }}
    {# 输出： 1 #}

.. note::

    它还适用于实现了 `Traversable`_ 接口的对象。

.. _`Traversable`: https://secure.php.net/manual/en/class.traversable.php
