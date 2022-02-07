``raw``
=======

``raw`` 过滤器标记一个值作为“安全”，这意味着在启用自动转义的环境中如果还有变量未转义，则
``raw`` 是适用于它的最后一个过滤器：

.. code-block:: twig

    {% autoescape %}
        {{ var|raw }} {# var不会被转义 #}
    {% endautoescape %}

.. note::

    **本注释仅适用于1.39和2.8版本之前的Twig。**

    在表达式中使用 ``raw`` 过滤器时要小心：

    .. code-block:: html+twig

        {% autoescape %}
            {% set hello = '<strong>Hello</strong>' %}
            {% set hola = '<strong>Hola</strong>' %}

            {{ false ? '<strong>Hola</strong>' : hello|raw }}
            does not render the same as
            {{ false ? hola : hello|raw }}
            but renders the same as
            {{ (false ? hola : hello)|raw }}
        {% endautoescape %}

    第一个三元语句未被转义：``hello`` 被标记为安全，并且Twig不会转义静态值（请参阅
    :doc:`escape<../tags/autoescape>`）。
    在第二个三元语句中，即使 ``hello`` 被标记为安全，但 ``hola`` 仍然是不安全的，整个表达也是如此。
    第三个三元语句被标记为安全，所以结果不会被转义。
