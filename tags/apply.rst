``apply``
=========

``apply`` 标签允许您在模板数据的区块上应用Twig过滤器：

.. code-block:: twig

    {% apply upper %}
        This text becomes uppercase
    {% endapply %}

还可以链接过滤器并将参数传递给它们：

.. code-block:: html+twig

    {% apply lower|escape('html') %}
        <strong>SOME TEXT</strong>
    {% endapply %}

    {# 输出： "&lt;strong&gt;some text&lt;/strong&gt;" #}
