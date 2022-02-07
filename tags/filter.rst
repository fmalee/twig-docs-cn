``filter``
==========

.. note::

    从Twig 2.9开始，应该改用 ``apply`` 标签，
    除了封装的模板数据没有限定作用域之外，它做的事情是相同的。

过滤器部分允许你对一个模板数据区块应用常规的Twig过滤器。
只需将代码封装在特定的 ``filter`` 部分中：

.. code-block:: twig

    {% filter upper %}
        This text becomes uppercase
    {% endfilter %}

还可以链接过滤器并将参数传递给它们：

.. code-block:: html+twig

    {% filter lower|escape('html') %}
        <strong>SOME TEXT</strong>
    {% endfilter %}

    {# 输出："&lt;strong&gt;some text&lt;/strong&gt;" #}
