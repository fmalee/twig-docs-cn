``with``
========

使用 ``with`` 标签可以创建新的内部作用域。在此作用域内设置的变量在作用域之外不可见：

.. code-block:: jinja

    {% with %}
        {% set foo = 42 %}
        {{ foo }}           在这里 foo 是 42
    {% endwith %}
    在这里foo不再可见了

你可以在 ``with`` 标签中传递要定义的变量的散列，而不是在作用域的开头定义变量; 前面的示例等效于以下示例：

.. code-block:: jinja

    {% with { foo: 42 } %}
        {{ foo }}          在这里 foo 是 42
    {% endwith %}
    在这里foo不再可见了

    {# 它适用于任何解析为一个散列的表达式 #}
    {% set vars = { foo: 42 } %}
    {% with vars %}
        ...
    {% endwith %}

默认情况下，内部作用域可以访问外部作用域的上下文; 你可以通过附加 ``only`` 关键字来禁用此行为：

.. code-block:: jinja

    {% set bar = 'bar' %}
    {% with { foo: 42 } only %}
        {# 只定义了 foo #}
        {# bar 没有定义 #}
    {% endwith %}
