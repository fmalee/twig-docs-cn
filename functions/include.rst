``include``
===========

``include`` 函数返回一个模板的已渲染内容：

.. code-block:: jinja

    {{ include('template.html') }}
    {{ include(some_var) }}

引入的模板可以访问活动(active)上下文的变量。

如果你使用的是文件系统加载器，则会在其定义的路径中查找模板。

默认情况下，上下文会传递给模板，但你也可以传递其他变量：

.. code-block:: jinja

    {# template.html可以访问当前上下文中的变量以及提供的额外变量 #}
    {{ include('template.html', {foo: 'bar'}) }}

你可以通过设置 ``with_context`` 为 ``false`` 来禁用对上下文的访问：

.. code-block:: jinja

    {# 只有foo变量才可以被访问 #}
    {{ include('template.html', {foo: 'bar'}, with_context = false) }}

.. code-block:: jinja

    {# 没有变量可以被访问 #}
    {{ include('template.html', with_context = false) }}

如果表达式求值为一个 ``Twig_Template`` 或 ``Twig_TemplateWrapper`` 实例，Twig将直接使用它::

    // {{ include(template) }}

    $template = $twig->load('some_template.twig');

    $twig->display('template.twig', array('template' => $template));

设置了 ``ignore_missing`` 标志后，如果对应模板不存在，Twig将返回一个空字符串：

.. code-block:: jinja

    {{ include('sidebar.html', ignore_missing = true) }}

你还可以提供一个在引用之前就检查了存在性的模板列表。将渲染未缺失的第一个模板：

.. code-block:: jinja

    {{ include(['page_detailed.html', 'page.html']) }}

如果设置了 ``ignore_missing``，但该模板不存在，它将回退到什么都不渲染，否则它将抛出异常。

引入一个最终用户创建的模板时，你应该考虑将其沙箱化：

.. code-block:: jinja

    {{ include('page.html', sandboxed = true) }}

参数
---------

* ``template``:       要渲染的模板
* ``variables``:      传递给模板的变量
* ``with_context``:   是否传递当前上下文变量
* ``ignore_missing``: 是否忽略缺失的模板
* ``sandboxed``:      是否沙箱化该模板
