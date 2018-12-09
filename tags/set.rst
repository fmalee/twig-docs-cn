``set``
=======

在代码块内部，你还可以为变量赋值。``set`` 标签可以进行赋值，同时可以有多个目标。

以下是如何把 ``bar`` 赋值给 ``foo`` 变量：

.. code-block:: jinja

    {% set foo = 'bar' %}

在调用 ``set`` 后，``foo`` 变量就像模板中的任何其他变量一个可供使用：

.. code-block:: jinja

    {# 显示 bar #}
    {{ foo }}

指定的值可以是任何有效的 :ref:`Twig表达式 <twig-expressions>`：

.. code-block:: jinja

    {% set foo = [1, 2] %}
    {% set foo = {'foo': 'bar'} %}
    {% set foo = 'foo' ~ 'bar' %}

可以在一个区块中同时赋值多个变量：

.. code-block:: jinja

    {% set foo, bar = 'foo', 'bar' %}

    {# 等效于 #}

    {% set foo = 'foo' %}
    {% set bar = 'bar' %}

``set`` 标签也可以用于“捕获”文本块：

.. code-block:: jinja

    {% set foo %}
        <div id="pagination">
            ...
        </div>
    {% endset %}

.. caution::

    如果启用了自动输出转义，Twig将仅在捕获文本块时认为内容是安全的。

.. note::

    请注意，循环在Twig中有作用域; 因此，在循环外部无法访问一个在 ``for`` 循环内声明的变量：

    .. code-block:: jinja

        {% for item in list %}
            {% set foo = item %}
        {% endfor %}

        {# foo 在这里不可用 #}

    如果要访问变量，只需在循环之前声明它：

    .. code-block:: jinja

        {% set foo = "" %}
        {% for item in list %}
            {% set foo = item %}
        {% endfor %}

        {# foo 在这里可用 #}
