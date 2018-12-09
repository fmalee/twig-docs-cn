``defined``
===========

``defined`` 检查是否在当前上下文中定义了一个变量。
如果你使用了 ``strict_variables`` 选项，这就非常有用：

.. code-block:: jinja

    {# 使用变量名称的 ``defined`` #}
    {% if foo is defined %}
        ...
    {% endif %}

    {# 和变量名称的属性 #}
    {% if foo.bar is defined %}
        ...
    {% endif %}

    {% if foo['bar'] is defined %}
        ...
    {% endif %}

在对某些方法调用中使用变量的表达式进行 ``defined`` 测试时，请确保首先定义它们：

.. code-block:: jinja

    {% if var is defined and foo.method(var) is defined %}
        ...
    {% endif %}
