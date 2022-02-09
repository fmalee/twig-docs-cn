面向设计师
===========================

本文档介绍模板引擎的语法和语义，这是创建Twig模板时最有用的参考。

概要
--------

模板实际就是一个常规的文本文件。它可以生成任意基于文本的格式（HTML、XML、CSV、LaTeX等等）。
它没有特定的扩展，``.html`` 或 ``.xml`` 都行。

模板包含 **变量** 或 **表达式**，在评估模板时，这些带值的变量或表达式会被替换；
另外，还有控制模板逻辑的 **标签**。

下面是一个非常简单的模板，它阐述了一些基础知识。稍后我们将进一步讨论。

.. code-block:: html+twig

    <!DOCTYPE html>
    <html>
        <head>
            <title>My Webpage</title>
        </head>
        <body>
            <ul id="navigation">
            {% for item in navigation %}
                <li><a href="{{ item.href }}">{{ item.caption }}</a></li>
            {% endfor %}
            </ul>

            <h1>My Webpage</h1>
            {{ a_variable }}
        </body>
    </html>

有两种形式的分隔符：``{% ... %}`` 和 ``{{ ... }}``。
前者用于执行语句，例如 ``for`` 循环；后者将打印表达式的结果。

IDE集成
----------------

很多IDE都支持Twig的语法高亮和自动完成。

* *Textmate* 通过 `Twig bundle`_
* *Vim* 通过 `Jinja syntax plugin`_ 或 `vim-twig plugin`_
* *Netbeans* 通过 `Twig syntax plugin`_ (直到7.1，原生于7.2)
* *PhpStorm* (原生于2.1)
* *Eclipse* 通过 `Twig plugin`_
* *Sublime Text* 通过 `Twig bundle`_
* *GtkSourceView* 通过 `Twig language definition`_ (由gedit和其他项目使用)
* *Coda* 和 *SubEthaEdit* 通过 `Twig syntax mode`_
* *Coda 2* 通过 `other Twig syntax mode`_
* *Komodo* 和 *Komodo Edit* 通过Twig高亮/语法检查模式
* *Notepad++* 通过 `Notepad++ Twig Highlighter`_
* *Emacs* 通过 `web-mode.el`_
* *Atom* 通过 `PHP-twig for atom`_
* *Visual Studio Code* 通过 `Twig pack`_

另外，`TwigFiddle`_ 是一个在线服务，它允许你在浏览器中执行Twig模板；它支持Twig的所有版本。

变量
---------

应用将变量传入模板中进行处理。变量可以包含你能访问的属性或元素。
变量的可视化表现形式很大程度上取决于提供变量的应用。

使用点号（``.``）来访问变量的属性（PHP对象的方法或属性，或PHP数组的单元）：

.. code-block:: twig

    {{ foo.bar }}

.. note::

    你务必要知道花括号并 *不是* 变量的一部分，它只是一个打印语句。
    在访问标签内部的变量时，不要将其放在花括号中。

