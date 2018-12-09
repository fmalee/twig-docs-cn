代码规范
================

在编写Twig模版时，我们推荐使用以下这些官方编码规范：

* 在起始分隔符(``{{``、``{%`` 以及 ``{#``)的后面加一个空格，并在结尾分隔符(
  ``}}``、``%}`` 以及 ``#}``)前面加一个空格：

  .. code-block:: jinja

    {{ foo }}
    {# comment #}
    {% if foo %}{% endif %}

  在使用空白控制字符时，不要在它和分隔符之间添加任何空格：

  .. code-block:: jinja

    {{- foo -}}
    {#- comment -#}
    {%- if foo -%}{%- endif -%}

* 在以下操作符前后添加一个空格：比较运算符 (``==``, ``!=``, ``<``, ``>``, ``>=``, ``<=``)、
  数学运算符 (``+``, ``-``, ``/``, ``*``, ``%``, ``//``, ``**``)、
  逻辑运算符 (``not``, ``and``, ``or``)、``~``、``is``、``in`` 以及三元运算符(``?:``):

  .. code-block:: jinja

     {{ 1 + 2 }}
     {{ foo ~ bar }}
     {{ true ? true : false }}

* 在散列中的 ``:`` 标志后添加一个空格，数组和散列的 ``,`` 后也添加一个空格：

  .. code-block:: jinja

     {{ [1, 2, 3] }}
     {{ {'foo': 'bar'} }}

* 不要在表达式的圆括号前后添加空格：

  .. code-block:: jinja

    {{ 1 + (2 * 3) }}

* 不要在字符串分隔符前后添加空格：

  .. code-block:: jinja

    {{ 'foo' }}
    {{ "foo" }}

* 不要在以下操作符前后添加空格：``|``、``.``、``..``、``[]``

  .. code-block:: jinja

    {{ foo|upper|lower }}
    {{ user.name }}
    {{ user[name] }}
    {% for i in 1..12 %}{% endfor %}

* 不要在过滤器和函数调用中的圆括号前后添加空格：

  .. code-block:: jinja

     {{ foo|default('foo') }}
     {{ range(1..10) }}

* 不要在数组和散列的首尾添加空格：

  .. code-block:: jinja

     {{ [1, 2, 3] }}
     {{ {'foo': 'bar'} }}

* 使用小写字母和下划线形式的变量名称：

  .. code-block:: jinja

     {% set foo = 'foo' %}
     {% set foo_bar = 'foo' %}

* 在标签内缩进代码（使用与模板渲染的目标语言相同的缩进方式）

  .. code-block:: jinja

     {% block foo %}
         {% if true %}
             true
         {% endif %}
     {% endblock %}
