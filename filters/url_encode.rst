``url_encode``
==============

``url_encode`` 过滤器百分比编码一个给定的字符串为URL片段，或将一个数组编码为查询字符串：

.. code-block:: twig

    {{ "path-seg*ment"|url_encode }}
    {# 输出："path-seg%2Ament" #}

    {{ "string with spaces"|url_encode }}
    {# 输出："string%20with%20spaces" #}

    {{ {'param': 'value', 'foo': 'bar'}|url_encode }}
    {# 输出："param=value&foo=bar" #}

.. note::

    在内部，Twig使用PHP的 `rawurlencode`_ 或 `http_build_query`_ 函数。

.. _`rawurlencode`: https://www.php.net/rawurlencode
.. _`http_build_query`: https://www.php.net/http_build_query
