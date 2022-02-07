``sandbox``
===========

当没有为Twig环境全局启用沙盒时， ``sandbox`` 标签可用于为引入的模板启用沙盒模式：

.. code-block:: twig

    {% sandbox %}
        {% include 'user.html' %}
    {% endsandbox %}

.. warning::

    ``sandbox`` 标签仅在启用沙盒扩展时可用（请参阅 :doc:`面向开发者<../api>` 文档）。

.. note::

    ``sandbox`` 标签只能用于沙盒一个 ``include`` 标签，并且不能用于沙盒模板的一部分。
    以下示例不起作用：

    .. code-block:: twig

        {% sandbox %}
            {% for i in 1..2 %}
                {{ i }}
            {% endfor %}
        {% endsandbox %}
