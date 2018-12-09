``url_encode``
==============

``url_encode`` 过滤器百分比编码一个给定的字符串为URL片段，或将一个数组编码为查询字符串：

.. code-block:: jinja

    {{ "path-seg*ment"|url_encode }}
    {# 输出： "path-seg%2Ament" #}

    {{ "string with spaces"|url_encode }}
    {# 输出： "string%20with%20spaces" #}

    {{ {'param': 'value', 'foo': 'bar'}|url_encode }}
    {# 输出： "param=value&foo=bar" #}

.. note::

    在内部，Twig使用PHP ``rawurlencode``。

.. _`rawurlencode`: https://secure.php.net/rawurlencode
