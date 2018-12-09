``escape``
==========

``escape`` 过滤器为了能安全插入到最终输出而转义一个字符串。
它根据模板上下文支持不同的转义策略。

默认情况下，它使用HTML转义策略：

.. code-block:: jinja

    {{ user.username|escape }}

为方便起见，给该过滤器定义了一个别名 ``e``：

.. code-block:: jinja

    {{ user.username|e }}

由于可选参数定义了要使用的转义策略，因此 ``escape`` 过滤器也可以在HTML之外的其他上下文中使用：

.. code-block:: jinja

    {{ user.username|e }}
    {# 等同于 #}
    {{ user.username|e('html') }}

以下是如何转义JavaScript代码中包含的变量的示例：

.. code-block:: jinja

    {{ user.username|escape('js') }}
    {{ user.username|e('js') }}

``escape`` 过滤器支持以下转义策略：

* ``html``: 将一个字符串转义为 **HTML正文** 的上下文。

* ``js``: 将一个字符串转义为 **JavaScript上下文**。

* ``css``: 将一个字符串转义为 **CSS上下文**。
  CSS转义可以应用于任何插入CSS的字符串，并转义除字母数字之外的所有内容。

* ``url``: 将一个字符串转义为 **URI或参数上下文**。
  它不应该用于转义整个URI; 而是只插入一个子组件。

* ``html_attr``: 将一个字符串转义为 **HTML属性** 上下文。

.. note::

    在内部，``escape`` 使用PHP原生的 `htmlspecialchars`_ 函数进行HTML转义策略。

.. caution::

    使用了自动转义后，当自动转义策略与转义过滤器应用的策略相同时，Twig会尝试不对变量进行双重转义。
    但是当使用变量作为转义策略时，这将不起作用：

    .. code-block:: jinja

        {% set strategy = 'html' %}

        {% autoescape 'html' %}
            {{ var|escape('html') }}   {# 不会双重转义 #}
            {{ var|escape(strategy) }} {# 将双重转义 #}
        {% endautoescape %}

    使用一个变量作为转义策略时，应禁用自动转义：

    .. code-block:: jinja

        {% set strategy = 'html' %}

        {% autoescape 'html' %}
            {{ var|escape(strategy)|raw }} {# 不会双重转义 #}
        {% endautoescape %}

自定义转义
---------------

你可以通过调用 ``core`` 扩展实例上的 ``setEscaper()`` 方法来定义自定义转义器。
第一个参数是转义器的名称（将在 ``escape`` 调用中使用），第二个参数必须是有效的PHP可调用对象：

.. code-block:: php

    $twig = new Twig_Environment($loader);
    $twig->getExtension('Twig_Extension_Core')->setEscaper('csv', 'csv_escaper');

当由Twig调用时，该可调用对象接收Twig环境实例，要转义的字符串和字符集。

.. note::

    内置的转义器不能被重写，主要是它们应该被视为最终的实现，也是为了更好的性能。

参数
---------

* ``strategy``: 转义策略
* ``charset``:  字符串的字符集

.. _`htmlspecialchars`: https://secure.php.net/htmlspecialchars
