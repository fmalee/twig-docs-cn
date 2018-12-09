``template_from_string``
========================

``template_from_string`` 函数从一个字符串中加载模板：

.. code-block:: jinja

    {{ include(template_from_string("Hello {{ name }}")) }}
    {{ include(template_from_string(page.template)) }}

.. note::

    默认情况下，``template_from_string`` 函数不可用。
    你必须在创建Twig环境时显式添加 ``Twig_Extension_StringLoader`` 扩展：

        $twig = new Twig_Environment(...);
        $twig->addExtension(new Twig_Extension_StringLoader());

.. note::

    即使你可能总是将 ``template_from_string`` 函数与 ``include``
    函数一起使用，也可以将它与任何以模板作为参数的标签或函数（如 ``embed`` 或 ``extends`` 标签）一起使用。

参数
---------

* ``template``: 模板
