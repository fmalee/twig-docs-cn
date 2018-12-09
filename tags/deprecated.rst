``deprecated``
==============

.. versionadded:: 1.36 and 2.6
    在Twig1.36和2.6中添加了 ``deprecated`` 标签。

如果在一个模板中使用 ``deprecated`` 标签，Twig会生成一个弃用通知（通过调用PHP函数
``trigger_error()``）：

.. code-block:: jinja

    {# base.twig #}
    {% deprecated 'The "base.twig" template is deprecated, use "layout.twig" instead.' %}
    {% extends 'layout.twig' %}

你还可以通过以下方式来弃用一个区块：

.. code-block:: jinja

    {% block hey %}
        {% deprecated 'The "hey" block is deprecated, use "greet" instead.' %}
        {{ block('greet') }}
    {% endblock %}

    {% block greet %}
        Hey you!
    {% endblock %}

请注意，默认情况下，弃用通知处于静默状态，从不显示或记录。
请参阅 :ref:`deprecation-notices` 以了解如何处理它们。
