``block``
=========

当模板使用继承时，如果要多次打印一个区块，请使用 ``block`` 函数：

.. code-block:: twig

    <title>{% block title %}{% endblock %}</title>

    <h1>{{ block('title') }}</h1>

    {% block body %}{% endblock %}

``block`` 函数还可用于显示另一个模板中的个区块：

.. code-block:: twig

    {{ block("title", "common_blocks.twig") }}

可以使用 ``defined`` 测试来检查当前模板的上下文中是否存在一个区块：

.. code-block:: twig

    {% if block("footer") is defined %}
        ...
    {% endif %}

    {% if block("footer", "common_blocks.twig") is defined %}
        ...
    {% endif %}

.. seealso::

    :doc:`extends<../tags/extends>`, :doc:`parent<../functions/parent>`
