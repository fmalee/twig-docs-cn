``macro``
=========

宏与常规编程语言中的函数相当。它可用于将常用的HTML习语放入可重用的元素中以便不重复自己。

以下是渲染一个表单元素的宏的一个小示例：

.. code-block:: jinja

    {% macro input(name, value, type, size) %}
        <input type="{{ type|default('text') }}" name="{{ name }}" value="{{ value|e }}" size="{{ size|default(20) }}" />
    {% endmacro %}

宏在几个方面与原生PHP函数不同：

* 默认参数值是通过使用宏内容中的 ``default`` 过滤器定义的;

* 一个宏的参数始终是可选的。

* 如果将额外的位置参数传递给宏，它们最终会在特殊的 ``varargs`` 变量中作为一个值列表。

但与PHP函数一样，宏无法访问当前模板的变量。

.. tip::

    你可以使用特殊的 ``_context`` 变量将整个上下文作为参数传递。

导入
------

宏可以在任何模板中定义，并且在使用之前需要“导入”（有关更多信息，请参阅
:doc:`import<../tags/import>` 标签的文档）：

.. code-block:: jinja

    {% import "forms.html" as forms %}

上面的 ``import`` 调用导入“forms.html”文件（它只能包含宏，或一个模板与一些宏），并将其函数导入为 ``forms`` 变量的项。

然后可以随意调用该宏：

.. code-block:: jinja

    <p>{{ forms.input('username') }}</p>
    <p>{{ forms.input('password', null, 'password') }}</p>

如果在同一模板中定义并使用宏，则可以使用特殊的 ``_self`` 变量来导入它们：

.. code-block:: jinja

    {% import _self as forms %}

    <p>{{ forms.input('username') }}</p>

如果要在同一文件中的另一个宏中使用宏，则需要本地化的导入它：

.. code-block:: jinja

    {% macro input(name, value, type, size) %}
        <input type="{{ type|default('text') }}" name="{{ name }}" value="{{ value|e }}" size="{{ size|default(20) }}" />
    {% endmacro %}

    {% macro wrapped_input(name, value, type, size) %}
        {% import _self as forms %}

        <div class="field">
            {{ forms.input(name, value, type, size) }}
        </div>
    {% endmacro %}

命名宏的结束标签
--------------------

Twig允许你在结束标签之后放置宏的名称以提高可读性：

.. code-block:: jinja

    {% macro input() %}
        ...
    {% endmacro input %}

当然，``endmacro`` 单词后面的名称必须与宏名称匹配。

.. seealso:: :doc:`from<../tags/from>`, :doc:`import<../tags/import>`
