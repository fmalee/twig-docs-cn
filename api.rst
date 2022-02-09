面向开发者
===================

这一章介绍Twig的API而非模版语言。这是针对实现模板接口的最有用的参考，而不是针对Twig模板的创建。

基础
------

Twig 使用一个名为 **environment** 的中心对象（它是 ``\Twig\Environment`` 类的对象）。
这个类的实例用于存储配置和扩展，以及加载模版。

大多数应用在初始化的时候创建一个 ``\Twig\Environment`` 对象，并用它来加载模板。
在某些情况下，同时拥有多个具有不同配置的环境可能会很有用。

配置Twig为应用加载模板的典型方法大致如下所示::

    require_once '/path/to/vendor/autoload.php';

    $loader = new \Twig\Loader\FilesystemLoader('/path/to/templates');
    $twig = new \Twig\Environment($loader, [
        'cache' => '/path/to/compilation_cache',
    ]);

这将会创建一个带默认配置的模板环境，以及一个在 ``/path/to/templates/`` 目录查找模版的加载器。
可以使用不同的加载器，如果你想从数据库或者其他来源加载模版，你也可以自己写一个加载器。

.. note::

    值得注意的是，环境的第二个参数是一个环境选项数组。其中的 ``cache``
    选项表示编译缓存目录，Twig将编译过的模板缓存储存在这个目录中以避免解析阶段的重复请求。
    这与你可能想要为评估后的模版添加的缓存有很大的区别。对于这样的需求，你可以使用其他PHP缓存库。

渲染模板
-------------------

要从一个Twig环境中加载模版，只需要调用 ``load()``
法即可，它会返回一个 ``\Twig\TemplateWrapper`` 实例::

    $template = $twig->load('index.html');

要使用一些变量来渲染模板，请调用 ``render()`` 方法::

    echo $template->render(['the' => 'variables', 'go' => 'here']);

.. note::

    ``display()`` 方法是直接输出已渲染模版的快捷方式。

你还可以一步完成模板的加载和渲染::

    echo $twig->render('index.html', ['the' => 'variables', 'go' => 'here']);

如果一个模板定义了区块，它们可以通过调用 ``renderBlock()`` 来单独渲染该区块::

    echo $template->renderBlock('block_name', ['the' => 'variables', 'go' => 'here']);

.. _environment_options:

环境选项
-------------------

在创建一个新的 ``\Twig\Environment`` 实例时，你可以传递一个选项数组作为构造函数的第二个参数::

    $twig = new \Twig\Environment($loader, ['debug' => true]);

有以下这些可用选项：

* ``debug`` *boolean*

  设置为 ``true`` 时，生成的模版会有一个 ``__toString()``
  方法，可以用它来显示生成的节点。（默认设置是 ``false``）。

* ``charset`` *string* (defaults to ``utf-8``)

  用于模板的字符集。

* ``cache`` *string* or ``false``

  一个存储已编译的模板的绝对路径，或者设为 ``false`` 来禁用缓存（这也是默认行为）。

* ``auto_reload`` *boolean*

  在使用Twig进行开发时，这有助于在源代码修改时随时进行重新编译。
  如果不为 ``auto_reload`` 选项设置值，它会自动根据 ``debug`` 选项的值被设定。

* ``strict_variables`` *boolean*

  如果设置为 ``false``，Twig会静默忽略无效的变量（包括变量、不存在的属性和方法），并以 ``null`` 值替换它们。
  如果将其设置为 ``true``，Twig则会抛出一个异常（默认是 ``false``）。

* ``autoescape`` *string*

  设置默认的自动转义策略（``name``、``html``、``js``、``css``、``url``、
  ``html_attr``，或者一个采用模板“filename”并返回要使用的转义策略的PHP回调 --
  该回调不能是函数名称，以避免与内置转义策略冲突）；将其设置为 ``false``，可以禁用自动转义。
  ``name`` 转义策略根据模板文件扩展名来确定用于该模板的转义策略
  （此策略在运行时中不会产生任何开销，因为自动转义是在编译时中完成的。）

