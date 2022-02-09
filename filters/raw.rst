``raw``
=======

``raw`` 过滤器标记一个值作为“安全”，这意味着在启用自动转义的环境中如果还有变量未转义，则
``raw`` 是适用于它的最后一个过滤器：

.. code-block:: twig

    {% autoescape %}
        {{ var|raw }} {# var不会被转义 #}
    {% endautoescape %}
