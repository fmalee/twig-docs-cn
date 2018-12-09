``format``
==========

``format`` 过滤器通过替换占位符（遵循 `sprintf`_ 符号）来格式化给定的字符串：

.. code-block:: jinja

    {{ "I like %s and %s."|format(foo, "bar") }}

    {# 如果foo参数等于foo字符串，则输出： I like foo and bar #}

.. _`sprintf`: https://secure.php.net/sprintf

.. seealso:: :doc:`replace<replace>`