* ``optimizations`` *integer*

  此处的标记表示哪种优化方式将被应用（默认是 ``-1`` —— 所有的优化设置都被启用；设置为 ``0``，则表示禁用）。

加载器
-------

加载器负责从文件系统之类的来源中加载模板。

编译缓存
~~~~~~~~~~~~~~~~~

所有模板加载器都可以在文件系统中缓存已编译的模板以便将来重用。
只编译一次模板能为Twig提速不少。

内置加载器
~~~~~~~~~~~~~~~~

这是一个内置加载器列表：

``\Twig\Loader\FilesystemLoader``
.................................

``\Twig\Loader\FilesystemLoader`` 从文件系统加载模板。
这个加载器可以从文件系统的目录中找到模板，这是加载模板的最佳方式::

    $loader = new \Twig\Loader\FilesystemLoader($templateDir);

它还可以从由多个目录组成的数组中寻找模板::

    $loader = new \Twig\Loader\FilesystemLoader([$templateDir1, $templateDir2]);

按照这样的配置，Twig会首先在 ``$templateDir1`` 中查找模板，如果没有，则回退到  ``$templateDir2`` 中继续查找模板。

还可以通过 ``addPath()`` 和 ``prependPath()`` 方法添加或预设路径::

    $loader->addPath($templateDir3);
    $loader->prependPath($templateDir4);

文件系统加载器还支持命名空间模板。这允许将拥有各种路径的模板组织到不同的命名空间下。

在使用 ``setPaths()``、``addPath()`` 以及 ``prependPath()``
方法时，将命名空间指定为第二个参数，如果没有指定，这些方法会调用主命名空间::

    $loader->addPath($templateDir, 'admin');

命名空间模板可以用通过特定的 ``@namespace_name/template_path`` 符号访问::

    $twig->render('@admin/index.html', []);

``\Twig\Loader\FilesystemLoader`` 支持绝对和相对路径。
使用相对路径是首选，因为它使缓存键独立于项目根目录
（例如，它允许从一个构建服务器加热缓存，其中目录可能与生产服务器上使用的目录不同）::

    $loader = new \Twig\Loader\FilesystemLoader('templates', getcwd().'/..');

.. note::

    当未将根路径作为第二个参数传递时，Twig使用 ``getcwd()`` 获取相对路径。

``\Twig\Loader\ArrayLoader``
............................

``\Twig\Loader\ArrayLoader`` 从一个PHP数组加载模板。它被传递一个绑定到模板名称的字符串数组::

    $loader = new \Twig\Loader\ArrayLoader([
        'index.html' => 'Hello {{ name }}!',
    ]);
    $twig = new \Twig\Environment($loader);

    echo $twig->render('index.html', ['name' => 'Fabien']);

这个加载器对于单元测试非常有用。它还可以用于将所有模板存放在单个PHP文件内的小型项目。

.. tip::

    使用带有缓存机制的 ``Array`` 加载器时，你应当明白新的缓存键是在每次模板内容改变时生成的（缓存键是指模板的源代码）。
    如果不希望缓存失控地增加，你需要注意自行清除旧的缓存。

``\Twig\Loader\ChainLoader``
............................

``\Twig\Loader\ChainLoader`` 将模板的加载工作委派给其他加载器::

    $loader1 = new \Twig\Loader\ArrayLoader([
        'base.html' => '{% block content %}{% endblock %}',
    ]);
    $loader2 = new \Twig\Loader\ArrayLoader([
        'index.html' => '{% extends "base.html" %}{% block content %}Hello {{ name }}{% endblock %}',
        'base.html'  => '永远不会被加载',
    ]);

    $loader = new \Twig\Loader\ChainLoader([$loader1, $loader2]);

    $twig = new \Twig\Environment($loader);

在查找模板时，Twig会轮流尝试每个加载器，并在找到模板时立即返回。
前面的例子中，在渲染 ``index.html`` 模板时，Twig会使用 ``$loader2``
来加载它，但 ``base.html`` 模板会从 ``$loader1`` 中加载。

