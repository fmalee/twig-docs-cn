``include``
===========

``include`` 语句可以引入一个模板，并将该文件的渲染内容返回到当前命名空间：

.. code-block:: jinja

    {% include 'header.html' %}
        Body
    {% include 'footer.html' %}

引入的模板可以访问活动(active)上下文的变量。

如果你使用的是文件系统加载器，则会在其定义的路径中查找模板。

你可以通过在 ``with`` 关键字后面传递其他变量来添加变量：

.. code-block:: jinja

    {# template.html可以访问当前上下文中的变量以及已提供的额外变量 #}
    {% include 'template.html' with {'foo': 'bar'} %}

    {% set vars = {'foo': 'bar'} %}
    {% include 'template.html' with vars %}

你可以通过附加 ``only`` 关键字来禁用对上下文的访问：

.. code-block:: jinja

    {# 只有foo变量才可以被访问 #}
    {% include 'template.html' with {'foo': 'bar'} only %}

.. code-block:: jinja

    {# 没有变量可以被访问 #}
    {% include 'template.html' only %}

.. tip::

    引入一个最终用户创建的模板时，你应该考虑将其沙箱化处理：
    有关的更多信息在 :doc:`面向开发者<../api>` 和 :doc:`sandbox<../tags/sandbox>` 标签文档中。

模板名称可以是任何有效的Twig表达式：

.. code-block:: jinja

    {% include some_var %}
    {% include ajax ? 'ajax.html' : 'not_ajax.html' %}

如果表达式求值为一个 ``Twig_Template`` 或一个
``Twig_TemplateWrapper`` 实例，Twig将直接使用它::

    // {% include template %}

    $template = $twig->load('some_template.twig');

    $twig->display('template.twig', array('template' => $template));

你可以标记一个包含 ``ignore missing`` 的 ``include``，在这种情况下，如果要引入的模板不存在，Twig将忽略该语句。
它必须放在模板名称后面。这是一些有效的例子：

.. code-block:: jinja

    {% include 'sidebar.html' ignore missing %}
    {% include 'sidebar.html' ignore missing with {'foo': 'bar'} %}
    {% include 'sidebar.html' ignore missing only %}

你还可以提供一个在引用之前就检查了存在性的模板列表。将渲染未缺失的第一个模板：

.. code-block:: jinja

    {% include ['page_detailed.html', 'page.html'] %}

如果设置了 ``ignore missing``，但该模板不存在，它将回退到什么都不渲染，否则它将抛出异常。
