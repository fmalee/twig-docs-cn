``include``
===========

``include`` 语句引入一个模板，并输出该文件的渲染内容：

.. code-block:: twig

    {% include 'header.html' %}
        Body
    {% include 'footer.html' %}

.. note::

    建议使用 :doc:`include<../functions/include>`
    函数，因为它提供了更灵活的相同功能：

    * ``include`` 函数在语义上更“正确”（引入一个模板并在当前作用域内输出其渲染的内容；
      一个标签不应显示任何内容）；

    * ``include`` 函数更“可组合（composable）”：

      .. code-block:: twig

          {# 将已渲染模板存储在变量中 #}
          {% set content %}
              {% include 'template.html' %}
          {% endset %}
          {# 对比 #}
          {% set content = include('template.html') %}

          {# 在已渲染模板上应用过滤器 #}
          {% apply upper %}
              {% include 'template.html' %}
          {% endapply %}
          {# 对比 #}
          {{ include('template.html')|upper }}

    * 得益于 :ref:`命名参数 <named-arguments>`，``include``
      函数不会对参数施加任何特定的顺序。

引入的模板可以访问活动(active)上下文的变量。

如果你使用的是文件系统加载器，则会在其定义的路径中查找模板。

你可以通过在 ``with`` 关键字后面传递其他变量来添加变量：

.. code-block:: twig

    {# template.html可以访问当前上下文中的变量以及已提供的额外变量 #}
    {% include 'template.html' with {'foo': 'bar'} %}

    {% set vars = {'foo': 'bar'} %}
    {% include 'template.html' with vars %}

你可以通过附加 ``only`` 关键字来禁用对上下文的访问：

.. code-block:: twig

    {# 只有foo变量才可以被访问 #}
    {% include 'template.html' with {'foo': 'bar'} only %}

.. code-block:: twig

    {# 没有变量可以被访问 #}
    {% include 'template.html' only %}

.. tip::

    引入一个最终用户创建的模板时，你应该考虑将其沙箱化处理：
    有关的更多信息在 :doc:`面向开发者<../api>` 和 :doc:`sandbox<../tags/sandbox>` 标签文档中。

模板名称可以是任何有效的Twig表达式：

.. code-block:: twig

    {% include some_var %}
    {% include ajax ? 'ajax.html' : 'not_ajax.html' %}

如果表达式求值为一个 ``\Twig\Template`` 或一个 ``\Twig\TemplateWrapper`` 实例，Twig将直接使用它::

    // {% include template %}

    $template = $twig->load('some_template.twig');

    $twig->display('template.twig', ['template' => $template]);

你可以标记一个包含 ``ignore missing`` 的 ``include``，在这种情况下，如果要引入的模板不存在，Twig将忽略该语句。
它必须放在模板名称后面。这是一些有效的例子：

.. code-block:: twig

    {% include 'sidebar.html' ignore missing %}
    {% include 'sidebar.html' ignore missing with {'foo': 'bar'} %}
    {% include 'sidebar.html' ignore missing only %}

你还可以提供一个在引用之前就检查了存在性的模板列表。将渲染未缺失的第一个模板：

.. code-block:: twig

    {% include ['page_detailed.html', 'page.html'] %}

如果设置了 ``ignore missing``，但该模板不存在，它将回退到什么都不渲染，否则它将抛出异常。