.. note::

    你还可以使用 ``addLoader()`` 方法来添加加载器。

创建你自己的加载器
~~~~~~~~~~~~~~~~~~~~~~

所有的加载器都实现了 ``\Twig\Loader\LoaderInterface``::

    interface \Twig\Loader\LoaderInterface
    {
        /**
         * Returns the source context for a given template logical name.
         *
         * @param string $name The template logical name
         *
         * @return \Twig\Source
         *
         * @throws \Twig\Error\LoaderError When $name is not found
         */
        public function getSourceContext($name);

        /**
         * Gets the cache key to use for the cache for a given template name.
         *
         * @param string $name The name of the template to load
         *
         * @return string The cache key
         *
         * @throws \Twig\Error\LoaderError When $name is not found
         */
        public function getCacheKey($name);

        /**
         * Returns true if the template is still fresh.
         *
         * @param string    $name The template name
         * @param timestamp $time The last modification time of the cached template
         *
         * @return bool    true if the template is fresh, false otherwise
         *
         * @throws \Twig\Error\LoaderError When $name is not found
         */
        public function isFresh($name, $time);

        /**
         * Check if we have the source code of a template, given its name.
         *
         * @param string $name The name of the template to check if we can load
         *
         * @return bool    If the template source code is handled by this loader or not
         */
        public function exists($name);
    }

根据最后修改的时间，如果当前被缓存的模板仍然是最新的，则 ``isFresh()`` 方法必须返回
``true``，否则返回 ``false``。

``getSourceContext()`` 方法必须返回 ``\Twig\Source`` 的一个实例。

使用扩展
----------------

Twig的扩展其实是为Twig添加新特性的软件包。通过 ``addExtension()`` 方法注册一个扩展::

    $twig->addExtension(new \Twig\Extension\SandboxExtension());

Twig捆绑了以下扩展：

* *Twig\Extension\CoreExtension*: 定义Twig的所有核心特性。

* *Twig\Extension\DebugExtension*: 定义 ``dump`` 函数以帮助调试模板变量。

* *Twig\Extension\EscaperExtension*: 添加自动输出转义以及转义/不转义代码区块的可能性。

* *Twig\Extension\SandboxExtension*: 为默认的Twig环境添加沙盒模式，使其能安全地评估未受信任的代码。

* *Twig\Extension\ProfilerExtensionr*: 启用内置的Twig分析器。

* *Twig\Extension\OptimizerExtension*: 在编译前优化节点树。

* *Twig\Extension\StringLoaderExtension*: 定义 ``template_from_string``
  函数以允许从模板中的字符串加载模板。

Core、Escaper 和 Optimizer 扩展是默认注册的。

内置扩展
-------------------

这一节介绍由内置扩展添加的特性

.. tip::

    阅读有关 :doc:`扩展Twig <advanced>` 的章节，了解如何创建自己的扩展。

核心扩展
~~~~~~~~~~~~~~

``core`` 扩展定义Twig的所有核心特性：

* :doc:`标签 <tags/index>`;
* :doc:`过滤器 <filters/index>`;
* :doc:`函数 <functions/index>`;
* :doc:`测试 <tests/index>`.

转义器扩展
~~~~~~~~~~~~~~~~~

``escaper`` 扩展为Twig添加了自动输出转义。它定义了 ``autoescape`` 标签和 ``raw`` 过滤器。

在创建转义器扩展时，你可以打开或者关闭全局输出转义策略::

    $escaper = new \Twig\Extension\EscaperExtension('html');
    $twig->addExtension($escaper);

如果将其设置为 ``html``，模板中的所有变量都会被转义（使用 ``html`` 转义策略）, 除非是用了 ``raw`` 过滤器：

.. code-block:: twig

    {{ article.to_html|raw }}

还可以使用 ``autoescape`` 标签来局部地改变转义模式：

