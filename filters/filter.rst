``filter``
==========

.. versionadded:: 2.10

    Twig 2.10中添加了 ``filter`` 过滤器。

``filter`` 过滤器使用箭头函数过滤序列或映射的元素。该箭头函数接收序列或映射的值：

.. code-block:: twig

    {% set sizes = [34, 36, 38, 40, 42] %}

    {{ sizes|filter(v => v > 38)|join(', ') }}
    {# 输出： 40, 42 #}

结合 ``for`` 标签，它允许过滤要迭代的项：

.. code-block:: twig

    {% for v in sizes|filter(v => v > 38) -%}
        {{ v }}
    {% endfor %}
    {# 输出： 40 42 #}

它也适用于映射：

.. code-block:: twig

    {% set sizes = {
        xs: 34,
        s:  36,
        m:  38,
        l:  40,
        xl: 42,
    } %}

    {% for k, v in sizes|filter(v => v > 38) -%}
        {{ k }} = {{ v }}
    {% endfor %}
    {# 输出： l = 40 xl = 42 #}

箭头函数还接收键作为第二个参数：

.. code-block:: twig

    {% for k, v in sizes|filter((v, k) => v > 38 and k != "xl") -%}
        {{ k }} = {{ v }}
    {% endfor %}
    {# 输出： l = 40 #}

请注意，箭头函数可以访问当前上下文。

参数
---------

* ``array``: 序列或映射
* ``arrow``: 箭头函数
