``reverse``
===========

``reverse`` 过滤器反转一个序列、映射或者字符串：

.. code-block:: jinja

    {% for user in users|reverse %}
        ...
    {% endfor %}

    {{ '1234'|reverse }}

    {# outputs 4321 #}

.. tip::

    对于序列和映射，反转时不保留其数字键。如果要保留它们的键，请将 ``true``
    作为参数传递给 ``reverse`` 过滤器：

    .. code-block:: jinja

        {% for key, value in {1: "a", 2: "b", 3: "c"}|reverse %}
            {{ key }}: {{ value }}
        {%- endfor %}

        {# 输出: 0: c    1: b    2: a #}

        {% for key, value in {1: "a", 2: "b", 3: "c"}|reverse(true) %}
            {{ key }}: {{ value }}
        {%- endfor %}

        {# 输出: 3: c    2: b    1: a #}

.. note::

    它还适用于实现了 `Traversable`_ 接口的对象。

参数
---------

* ``preserve_keys``: 在反转映射或序列时保留键。

.. _`Traversable`: https://secure.php.net/Traversable
