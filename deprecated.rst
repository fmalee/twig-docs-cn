弃用功能
===================

本文档列出了Twig 2.x中不推荐使用的功能。
保留弃用的功能是为了向后兼容，并在下一个主要版本中删除（在Twig 3.0中删除了Twig 2.x中弃用的功能）。

PSR-0
-----

* 自 Twig 2.7 起，PSR-0 类被弃用，取而代之的是命名空间类。

继承
-----------

* 从Twig 2.5.0开始，不推荐在子模板中的非捕获区块中定义“区块”定义。
  在Twig 3.0中，它会抛出一个 ``Twig\Error\SyntaxError`` 异常。
  它无论如何都不起作用，所以大多数项目都不需要做任何升级。

错误
------

* 自从Twig 2.6.1之后，将字符串作为 ``\Twig\Error\Error`` /
  ``\Twig\Error\Error`` 构造函数上的 ``$source`` 参数传递已被弃用。
  而是传递一个 ``Twig\Source`` 的实例。
  

标签
----

* Twig 2.7不推荐使用 ``spaceless`` 标签。改用 ``spaceless`` 过滤器或
  ``{% apply spaceless %}``（``Twig\Node\SpacelessNode`` 和
  ``Twig\TokenParser\SpacelessTokenParser`` 类也不推荐使用）。

* 在Twig 2.5.0中不推荐使用子模板根级别的 ``spaceless`` 标签。
  这不起作用，因为无论如何人们都希望它能够工作。
  在Twig 3.0中，它会抛出一个 ``Twig\Error\SyntaxError`` 异常。

* Twig 2.9不推荐使用 ``filter`` 标签。改用 ``apply`` 标签（也不推荐使用 
  ``Twig\TokenParser\FilterTokenParser`` 类）。

* 在Twig 2.10中，不推荐在 ``for`` 标签上添加 ``if`` 条件。
  可以在“for”正文中使用 ``filter`` 过滤器或“if”条件（如果您的条件取决于循环中更新的变量）。

最终类
-------------

以下类在Twig 2中标记为 ``@final``，并且在3.0中是最终的：

* ``Twig\Node\ModuleNode``
* ``Twig\TwigFilter``
* ``Twig\TwigFunction``
* ``Twig\TwigTest``
* ``Twig\Profiler\Profile``

解析器
------

* 自Twig 2.7开始，``\Twig\Parser::isReservedMacroName()`` / ``Twig\Parser``
  函数已被弃用，Twig 3.0中将被删除。由于Twig 2没有任何保留的宏名称，因此它总是返回 ``false``。

环境
-----------

* 自Twig 2.7开始，``Twig\Environment`` 上的 ``base_template_class``
  选项已被弃用，并将在Twig 3.0中删除。

* 自Twig 2.7开始，``Twig\Environment::getBaseTemplateClass()``
  和 ``Twig\Environment::setBaseTemplateClass()`` 方法已被弃用，并将在Twig 3.0中删除。

* 自Twig 2.7开始，``Twig\Environment::getTemplateClass()``
  被标记为内部，不应使用。

* 自Twig 2.7开始，将 ``Twig\Template`` 实例传递给 ``Twig\Environment::load()``
  和 ``Twig\Environment::resolveTemplate()`` 已被弃用。

* 根据输入，``Twig\Environment::resolveTemplate()`` 可以返回一个
  ``Twig\Template`` 或 ``Twig\TemplateWrapper`` 实例。
  在Twig 3.0中，此方法将 **始终** 返回一个 ``Twig\TemplateWrapper``实例。
  如果希望向前兼容，则只能依赖此类的方法。

接口
----------

* 自Twig 2.7开始，空的 ``Twig\Loader\ExistsLoaderInterface``
  接口已被弃用，并将在Twig 3.0中删除。

* 自Twig 2.7开始，``Twig\Extension\InitRuntimeInterface``
  接口已被弃用，并将在Twig 3.0中删除。

扩展
----------

* 自Twig 2.11开始，``Twig\Extension\CoreExtension::setEscaper()``
  和 ``Twig\Extension\CoreExtension::getEscapers()`` 已被弃用。在 ``Twig\Extension\EscaperExtension`` 上使用相同的方法代替。

杂项
-------------

* 自Twig 2.7开始，``Twig_SimpleFilter``、``Twig_SimpleFunction``
  以及 ``Twig_SimpleTest`` 空类将被弃用，并将在Twig 3.0中删除。
  分别使用 ``Twig\TwigFilter``、``Twig\TwigFunction`` 以及 ``Twig\TwigTest``。

* 自Twig 2.8.2开始，``Twig\Loader\FilesystemLoader::findTemplate()``
  的所有用法都会检查返回值是否为 ``null`` （与返回 ``false``
  的意思相同）。如果您正在重写 ``Twig\Loader\FilesystemLoader::findTemplate()``，则必须返回
  ``null`` 而不是 ``false``，才能与Twig 3.0兼容。
