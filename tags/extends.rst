``extends``
===========

``extends`` 标签可以用于从另一个延伸的模板。
The ``extends`` tag can be used to extend a template from another one.

.. note::

    与PHP一样，Twig不支持多重继承。
    因此，每次渲染只能调用一个扩展标签。但是Twig支持横向 :doc:`重用<use>`。

让我们定义一个 ``base.html`` 基础模板，它定义一个简单的HTML骨架文档：

.. code-block:: html+jinja

    <!DOCTYPE html>
    <html>
        <head>
            {% block head %}
                <link rel="stylesheet" href="style.css" />
                <title>{% block title %}{% endblock %} - My Webpage</title>
            {% endblock %}
        </head>
        <body>
            <div id="content">{% block content %}{% endblock %}</div>
            <div id="footer">
                {% block footer %}
                    &copy; Copyright 2011 by <a href="http://domain.invalid/">you</a>.
                {% endblock %}
            </div>
        </body>
    </html>

在此示例中，块标记定义了子模板可以填充的四个块。
In this example, the :doc:`block<block>` tags define four blocks that child
templates can fill in.

所有block标记都是告诉模板引擎子模板可以覆盖模板的那些部分。
All the ``block`` tag does is to tell the template engine that a child
template may override those portions of the template.

子模板
--------------

一个子模板可能如下所示：

