``reduce``
==========

``reduce`` 过滤器使用箭头函数迭代地将序列或映射简化到单个值，以便将其减少到单个值。
箭头函数接收上一次迭代的返回值和序列或映射的当前值：

.. code-block:: twig

    {% set numbers = [1, 2, 3] %}

    {{ numbers|reduce((carry, v) => carry + v) }}
    {# 输出：6 #}

``reduce`` 过滤器将 ``initial`` 值作为第二个参数：

.. code-block:: twig

    {{ numbers|reduce((carry, v) => carry + v, 10) }}
    {# 输出：16 #}

请注意，箭头函数可以访问当前上下文。

参数
---------

* ``arrow``: 箭头函数
* ``initial``: 初始值
