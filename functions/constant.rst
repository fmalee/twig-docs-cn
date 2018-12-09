``constant``
============

``constant`` 返回给定字符串的常量值：

.. code-block:: jinja

    {{ some_date|date(constant('DATE_W3C')) }}
    {{ constant('Namespace\\Classname::CONSTANT_NAME') }}

你也可以从对象实例中读取常量：

.. code-block:: jinja

    {{ constant('RSS', date) }}

可以使用 ``defined`` 测试来检查一个常量是否已定义：

.. code-block:: jinja

    {% if constant('SOME_CONST') is defined %}
        ...
    {% endif %}