.. code-block:: twig

    {% autoescape 'html' %}
        {{ var }}
        {{ var|raw }}      {# var won't be escaped #}
        {{ var|escape }}   {# var won't be double-escaped #}
    {% endautoescape %}

.. warning::

    ``autoescape`` 标签对引入的文件没有影响。

像下面这样实现转义规则：

* 在模板中直接用作变量或过滤器参数的字面值（包括整型数、布尔值、数组等）从不自动转义：

  .. code-block:: html+twig

        {{ "Twig<br/>" }} {# 不转义 #}

        {% set text = "Twig<br/>" %}
        {{ text }} {# 将转义 #}

* 结果是字面值或标记为安全的变量的表达式永远不会自动转义：

  .. code-block:: html+twig

        {{ foo ? "Twig<br/>" : "<br/>Twig" }} {# 不会转义 #}

        {% set text = "Twig<br/>" %}
        {{ true ? text : "<br/>Twig" }} {# 不会转义 #}
        {{ false ? text : "<br/>Twig" }} {# 不会转义 #}

        {% set text = "Twig<br/>" %}
        {{ foo ? text|raw : "<br/>Twig" }} {# 不会转义 #}

* 具有 ``__toString`` 方法的对象被转换为字符串并转义。你可以通过
  ``EscaperExtension::addSafeClass()`` 将某些类或接口标记为对某些策略是安全的：

  .. code-block:: twig

        // 将 Foo 类的对象标记为对 HTML 策略是安全的
        $escaper->addSafeClass('Foo', ['html']);

        // 将接口 Foo 的对象标记为对 HTML 策略是安全的
        $escaper->addSafeClass('FooInterface', ['html']);

        // 将 Foo 类的对象标记为对 HTML 和 JS 策略是安全的
        $escaper->addSafeClass('Foo', ['html', 'js']);

        // 将 Foo 类的对象标记为对所有策略是安全的
        $escaper->addSafeClass('Foo', ['all']);

* 转义应用于打印之前，其他过滤器应用之后：

  .. code-block:: twig

        {{ var|upper }} {# 等同于 {{ var|upper|escape }} #}

* `raw` 过滤器只能用在过滤器链的结尾：

  .. code-block:: twig

        {{ var|raw|upper }} {# 将会转义 #}

        {{ var|upper|raw }} {# 不会转义 #}

* 如果当前上下文（例如 ``html`` 或
  ``js``）的过滤器链中最后一个过滤器被标注为安全，那么自动转义不会被应用。
  ``escape`` 和 ``escape('html')`` 用于将HTML标注为安全，``escape('js')``
  用于将JavaScript标注为安全，``raw`` 可以将任意内容标注为安全：

  .. code-block:: twig

        {% autoescape 'js' %}
            {{ var|escape('html') }} {# 将针对HTML和JavaScript进行转义 #}
            {{ var }} {# 将被转义为JavaScript #}
            {{ var|escape('js') }} {# 不会双重转义 #}
        {% endautoescape %}

.. note::

    自动转义有一些局限性，因为针对表达式的转义是在评估之后才应用的。
    举个例子，在处理连接时，``{{ foo|raw ~ bar }}``
    不会给出预期结果，因为转义是应用于连接的结果上的，而不是应用在单个变量上（所以
    ``raw`` 过滤器此时不会生效）。

沙盒扩展
~~~~~~~~~~~~~~~~~

``sandbox`` 扩展用于评估未被信任的代码。禁止访问不安全的属性和方法。
沙盒的安全性由一个策略实例进行管理。
默认地，Twig带有一个策略类：``\Twig\Sandbox\SecurityPolicy``。
这个类允许你为标签、过滤器、属性以及方法添加白名单::

    $tags = ['if'];
    $filters = ['upper'];
    $methods = [
        'Article' => ['getTitle', 'getBody'],
    ];
    $properties = [
        'Article' => ['title', 'body'],
    ];
    $functions = ['range'];
    $policy = new \Twig\Sandbox\SecurityPolicy($tags, $filters, $methods, $properties, $functions);

基于上述配置，安全策略仅允许使用 ``if`` 标签和 ``upper`` 过滤器。
而且，模板只能调用 ``Article`` 对象上的 ``getTitle()``、``getBody()``
方法以及 ``title``、``body`` 共有属性。
其它的用法都被禁止，并会生成一个 ``\Twig\Sandbox\SecurityError`` 异常。

策略对象是沙盒的构造函数的第一个参数::

    $sandbox = new \Twig\Extension\SandboxExtension($policy);
    $twig->addExtension($sandbox);

默认情况下，沙盒模式是被禁用了的。但在使用 ``sandbox`` 标签引入未被信任的模板代码时，将会启用沙盒模式：

.. code-block:: twig

    {% sandbox %}
        {% include 'user.html' %}
    {% endsandbox %}

可以将该扩展的构造函数的第二个参数设置为 ``true``，以将所有模板放入沙盒中::

    $sandbox = new \Twig\Extension\SandboxExtension($policy, true);

分析器扩展
~~~~~~~~~~~~~~~~~~

``profiler`` 扩展为Twig模板启用了一个分析器；由于它增加了一些开销，所以只能在开发环境中使用::

    $profile = new \Twig\Profiler\Profile();
    $twig->addExtension(new \Twig\Extension\ProfilerExtension($profile));

    $dumper = new \Twig\Profiler\Dumper\TextDumper();
    echo $dumper->dump($profile);

一份分析结果包含了模板、代码区块以及宏执行的时间和内存消耗等信息。

可以将分析数据转换成一个与 `Blackfire.io <https://blackfire.io/>`_ 兼容的格式::

    $dumper = new \Twig\Profiler\Dumper\BlackfireDumper();
    file_put_contents('/path/to/profile.prof', $dumper->dump($profile));

将分析结果上传，使其可视化（需要先创建一个 `免费账号 <https://blackfire.io/signup?utm_source=twig&utm_medium=doc&utm_campaign=profiler>`_ ）：

.. code-block:: sh

    blackfire --slot=7 upload /path/to/profile.prof

优化器扩展
~~~~~~~~~~~~~~~~~~~

``optimizer`` 扩展在编译前优化节点树::

    $twig->addExtension(new \Twig\Extension\OptimizerExtension());

默认地，所有优化项都是开启了的。你可以通过将某些你想要启用的优化项传递给构造函数，以开启它们::

    $optimizer = new \Twig\Extension\OptimizerExtension(\Twig\NodeVisitor\OptimizerNodeVisitor::OPTIMIZE_FOR);

    $twig->addExtension($optimizer);

Twig 支持以下优化项：

* ``\Twig\NodeVisitor\OptimizerNodeVisitor::OPTIMIZE_ALL``，启用所有优化项（这是默认值）
* ``\Twig\NodeVisitor\OptimizerNodeVisitor::OPTIMIZE_NONE``，禁用所有优化项。
  这会减少编译时间，但会增加执行时间和内存消耗。
* ``\Twig\NodeVisitor\OptimizerNodeVisitor::OPTIMIZE_FOR``，如果可能，则通过移除
   ``loop`` 变量的创建来优化 ``for`` 标签。
* ``\Twig\NodeVisitor\OptimizerNodeVisitor::OPTIMIZE_RAW_FILTER``，如果可能，则移除 ``raw`` 过滤器。
* ``\Twig\NodeVisitor\OptimizerNodeVisitor::OPTIMIZE_VAR_ACCESS``，如果可能，则简化已编译模板中变量的创建和访问。

异常
----------

Twig可以抛出异常：

* ``\Twig\Error\Error``: 所有错误的基础异常。

* ``\Twig\Error\SyntaxError``: 抛出此异常，表示模板语法存在问题。

* ``\Twig\Error\RuntimeError``: 在运行时发生了某个错误，则抛出这个异常（比如某个过滤器并不存在）。

* ``\Twig\Error\LoaderError``: 在模板加载过程中发生了某个错误，则抛出此异常。

* ``\Twig\Sandbox\SecurityError``: 在沙盒化的模板中调用了某个未被允许的标签、过滤器或方法时，抛出此异常。
