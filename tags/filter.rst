``filter``
==========

过滤器部分允许你对一个模板数据区块应用常规的Twig过滤器。
只需将代码封装在特定的 ``filter`` 部分中：

.. code-block:: jinja

    {% filter upper %}
        This text becomes uppercase
    {% endfilter %}

你还可以链接过滤器：

.. code-block:: jinja

    {% filter lower|escape %}
        <strong>SOME TEXT</strong>
    {% endfilter %}

    {# 输出："&lt;strong&gt;some text&lt;/strong&gt;" #}
