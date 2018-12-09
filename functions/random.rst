``random``
==========

``random`` 函数根据提供的参数类型返回一个随机值：

* 一个序列中的随机项;
* 一个字符串中的随机字符;
* 0和整数参数之间的随机整数（包括）。

.. code-block:: jinja

    {{ random(['apple', 'orange', 'citrus']) }} {# 示例输出: orange #}
    {{ random('ABC') }}                         {# 示例输出: C #}
    {{ random() }}                              {# 示例输出: 15386094 (作为原生PHP的 mt_rand 函数) #}
    {{ random(5) }}                             {# 示例输出: 3 #}

参数
---------

* ``values``: 值

.. _`mt_rand`: https://secure.php.net/mt_rand
