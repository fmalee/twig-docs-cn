``default``
===========

如果值未定义或为空，则 ``default`` 过滤器返回传递的默认值，否则返回变量的值：

.. code-block:: twig

    {{ var|default('var is not defined') }}

    {{ var.foo|default('foo item on var is not defined') }}

    {{ var['foo']|default('foo item on var is not defined') }}

    {{ ''|default('passed var is empty')  }}

在某些方法调用的使用变量的表达式中使用 ``default`` 过滤器时，请确保在未定义一个变量时使用 ``default`` 过滤器：

.. code-block:: twig

    {{ var.method(foo|default('foo'))|default('foo') }}

对布尔变量使用 ``default`` 过滤器可能会触发意外行为，因为 ``false`` 被视为空值。考虑使用 ``??`` 代替：

.. code-block:: twig

    {% set foo = false %}
    {{ foo|default(true) }} {# true #}
    {{ foo ?? true }} {# false #}

.. note::

    阅读 :doc:`defined<../tests/defined>` 和 :doc:`empty<../tests/empty>`
    测试的文档，以了解有关其语义的更多信息。

参数
---------

* ``default``: 默认值