.. sidebar:: 实现

    为方便起见，``foo.bar`` 在PHP层做了下面这些工作：

    * 检查 ``foo`` 是不是一个数组，``bar`` 是不是一个有效元素;
    * 如果不是，如果 ``foo`` 是一个对象，检查 ``bar`` 是不是有效的属性。
    * 如果不是，如果 ``foo`` 是一个对象，检查 ``bar`` 是不是有效的方法。(即使
      ``bar`` 是构造函数 - 则使用 ``__construct()`` 替代它）
    * 如果不是，如果 ``foo`` 是一个对象，检查 ``getBar`` 是不是有效的方法。
    * 如果不是，如果 ``foo`` 是一个对象，检查 ``isBar`` 是不是有效的方法。
    * 如果不是，如果 ``foo`` 是一个对象，检查 ``hasBar`` 是不是有效的方法。
    * 如果不是，即返回一个 ``null`` 值。

    Twig还支持一种特定的语法来访问PHP数组中的单元，``foo['bar']``：

    * 检查 ``foo`` 是不是一个数组，并检查 ``bar`` 是不是一个有效元素；
    * 如果不是，即返回一个 ``null`` 值。

如果变量或属性不存在，当 ``strict_variables`` 选项被设置为 ``false`` 时，你将接收一个 ``null`` 值。
另外，如果 ``strict_variables`` 被设置了，Twig将抛出一个错误（查看
:ref:`环境选项<environment_options>`）。

.. note::

    如果你想访问变量的动态属性，请使用 :doc:`attribute<functions/attribute>` 函数替代。

    当属性包含特殊字符（如将被解释为减号运算符的 ``-``）时，
    ``attribute`` 函数也能派上用场：

    .. code-block:: twig

        {# 相当于非工作的foo.data-foo #}
        {{ attribute(foo, 'data-foo') }}

全局变量
~~~~~~~~~~~~~~~~

以下变量在模板中始终可用：

* ``_self``: 引用当前模板；
* ``_context``: 引用当前上下文；
* ``_charset``:  引用当前字符集；

设置变量
~~~~~~~~~~~~~~~~~

你可以在代码块内为变量赋值。这里用到了 :doc:`set<tags/set>` 标签：

.. code-block:: twig

    {% set foo = 'foo' %}
    {% set foo = [1, 2] %}
    {% set foo = {'foo': 'bar'} %}

过滤器
-------

可以通过 **过滤器** 来修改变量。过滤器中，用一个管道符号（``|``）来分隔多个过滤器。
可以链接多个过滤器。一个过滤器的输出结果将用在下一个过滤器中。

下面的例子会删除所有带有 ``name`` 和title-cases的HTML标签:

.. code-block:: twig

    {{ name|striptags|title }}

过滤器接收由圆括号包裹的参数。本例使用了逗号拼接列表中的元素：

.. code-block:: twig

    {{ list|join(', ') }}

要在一段代码中应用过滤器，请使用 :doc:`apply<tags/apply>` 标签:

.. code-block:: twig

    {% apply upper %}
        This text becomes uppercase
    {% endapply %}

访问 :doc:`过滤器<filters/index>` 页面，了解更多内置过滤器。

函数
---------

函数可被调用，用于生产内容。函数通过函数名被调用，其后紧跟圆括号（``()``），它还可以设置参数。

举个例子，``range`` 函数返回一个包含整数等差数列的列表：

.. code-block:: twig

    {% for i in range(0, 3) %}
        {{ i }},
    {% endfor %}

访问 :doc:`函数<functions/index>` 页面，了解更多的内置函数。

.. _named-arguments:

具名实参
---------------

.. code-block:: twig

    {% for i in range(low=1, high=10, step=2) %}
        {{ i }},
    {% endfor %}

使用具名实参，使模板中作为参数被传递的值更加清晰。

.. code-block:: twig

    {{ data|convert_encoding('UTF-8', 'iso-2022-jp') }}

    {# versus #}

    {{ data|convert_encoding(from='iso-2022-jp', to='UTF-8') }}

具名实参同样允许你跳过某些不需要改变默认值的参数：

.. code-block:: twig

    {# 第一个参数是日期格式，如果传递的是空值，它将是默认的全局日期格式。 #}
    {{ "now"|date(null, "Europe/Paris") }}

    {# 或者，通过为时区使用一个具名实参来跳过格式值。 #}
    {{ "now"|date(timezone="Europe/Paris") }}

你还可以在一次调用中，同时使用位置参数和具名实参，此时，位置参数必须放在具名实参前面：

.. code-block:: twig

    {{ "now"|date('d/m/Y H:i', timezone="Europe/Paris") }}

.. tip::

    每个函数和过滤器的文档页面都有一章节，列出支持的所有参数。

控制结构
-----------------

控制结构是指控制程序流程的所有东西 - 条件（即 ``if``、``elseif``、``else``），``for`` 循环，以及程序块等等。
控制结构出现在 ``{% ... %}`` 区块中。

例如，要显示一个名为 ``users`` 的变量中提供的用户列表，请使用 :doc:`for<tags/for>` 标签：

.. code-block:: html+twig

    <h1>Members</h1>
    <ul>
        {% for user in users %}
            <li>{{ user.username|e }}</li>
        {% endfor %}
    </ul>

:doc:`if<tags/if>` 标签可以用来测试一个表达式：

.. code-block:: html+twig

    {% if users|length > 0 %}
        <ul>
            {% for user in users %}
                <li>{{ user.username|e }}</li>
            {% endfor %}
        </ul>
    {% endif %}

前往 :doc:`标签<tags/index>` 页面，了解更多内置的标签。

注释
--------

要在模板中注释某一行，使用注释语法 ``{# ... #}``。这常用于调试或者为自己或其他模板设计师添加信息。

.. code-block:: twig

    {# 注意：已禁用模板，因为我们不再使用它
        {% for user in users %}
            ...
        {% endfor %}
    #}

引入其他模板
-------------------------

Twig提供的 :doc:`include<functions/include>`
函数使你更方便地在模板中引入模板，并将该模板的已渲染内容返回到当前模板：

.. code-block:: twig

    {{ include('sidebar.html') }}

默认地，被引入的模板可以访问当前模板的上下文。这意味着，在主模板中定义的任意变量，在被引入的模板中同样可用。

.. code-block:: twig

    {% for box in boxes %}
        {{ include('render_box.html') }}
    {% endfor %}

被引入的 ``render_box.html`` 模板可以访问 ``box`` 变量。

模板的文件名，取决于模板加载器。举个例子：``\Twig\Loader\FilesystemLoader``
允许你通过给定文件名称访问其他模板。你可以使用一个斜线来访问子目录内的模板：

.. code-block:: twig

    {{ include('sections/articles/sidebar.html') }}

这个行为取决于内嵌Twig的应用。

模板继承
--------------------

模板继承是Twig最强大的地方。
模板继承允许你构建一个包含你网站中所有通用元素的基础的"骨架"模板，并定义子模版可以重写的 **区块**。

从一个例子开始更容易理解这个概念。

让我们定义一个基础的 ``base.html`` 模板，它定义了可能用于两栏页面的HTML框架文档：

.. code-block:: html+twig

    <!DOCTYPE html>
    <html>
        <head>
            {% block head %}
                <link rel="stylesheet" href="style.css"/>
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

在这个例子中，:doc:`block<tags/block>` 标签定义了4个区块，可以由子模版进行填充。
对于模板引擎来说，所有的 ``block`` 标签都可以由子模版来重写该部分。

子模版大概是这个样子的：

.. code-block:: html+twig

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
            Welcome to my awesome homepage.
        </p>
    {% endblock %}

其中的 :doc:`extends<tags/extends>` 标签是关键所在。它告诉模板引擎当前模板扩展自另一个模板。
当模板系统评估这个模板时，首先会定位到父模板。注意：``extends`` 标签必须是模板的第一个标签。

注意，由于子模版未定义 ``footer`` 区块，就用来自父模板的值替代使用了。

可以通过使用 :doc:`parent<functions/parent>` 函数来渲染父级区块。它将返回父级区块的结果：

.. code-block:: html+twig

    {% block sidebar %}
        <h3>Table Of Contents</h3>
        ...
        {{ parent() }}
    {% endblock %}

.. tip::

    在 :doc:`extends<tags/extends>`
    标签的文档页面中，还有更多的高级特性介绍，例如块嵌套、作用域、动态继承和条件继承。

.. note::

    在 :doc:`use<tags/use>` 标签的帮助下，Twig还可以通过“横向重用”来支持多重继承。

HTML转义
-------------

当由模板生成HTML时，总会存在一个风险，包含字符的变量会影响最终生成的HTML。
有两种办法来处理：手动地转义每个变量，或者默认自动转义所有变量。

Twig两者都支持，自动转义是默认启用的。

可以通过 :ref:`autoescape<environment_options>` 选项来配置自动转义策略，该策略默认是 ``html``。

使用手动转义
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

如果选择了手动转义，那么转义所需变量就是 **你** 的职责了。
转义什么？来自不可信来源的任何变量。

可以使用 :doc:`escape<filters/escape>` 或 ``e`` 过滤器来转义变量：

.. code-block:: twig

    {{ user.username|e }}

默认地，``escape`` 过滤器使用 ``html``
策略，但取决于转义的上下文，你可能需要显式地使用其他的策略：

.. code-block:: twig

    {{ user.username|e('js') }}
    {{ user.username|e('css') }}
    {{ user.username|e('url') }}
    {{ user.username|e('html_attr') }}

使用自动转义
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

不论是否启用了自动转义，你都可以在模板中使用 :doc:`autoescape<tags/autoescape>`
标签来标记某一部分是否已被转义：

.. code-block:: twig

    {% autoescape %}
        Everything will be automatically escaped in this block (using the HTML strategy)
    {% endautoescape %}

默认地，自动转义使用 ``html`` 转义策略。如果你在其他上下文中输出变量，你必须使用合适的转义策略显式地进行转义：

.. code-block:: twig

    {% autoescape 'js' %}
        Everything will be automatically escaped in this block (using the JS strategy)
    {% endautoescape %}

转义
--------

忽略Twig模板的某一部分，有时是可取的，甚至必要的，被忽略的部分或作为变量或者代码块处理。
比如，使用默认的语法时，你想要在模板中以原生字符串的形式使用 ``{{``，而不是以变量的开头定界符来使用，此时便存在一个风险。

最简单的办法就是使用一个变量表达式来输出变量定界符（``{{``）：

.. code-block:: twig

    {{ '{{' }}

对于较大的段落，它也能一字不差的处理。参考 :doc:`verbatim<tags/verbatim>` 标签。

宏指令
------

宏指令堪比常规程序语言中函数。它们有助于重用HTML片段，避免重复。
它们在 :doc:`macro<tags/macro>` 标签文档中有描述。

.. _twig-expressions:

表达式
-----------

Twig允许在任意位置使用表达式。

.. note::

    运算符优先级如下所示，从最低优先级开始：``?:`` (三元运算符)、``b-and``、
    ``b-xor``、``b-or``、``or``、``and``、``==``、``!=``、``<=>``、``<``、
    ``>``、``>=``、``<=``、``in``、``matches``、``starts with``、
    ``ends with``、``..``、``+``、``-``、``~``、``*``、``/``、``//``、
    ``%``、``is``（测试）、``**``、``??``、``|``（过滤器）, ``[]`` 以及 ``.``：


    .. code-block:: twig

        {% set greeting = 'Hello ' %}
        {% set name = 'Fabien' %}

        {{ greeting ~ name|lower }}   {# Hello fabien #}

        {# 使用括号来改变优先顺序 #}
        {{ (greeting ~ name)|lower }} {# hello fabien #}

字面值
~~~~~~~~

表达式的最简单形式就是字面值。字面值是对PHP类型的陈述，比如字符串、数字、以及数组。存在以下字面值：

* ``"Hello World"``: 在双引号或单引号中的任何内容都是一个字符串。
  无论何时，如果你要在模板中用到字符串，它都能为你带来帮助（比如作为函数调用的参数、过滤器、扩展或引入模版）。
  如果字符串前面有一个反斜杠（``\``），则字符串可以包含分隔符 -- 例如 ``'It\'s good'``。
  如果字符串包含了一个反斜线(例如，``'c:\Program Files'``)，以用两个反斜线来转义它（例如，``'c:\\Program Files'``）。

* ``42`` / ``42.23``: 整数和浮点数是通过写下数字来创建的。
  如果存在点号，则该数字为浮点数，否则为整数。

* ``["foo", "bar"]``: 数组，由一对方括号（``[]``）包裹的由逗号（``,``）分隔的表达式序列组成。

* ``{"foo": "bar"}``: 散列，由一对花括号（``{}``）包裹的以逗号（``,``）分隔的键值对列表构成。

  .. code-block:: twig

    {# 键是字符串 #}
    { 'foo': 'foo', 'bar': 'bar' }

    {# 键是名称（相当于前一个散列） #}
    { foo: 'foo', bar: 'bar' }

    {# 键是数字 #}
    { 2: 'foo', 4: 'bar' }

    {# 如果与变量名相同，则可以省略键 #}
    { foo }
    {# 相当于 #}
    { 'foo': foo }

    {# 键是表达式（表达式必须括在括号中） #}
    {% set foo = 'foo' %}
    { (foo): 'foo', (1 + 1): 'bar', (foo ~ 'b'): 'baz' }

* ``true`` / ``false``: ``true`` 表示正确的值，``false`` 表示错误的值。

* ``null``: ``null`` 表示没有具体的值。
  这是在变量没有值时返回的结果。``none`` 是 ``null`` 的别名。

数组和散列可以嵌套：

.. code-block:: twig

    {% set foo = [1, {"foo": "bar"}] %}

.. tip::

    使用双引号或单引号字符串对性能没有影响，但
    :ref:`字符串插值 <templates-string-interpolation>`
    只被双引号字符串支持。

.. _math-operator:

数学
~~~~

Twig允许你在模板中进行数学运算；并支持以下运算符：

* ``+``: 相加。将两个数字一起相加（操作数将转换为数字）。 ``{{ 1 + 1 }}`` 的结果为 ``2``。

* ``-``: 相减。从第一个数字中减去第二个数字。 ``{{ 3 - 2 }}`` 的结果为 ``1``。

* ``/``: 相除。两个数字相除。返回的值将是一个浮点数。 ``{{ 1 / 2 }}`` 的结果为 ``{{ 0.5 }}``。

* ``%``: 取余。计算整数除法的余数。``{{ 11 % 7 }}`` 的结果为 ``4``。

* ``//``: 取整。两个数字相除并返回内联整数结果。``{{ 2 // 7 }}`` 的结果为
  ``2``，``{{ -20  // 7 }}`` 的结果为 ``-3``
  （这只是 :doc:`round<filters/round>` 过滤器的语法糖）。

* ``*``: 相乘。将左操作数与右操作数相乘。 ``{{ 2 * 2 }}`` 的结果为 ``4``。

* ``**``: 求幂。将左操作数提升到右操作数的幂。``{{ 2 ** 3 }}`` 的结果为 ``8``。

.. _template_logic:

逻辑
~~~~~

你可以使用以下操作符来组合多个表达式：

* ``and``: 与。如果左侧和右侧操作数均为true，则返回true。

* ``or``: 或。如果左操作数或右操作数为true，则返回true。

* ``not``: 非。否定声明。

* ``(expr)``: 分组表达式。

.. note::

    Twig还支持位操作符（``b-and``、``b-xor`` 以及 ``b-or``）。

.. note::

    运算操作符是大小写敏感的。

.. _comparisons-operator:

比较
~~~~~~~~~~~

以下比较运算符可以用于任意表达式中：``==``、``!=``、``<``、``>``、``>=`` 以及 ``<=``。

你还可以检查字符串是否由另一个字符串开头 ``starts with`` 或结尾 ``ends with``：

.. code-block:: twig

    {% if 'Fabien' starts with 'F' %}
    {% endif %}

    {% if 'Fabien' ends with 'n' %}
    {% endif %}

.. note::

    对于复杂的字符串比较，``matches`` 操作符允许你使用`正则表达式`_：

    .. code-block:: twig

        {% if phone matches '/^[\\d\\.]+$/' %}
        {% endif %}

.. _containment-operator:

包含操作符
~~~~~~~~~~~~~~~~~~~~

包含操作符 ``in`` 用于测试是否存在包含关系。
如果左侧运算对象包含在右侧运算对象中，则返回 ``true``：

.. code-block:: twig

    {# 返回 true #}

    {{ 1 in [1, 2, 3] }}

    {{ 'cd' in 'abcde' }}

.. tip::

    你可以使用这个过滤器对字符串、数组、实现了 ``Traversable`` 接口的对象进行包含关系测试。

使用 ``not in`` 操作符执行一个否定测试：

.. code-block:: twig

    {% if 1 not in [1, 2, 3] %}

    {# 等同于 #}
    {% if not (1 in [1, 2, 3]) %}

.. _test-operator:

测试操作符
~~~~~~~~~~~~~

使用 ``is`` 操作符执行测试。可以用于针对变量和一般表达式之间的关系进行测试。右侧操作数即是测试的名称：

.. code-block:: twig

    {# 找出变量是否为奇数 #}

    {{ name is odd }}

测试可以接受参数：

.. code-block:: twig

    {% if post.status is constant('Post::PUBLISHED') %}

``is not`` 操作符进行一个否定测试：

.. code-block:: twig

    {% if post.status is not constant('Post::PUBLISHED') %}

    {# 等同于 #}
    {% if not (post.status is constant('Post::PUBLISHED')) %}

查看 :doc:`测试<tests/index>` 页面，了解更多内置测试。

.. _other-operators:

其他操作符
~~~~~~~~~~~~~~~

以下运算符不适用于任何其他类别：

* ``|``: 应用一个过滤器。

* ``..``: 创建一个基于操作符前后操作数的序列（它是
  :doc:`range<functions/range>` 函数的语法糖）：

  .. code-block:: twig

      {{ 1..5 }}

      {# 等同于 #}
      {{ range(1, 5) }}

  注意，由于 :ref:`运算符优先级规则 <twig-expressions>`
  的原因，你必须使用在将本操作符与过滤器组合时使用括号包裹：

  .. code-block:: twig

      (1..5)|join(', ')

* ``~``: 将所有操作数转换为字符串并连接它们。``{{ "Hello " ~ name ~ "!" }}``
  将会返回 ``Hello John!`` (假定 ``name`` 为 ``'John'``)。

* ``.``、``[]``: 获取变量的属性。

* ``?:``: 三元操作符：

  .. code-block:: twig

      {{ foo ? 'yes' : 'no' }}
      {{ foo ?: 'no' }} 等同于 {{ foo ? foo : 'no' }}
      {{ foo ? 'yes' }} 等同于 {{ foo ? 'yes' : '' }}

* ``??``: 非空操作符：

  .. code-block:: twig

      {# 如果定义了foo，则返回foo的值而不是null，否则返回'no' #}
      {{ foo ?? 'no' }}

.. _templates-string-interpolation:

字符串插值
~~~~~~~~~~~~~~~~~~~~

字符串插值（``#{expression}``）允许在一个 *双引号字符串* 中出现任意有效的表达式。
评估表达式的结果，就是将其插入到字符串中：

.. code-block:: twig

    {{ "foo #{bar} baz" }}
    {{ "foo #{1 + 2} baz" }}

.. _templates-whitespace-control:

空白控制
------------------

模板标签后的第一个换行会被自动移除（如同PHP）。
空白并不是由模板引擎进一步修改的，所以每个空白（空格、制表符、换行）都被未改变地返回。

你还可以在每个标签级别上控制空白。通过使用标签上的空白控制修饰符，可以修剪前导和/或尾随空白。

Twig支持两中修饰符：

* *修剪空白* 的 ``-`` 修饰符：删除所有空白（包括换行符）；

* *修剪行空白* 的 ``~`` 修饰符：删除所有空白（不包括换行符）。
  在右侧使用此修饰符，会禁用默认删除从 PHP 继承的第一个换行符。

修饰符可以用在标签的任一侧，例如 ``{%-`` 或 ``-%}`` ，它们会占用标签那一侧的所有空格。
可以在标签的一侧或两侧使用修饰符：

.. code-block:: html+twig

    {% set value = 'no spaces' %}
    {#- 没有首尾空白 -#}
    {%- if true -%}
        {{- value -}}
    {%- endif -%}
    {# 输出：'no spaces' #}

    <li>
        {{ value }}    </li>
    {# 输出：'<li>\n    no spaces    </li>' #}

    <li>
        {{- value }}    </li>
    {# 输出：'<li>no spaces    </li>' #}

    <li>
        {{~ value }}    </li>
    {# 输出：'<li>\nno spaces    </li>' #}

.. tip::

    除了空白修饰符之外，Twig还有一个 ``spaceless`` 过滤器，可以移除
    **HTML标签之间** 的空白：

    .. code-block:: html+twig

        {% apply spaceless %}
            <div>
                <strong>foo bar</strong>
            </div>
        {% endapply %}

        {# 将会输出：<div><strong>foo bar</strong></div> #}

扩展
----------

Twig支持扩展。如果想要创建自己的扩展，请阅读
:ref:`创建扩展<creating_extensions>` 章节。

.. _`Twig bundle`:                https://github.com/Anomareh/PHP-Twig.tmbundle
.. _`Jinja syntax plugin`:        http://jinja.pocoo.org/docs/integration/#vim
.. _`vim-twig plugin`:            https://github.com/lumiliet/vim-twig
.. _`Twig syntax plugin`:         http://plugins.netbeans.org/plugin/37069/php-twig
.. _`Twig plugin`:                https://github.com/pulse00/Twig-Eclipse-Plugin
.. _`Twig language definition`:   https://github.com/gabrielcorpse/gedit-twig-template-language
.. _`Twig syntax mode`:           https://github.com/bobthecow/Twig-HTML.mode
.. _`other Twig syntax mode`:     https://github.com/muxx/Twig-HTML.mode
.. _`Notepad++ Twig Highlighter`: https://github.com/Banane9/notepadplusplus-twig
.. _`web-mode.el`:                http://web-mode.org/
.. _`正则表达式`:                   https://www.php.net/manual/en/pcre.pattern.php
.. _`PHP-twig for atom`:          https://github.com/reesef/php-twig
.. _`TwigFiddle`:                 https://twigfiddle.com/
.. _`Twig pack`:                  https://marketplace.visualstudio.com/items?itemName=bajdzis.vscode-twig-pack
