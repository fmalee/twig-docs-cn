``autoescape``
==============

无论是否启用自动转义，你都可以使用 ``autoescape`` 标签来标记要转义的模板的一个切片：

.. code-block:: jinja

    {% autoescape %}
        此区块中的所有内容都将自动转义
        使用 HTML 策略
    {% endautoescape %}

    {% autoescape 'html' %}
        此区块中的所有内容都将自动转义
        使用 HTML 策略
    {% endautoescape %}

    {% autoescape 'js' %}
        此区块中的所有内容都将自动转义
        使用 js 转义策略
    {% endautoescape %}

    {% autoescape false %}
        一切都将按此区块输出
    {% endautoescape %}

启用自动转义功能后，除标记为安全的值外，默认情况下都会转义所有内容。
可以使用 :doc:`raw<../filters/raw>` 过滤器在模板中标记这些：

.. code-block:: jinja

    {% autoescape %}
        {{ safe_value|raw }}
    {% endautoescape %}

返回模板数据的函数（如 :doc:`macros<macro>` 和 :doc:`parent<../functions/parent>`）始终返回安全标记。


.. note::

    Twig非常智能，无法再通过 :doc:`escape<../filters/escape>` 过滤器来转义已经转义的值。

.. note::

    Twig不会转义静态表达式：

    .. code-block:: jinja

        {% set hello = "<strong>Hello</strong>" %}
        {{ hello }}
        {{ "<strong>world</strong>" }}

    将渲染："<strong>Hello</strong> **world**"。

.. note::

    :doc:`面向开发者<../api>` 一章提供了有关何时以及如何应用自动转义的更多信息。
