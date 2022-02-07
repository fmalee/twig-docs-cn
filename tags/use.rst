``use``
=======

.. note::

    横向重用是一种高级的Twig功能，在常规模板中几乎不需要。
    它主要用在在不使用继承的情况下使模板区块可重用的项目。

模板继承是Twig最强大的功能之一，但它仅限于单继承；一个模板只能扩展另一个模板。
此限制使模板继承易于理解且易于调试：

.. code-block:: twig

    {% extends "base.html" %}

    {% block title %}{% endblock %}
    {% block content %}{% endblock %}

横向重用是实现与多重继承相同目标的一种方法，但没有相关的复杂性：

.. code-block:: twig

    {% extends "base.html" %}

    {% use "blocks.html" %}

    {% block title %}{% endblock %}
    {% block content %}{% endblock %}

``use`` 语句告诉Twig将 ``blocks.html`` 中已定义的区块导入到当前模板（它就像宏一样，但只是针对区块）：

.. code-block:: twig

    {# blocks.html #}

    {% block sidebar %}{% endblock %}

在此示例中， ``use`` 语句将 ``sidebar`` 区块导入主模板。
它的代码大部分等同于以下代码（导入的区块不会自动输出）：

.. code-block:: twig

    {% extends "base.html" %}

    {% block sidebar %}{% endblock %}
    {% block title %}{% endblock %}
    {% block content %}{% endblock %}

.. note::

    ``use`` 标签仅在不扩展另一个模板、不定义宏以及内容为空时导入模板。但它可以 *使用* 其他模板。

.. note::

    由于 ``use`` 语句的解析与传递给模板的上下文无关，因此模板引用不能是表达式。

主模板也可以重写任何导入的区块。如果该模板已经定义了 ``sidebar``
区块，则忽略 ``blocks.html`` 中已定义的区块。为避免名称冲突，你可以重命名导入的区块：

.. code-block:: twig

    {% extends "base.html" %}

    {% use "blocks.html" with sidebar as base_sidebar, title as base_title %}

    {% block sidebar %}{% endblock %}
    {% block title %}{% endblock %}
    {% block content %}{% endblock %}

``parent()`` 函数自动确定正确的继承树，因此可以在重写导入模板中已定义的区块时使用它：

.. code-block:: twig

    {% extends "base.html" %}

    {% use "blocks.html" %}

    {% block sidebar %}
        {{ parent() }}
    {% endblock %}

    {% block title %}{% endblock %}
    {% block content %}{% endblock %}

在此示例中，``parent()`` 将准确的从 ``blocks.html`` 模板中用 ``sidebar`` 区块。

.. tip::

    重命名允许你通过调用“父”区块来模拟继承：

    .. code-block:: twig

        {% extends "base.html" %}

        {% use "blocks.html" with sidebar as parent_sidebar %}

        {% block sidebar %}
            {{ block('parent_sidebar') }}
        {% endblock %}

.. note::

    你可以在任何给定模板中使用任意数量的 ``use`` 语句。
    如果两个导​​入的模板定义了相同的块，则最后一个模板将获胜。
