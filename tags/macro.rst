``macro``
=========

宏与常规编程语言中的函数相当。它们对于重用模板片段而不是重复劳动很有用。

宏在常规模板中定义。

想象一下，有一个通用的帮助模板（称为 ``forms.html``），它定义了如何通过宏渲染 HTML 表单：

.. code-block:: html+twig

    {% macro input(name, value, type = "text", size = 20) %}
        <input type="{{ type }}" name="{{ name }}" value="{{ value|e }}" size="{{ size }}"/>
    {% endmacro %}

    {% macro textarea(name, value, rows = 10, cols = 40) %}
        <textarea name="{{ name }}" rows="{{ rows }}" cols="{{ cols }}">{{ value|e }}</textarea>
    {% endmacro %}

每个宏参数都可以有一个默认值（如果调用中没有提供，这里的 ``text`` 是 ``type`` 的默认值）。

宏在几个方面与原生PHP函数不同：

* 默认参数值是通过使用宏内容中的 ``default`` 过滤器定义的;

* 一个宏的参数始终是可选的。

* 如果将额外的位置参数传递给宏，它们最终会在特殊的 ``varargs`` 变量中作为一个值列表。

但与PHP函数一样，宏无法访问当前模板的变量。

.. tip::

    你可以使用特殊的 ``_context`` 变量将整个上下文作为参数传递。

导入宏
----------------

有两种方法可以导入宏。你可以将包含宏的完整模板导入局部变量（通过 ``import``
标签）或仅从模板中导入特定的宏（通过 ``from`` 标签）。

要将模板中的所有宏导入局部变量，请使用 ``import`` 标签：

.. code-block:: twig

    {% import "forms.html" as forms %}

上面的“import”调用导入了“forms.html”文件（它可以只包含宏，或者一个模板和一些宏），并将宏作为 ``forms`` 局部变量的项导入。

然后可以在 *当前* 模板中随意调用该宏：

.. code-block:: html+twig

    <p>{{ forms.input('username') }}</p>
    <p>{{ forms.input('password', null, 'password') }}</p>

或者，您可以通过 ``from`` 标签将模板中的宏名称导入当前命名空间：

.. code-block:: html+twig

    {% from 'forms.html' import input as input_field, textarea %}

    <p>{{ input_field('password', '', 'password') }}</p>
    <p>{{ textarea('comment') }}</p>

.. tip::

    当宏的使用和定义都在同一个模板中时，你不需要导入宏，因为它们在特殊的 ``_self`` 变量下自动可用：

    .. code-block:: html+twig

        <p>{{ _self.input('password', '', 'password') }}</p>

        {% macro input(name, value, type = "text", size = 20) %}
            <input type="{{ type }}" name="{{ name }}" value="{{ value|e }}" size="{{ size }}"/>
        {% endmacro %}

    宏自动导入仅适用于Twig 2.11。对于旧版本，请使用模板名称的特殊 ``_self`` 变量导入宏：

    .. code-block:: html+twig

        {% import _self as forms %}

        <p>{{ forms.input('username') }}</p>

.. note::

    在Twig 2.11之前，当你想在同一个文件的另一个宏中使用一个宏时，你需要在本地导入它：

    .. code-block:: html+twig

        {% macro input(name, value, type, size) %}
            <input type="{{ type|default('text') }}" name="{{ name }}" value="{{ value|e }}" size="{{ size|default(20) }}"/>
        {% endmacro %}

        {% macro wrapped_input(name, value, type, size) %}
            {% import _self as forms %}

            <div class="field">
                {{ forms.input(name, value, type, size) }}
            </div>
        {% endmacro %}

宏作用域
--------------

.. versionadded:: 2.11

    本段中描述的作用范围规则从Twig 2.11开始生效。

无论你是通过 ``import`` 还是 ``from`` 导入宏，作用域规则都是相同的。

导入的宏始终是当前模板的 **本地**。这意味着宏在当前模板中定义的所有区块和其他宏中都可用，
但在引入的模板或子模板中不可用；你需要在每个模板中显式地重新导入宏。

导入的宏在 ``embed`` 标签的正文中不可用，你需要在标签内显式地重新导入宏。

当从 ``block`` 标签调用 ``import`` 或 ``from``
时，导入的宏只在当前区块中定义，它们会覆盖在模板级别定义的同名宏。

当从 ``macro`` 标签调用 ``import`` 或 ``from``
时，导入的宏仅在当前宏中定义，它们会覆盖在模板级别定义的同名宏。

.. note::

    在Twig 2.11之前，可以在“子区块”的区块中使用导入的宏。
    升级到 2.11时，你需要在全局范围内移动该导入或在“子区块”中显式的重新导入宏。

检查是否定义了宏
------------------------------

.. versionadded:: 2.11

    在Twig 2.11中添加了对宏的 ``defined`` 测试的支持。

你可以通过 ``defined`` 测试来检查是否定义了一个宏：

.. code-block:: twig

    {% import "macros.twig" as macros %}

    {% from "macros.twig" import hello %}

    {% if macros.hello is defined -%}
        OK
    {% endif %}

    {% if hello is defined -%}
        OK
    {% endif %}

命名宏的结束标签
--------------------

Twig允许你将宏的名称放在结束标签之后以提高可读性（``endmacro`` 单词之后的名称必须与宏名称匹配）：

.. code-block:: twig

    {% macro input() %}
        ...
    {% endmacro input %}
