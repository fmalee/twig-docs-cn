``sort``
========

``sort`` 过滤器对一个数组进行排序：

.. code-block:: jinja

    {% for user in users|sort %}
        ...
    {% endfor %}

.. note::

    在内部，Twig使用PHP的 `asort`_ 函数来维护索引关联。它通过将Traversable转换为数组来支持它们。

.. _`asort`: https://secure.php.net/asort