.. code-block:: jinja

    {% extends "base.html" %}

    {% block title %}Index{% endblock %}
    {% block head %}
        {{ parent() }}
        <style type="text/css">
            .important { color: #336699; }
        </style>
    {% endblock %}
    {% block content %}
        <h1>Index</h1>
        <p class="important">
            Welcome on my awesome homepage.
        </p>
    {% endblock %}

该extends标签是这里的关键。它告诉模板引擎该模板“扩展”另一个模板。当模板系统评估此模板时，首先它找到父模板。extends标记应该是模板中的第一个标记。
The ``extends`` tag is the key here. It tells the template engine that this
template "extends" another template. When the template system evaluates this
template, first it locates the parent. The extends tag should be the first tag
in the template.

请注意，由于子模板未定义footer块，因此将使用父模板中的值。
Note that since the child template doesn't define the ``footer`` block, the
value from the parent template is used instead.

你无法block在同一模板中定义具有相同名称的多个标记。存在这种限制是因为块标签在“两个”方向上工作。
也就是说，块标记不仅提供填充孔 - 它还定义填充父级孔的内容。
如果block模板中有两个类似命名的标记，则该模板的父级将不知道要使用哪个块的内容。
You can't define multiple ``block`` tags with the same name in the same
template. This limitation exists because a block tag works in "both"
directions. That is, a block tag doesn't just provide a hole to fill - it also
defines the content that fills the hole in the *parent*. If there were two
similarly-named ``block`` tags in a template, that template's parent wouldn't
know which one of the blocks' content to use.

如果要多次打印块，则可以使用以下 block功能：
If you want to print a block multiple times you can however use the
``block`` function:

.. code-block:: jinja

    <title>{% block title %}{% endblock %}</title>
    <h1>{{ block('title') }}</h1>
    {% block body %}{% endblock %}

父区块
-------------

可以使用父函数呈现父块的内容 。这会返回父块的结果：
It's possible to render the contents of the parent block by using the
:doc:`parent<../functions/parent>` function. This gives back the results of
the parent block:

.. code-block:: jinja

    {% block sidebar %}
        <h3>Table Of Contents</h3>
        ...
        {{ parent() }}
    {% endblock %}

命名区块的结束标签
--------------------

Twig允许你在结束标记之后放置块的名称以提高可读性：
Twig allows you to put the name of the block after the end tag for better
readability:

.. code-block:: jinja

    {% block sidebar %}
        {% block inner_sidebar %}
            ...
        {% endblock inner_sidebar %}
    {% endblock sidebar %}

当然，endblock单词后面的名称必须与块名称匹配。
Of course, the name after the ``endblock`` word must match the block name.

区块嵌套和范围
-----------------------

块可以嵌套以用于更复杂的布局。默认情况下，块可以访问外部作用域中的变量：
Blocks can be nested for more complex layouts. Per default, blocks have access
to variables from outer scopes:

.. code-block:: jinja

    {% for item in seq %}
        <li>{% block loop_item %}{{ item }}{% endblock %}</li>
    {% endfor %}

区块的快捷方式
---------------

对于内容很少的块，可以使用快捷语法。以下构造执行相同的操作：
For blocks with little content, it's possible to use a shortcut syntax. The
following constructs do the same thing:

.. code-block:: jinja

    {% block title %}
        {{ page_title|title }}
    {% endblock %}

.. code-block:: jinja

    {% block title page_title|title %}

动态继承
-------------------

Twig通过使用变量作为基本模板来支持动态继承：
Twig supports dynamic inheritance by using a variable as the base template:

.. code-block:: jinja

    {% extends some_var %}

如果变量求值为a Twig_Template或Twig_TemplateWrapper 实例，则Twig将使用它作为父模板：
If the variable evaluates to a ``Twig_Template`` or a ``Twig_TemplateWrapper``
instance, Twig will use it as the parent template::

    // {% extends layout %}

    $layout = $twig->load('some_layout_template.twig');

    $twig->display('template.twig', array('layout' => $layout));

你还可以提供已检查存在的模板列表。存在的第一个模板将用作父级：
You can also provide a list of templates that are checked for existence. The
first template that exists will be used as a parent:

.. code-block:: jinja

    {% extends ['layout.html', 'base_layout.html'] %}

条件继承
-----------------------

由于父模板的名称可以是任何有效的Twig表达式，因此可以使继承机制成为条件：
As the template name for the parent can be any valid Twig expression, it's
possible to make the inheritance mechanism conditional:

.. code-block:: jinja

    {% extends standalone ? "minimum.html" : "base.html" %}

在此示例中，如果standalone变量求值为，则模板将扩展“minimum.html”布局模板，否则扩展true“base.html”。
In this example, the template will extend the "minimum.html" layout template
if the ``standalone`` variable evaluates to ``true``, and "base.html"
otherwise.

区块是如何工作？
-------------------

块提供了一种方法来更改模板的某个部分的呈现方式，但它不会以任何方式干扰模板周围的逻辑。
A block provides a way to change how a certain part of a template is rendered
but it does not interfere in any way with the logic around it.

让我们以下面的例子来说明一个块如何工作，更重要的是，它是如何工作的：
Let's take the following example to illustrate how a block works and more
importantly, how it does not work:

.. code-block:: jinja

    {# base.twig #}

    {% for post in posts %}
        {% block post %}
            <h1>{{ post.title }}</h1>
            <p>{{ post.body }}</p>
        {% endblock %}
    {% endfor %}

如果渲染此模板，无论是否使用block标记，结果都将完全相同。该block内部for循环只是一个方式，通过一个孩子的模板，使其覆盖的：
If you render this template, the result would be exactly the same with or
without the ``block`` tag. The ``block`` inside the ``for`` loop is just a way
to make it overridable by a child template:

.. code-block:: jinja

    {# child.twig #}

    {% extends "base.twig" %}

    {% block post %}
        <article>
            <header>{{ post.title }}</header>
            <section>{{ post.text }}</section>
        </article>
    {% endblock %}

现在，在渲染子模板时，循环将使用子模板中定义的块而不是基模中定义的块; 然后，执行的模板等效于以下模板：
Now, when rendering the child template, the loop is going to use the block
defined in the child template instead of the one defined in the base one; the
executed template is then equivalent to the following one:

.. code-block:: jinja

    {% for post in posts %}
        <article>
            <header>{{ post.title }}</header>
            <section>{{ post.text }}</section>
        </article>
    {% endfor %}

让我们再看一个例子：if声明中包含的一个块：
Let's take another example: a block included within an ``if`` statement:

.. code-block:: jinja

    {% if posts is empty %}
        {% block head %}
            {{ parent() }}

            <meta name="robots" content="noindex, follow">
        {% endblock head %}
    {% endif %}

与你的想法相反，此模板不会有条件地定义块; 它只是通过子模板覆盖条件所呈现的内容的输出true。
Contrary to what you might think, this template does not define a block
conditionally; it just makes overridable by a child template the output of
what will be rendered when the condition is ``true``.

如果要有条件地显示输出，请使用以下代码：
If you want the output to be displayed conditionally, use the following
instead:

.. code-block:: jinja

    {% block head %}
        {{ parent() }}

        {% if posts is empty %}
            <meta name="robots" content="noindex, follow">
        {% endif %}
    {% endblock head %}

.. seealso:: :doc:`block<../functions/block>`, :doc:`block<../tags/block>`, :doc:`parent<../functions/parent>`, :doc:`use<../tags/use>`
