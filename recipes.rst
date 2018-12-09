使用技巧
========

.. _deprecation-notices:

显示弃用通知
------------------------------

被弃用的特性会产生弃用通知（通过调用PHP的 ``trigger_error()`` 函数）。
默认地，弃用通知是沉默的，不会显示或记录。

要便捷地删除模版中的所有已弃用的特性用法，需要编写并运行以下脚本::

    require_once __DIR__.'/vendor/autoload.php';

    $twig = create_your_twig_env();

    $deprecations = new Twig_Util_DeprecationCollector($twig);

    print_r($deprecations->collectDir(__DIR__.'/templates'));

其中的 ``collectDir()`` 方法编译目录中找到的所有模版，捕获并返回弃用通知。

.. tip::

    If your templates are not stored on the filesystem, use the ``collect()``
    method instead. ``collect()`` takes a ``Traversable`` which must return
    template names as keys and template contents as values (as done by
    ``Twig_Util_TemplateDirIterator``).
    如果你的模版并不存放在文件系统中，使用搭配了迭代器Iterator的collect()方法。迭代器必须返回以模板名为键，模版内容为值的结果（由Twig_Util_TemplateDirIterator完成处理）。

然而，这些代码并不能找到所有的弃用（比如使用弃用了的一些Twig类）。
若要捕获所有弃用通知，需要像下面这页注册一个自定义的错误处理器：
However, this code won't find all deprecations (like using deprecated some Twig
classes). To catch all notices, register a custom error handler like the one
below::

    $deprecations = array();
    set_error_handler(function ($type, $msg) use (&$deprecations) {
        if (E_USER_DEPRECATED === $type) {
            $deprecations[] = $msg;
        }
    });

    // run your application

    print_r($deprecations);

注意，大多数弃用通知是在编译过程中触发的，所以它们不会在模版已缓存时生成。
Note that most deprecation notices are triggered during **compilation**, so
they won't be generated when templates are already cached.

.. tip::

    如果要管理PHPUnit测试中的弃用通知，请查看
    `symfony/phpunit-bridge <https://github.com/symfony/phpunit-bridge>`_
    软件包，这样可以简化该过程。

使用布局条件语句
---------------------------

使用Ajax意味着相同的内容有时是以本身的样子显示，有时是被一个布局装饰的。
由于Twig布局模板的名称可以是任意有效的表达式，当通过Ajax传递请求时，可以传递一个被解析为
``true`` 的变量，然后选择相应的布局：

.. code-block:: jinja

    {% extends request.ajax ? "base_ajax.html" : "base.html" %}

    {% block content %}
        This is the content to be displayed.
    {% endblock %}

使用动态的引入
-------------------------

在引入一个模板时，它的名称不必是一个字符串。举个例子，被引入的模板名可以依赖变量的值：

.. code-block:: jinja

    {% include var ~ '_foo.html' %}

如果 ``var`` 解析为 ``index``，则 ``index_foo.html`` 模板将被渲染。

事实上，模板名称可以是任意有效的表达式，就像下面这样：

.. code-block:: jinja

    {% include var|default('index') ~ '_foo.html' %}

Overriding a Template that also extends itself
重写也扩展自己的模板
----------------------------------------------

可以通过两种方式来自定义模板：

* *继承*: 一个模板扩展一个父模板，并重写一些区块。

* *替换*: 如果你用到了文件系统加载器，Twig会加载目录列表中找到的第一个模板；在一个目录中找到的一个模板
  *替换* 列表中其他目录中的另一个模板。

但是你如何将两者结合起来：替换一个也扩展自己的模板（也就是列表中其他目录中的模板）？
But how do you combine both: *replace* a template that also extends itself
(aka a template in a directory further in the list)?

比方说，你的模板是从两个加载.../templates/mysite 和.../templates/default顺序。该page.twig模板存储在.../templates/default内容如下：
Let's say that your templates are loaded from both ``.../templates/mysite``
and ``.../templates/default`` in this order. The ``page.twig`` template,
stored in ``.../templates/default`` reads as follows:

.. code-block:: jinja

    {# page.twig #}
    {% extends "layout.twig" %}

    {% block content %}
    {% endblock %}

你可以通过放入具有相同名称的文件来替换此模板 .../templates/mysite。如果你想扩展原始模板，可能会写下以下内容：
You can replace this template by putting a file with the same name in
``.../templates/mysite``. And if you want to extend the original template, you
might be tempted to write the following:

.. code-block:: jinja

    {# page.twig in .../templates/mysite #}
    {% extends "page.twig" %} {# from .../templates/default #}

当然，这不起作用，因为Twig将始终从中加载模板 .../templates/mysite。
Of course, this will not work as Twig will always load the template from
``.../templates/mysite``.

事实证明，可以通过在模板目录的末尾添加一个目录来实现这一点，该目录是所有其他目录的父目录：.../templates在我们的例子中。这样可以使我们系统中的每个模板文件都具有唯一的可寻址性。大多数情况下，你将使用“普通”路径，但在特殊情况下，想要使用覆盖版本本身扩展模板，我们可以在extends标记中引用其父级的完整，明确的模板路径：
It turns out it is possible to get this to work, by adding a directory right
at the end of your template directories, which is the parent of all of the
other directories: ``.../templates`` in our case. This has the effect of
making every template file within our system uniquely addressable. Most of the
time you will use the "normal" paths, but in the special case of wanting to
extend a template with an overriding version of itself we can reference its
parent's full, unambiguous template path in the extends tag:

.. code-block:: jinja

    {# page.twig in .../templates/mysite #}
    {% extends "default/page.twig" %} {# from .../templates #}

.. note::

    这个技巧的灵感来自以下Django wiki页面：
    https://code.djangoproject.com/wiki/ExtendingTemplates

自定义语法
----------------------

Twig允许对块分隔符进行一些语法自定义。
建议不要使用此功能，因为模板将与你的自定义语法绑定。但对于特定项目，更改默认值是有意义的。
Twig allows some syntax customization for the block delimiters. It's not
recommended to use this feature as templates will be tied with your custom
syntax. But for specific projects, it can make sense to change the defaults.

要改变代码块分隔符，你必须创建你自己的词法分析程序对象：
To change the block delimiters, you need to create your own lexer object::

    $twig = new Twig_Environment(...);

    $lexer = new Twig_Lexer($twig, array(
        'tag_comment'   => array('{#', '#}'),
        'tag_block'     => array('{%', '%}'),
        'tag_variable'  => array('{{', '}}'),
        'interpolation' => array('#{', '}'),
    ));
    $twig->setLexer($lexer);

这里有一些模仿其它模板引擎语法的配置示例：
Here are some configuration example that simulates some other template engines
syntax::

    // Ruby erb syntax
    $lexer = new Twig_Lexer($twig, array(
        'tag_comment'  => array('<%#', '%>'),
        'tag_block'    => array('<%', '%>'),
        'tag_variable' => array('<%=', '%>'),
    ));

    // SGML Comment Syntax
    $lexer = new Twig_Lexer($twig, array(
        'tag_comment'  => array('<!--#', '-->'),
        'tag_block'    => array('<!--', '-->'),
        'tag_variable' => array('${', '}'),
    ));

    // Smarty like
    $lexer = new Twig_Lexer($twig, array(
        'tag_comment'  => array('{*', '*}'),
        'tag_block'    => array('{', '}'),
        'tag_variable' => array('{$', '}'),
    ));

使用动态的对象属性
-------------------------------

当Twig遇到一个像article.title这样的变量，它会尝试在article对象中寻找
title公共属性。
When Twig encounters a variable like ``article.title``, it tries to find a
``title`` public property in the ``article`` object.

如果使用了__get()魔术方法进行动态定义，即使该公共属性并不存在，Twig依然会进行上面所述的工作。你只需要像下面这段代码所示，再实现一下 __isset() 魔术方法即可：
It also works if the property does not exist but is rather defined dynamically
thanks to the magic ``__get()`` method; you just need to also implement the
``__isset()`` magic method like shown in the following snippet of code::

    class Article
    {
        public function __get($name)
        {
            if ('title' == $name) {
                return 'The title';
            }

            // throw some kind of error
        }

        public function __isset($name)
        {
            if ('title' == $name) {
                return true;
            }

            return false;
        }
    }

在嵌套的循环中访问父级上下文
--------------------------------------------

有时，当我们在使用嵌套的循环时，需要访问父级上下文。
父级上下文可以使用 ``loop.parent`` 变量来访问。举个例子，如果你有下面这样的模板数据::

    $data = array(
        'topics' => array(
            'topic1' => array('Message 1 of topic 1', 'Message 2 of topic 1'),
            'topic2' => array('Message 1 of topic 2', 'Message 2 of topic 2'),
        ),
    );

然后，下面这个模板将在所有话题中显示所有信息：

.. code-block:: jinja

    {% for topic, messages in topics %}
        * {{ loop.index }}: {{ topic }}
      {% for message in messages %}
          - {{ loop.parent.loop.index }}.{{ loop.index }}: {{ message }}
      {% endfor %}
    {% endfor %}

输出的结果类似这样：

.. code-block:: text

    * 1: topic1
      - 1.1: The message 1 of topic 1
      - 1.2: The message 2 of topic 1
    * 2: topic2
      - 2.1: The message 1 of topic 2
      - 2.2: The message 2 of topic 2

在内部的循环中，``loop.parent`` 用于访问外部上下文。
所以在外部循环中定义的当前 ``topic`` 的索引可以通过 ``loop.parent.loop.index`` 访问。

动态定义未定义的函数和过滤器
---------------------------------------------------

如果一个函数或过滤器未被定义，Twig默认地会抛出一个 ``Twig_Error_Syntax`` 异常。
然而，还可以调用一个能返回函数或过滤器的`回调`_（任意有效的PHP可调用对象）。

对于过滤器，使用 ``registerUndefinedFilterCallback()`` 来注册回调。
对于函数，则使用 ``registerUndefinedFunctionCallback()``::

    // auto-register all native PHP functions as Twig functions
    // don't try this at home as it's not secure at all!
    $twig->registerUndefinedFunctionCallback(function ($name) {
        if (function_exists($name)) {
            return new Twig_Function($name, $name);
        }

        return false;
    });

如果该可调用对象不能返回有效的函数或过滤器，它必须返回 ``false``。

如果你注册了超过一个回调，Twig将轮流调用它们直到不再返回 ``false``。

.. tip::

    由于函数和过滤器的解析在编译期间就已完成，所以注册这些回调时并不会产生额外的开销。

验证模板语法
------------------------------

当模板代码是由第三方（比如通过web接口）提供的，那么就需要再保存它之前进行模板语法验证。如果模板代码是存放在$template变量中的，你可以这样处理：
When template code is provided by a third-party (through a web interface for
instance), it might be interesting to validate the template syntax before
saving it. If the template code is stored in a `$template` variable, here is
how you can do it::

    try {
        $twig->parse($twig->tokenize(new Twig_Source($template)));

        // the $template is valid
    } catch (Twig_Error_Syntax $e) {
        // $template contains one or more syntax errors
    }

如果你遍历了一组文件，可以将文件名传递给 tokenize() 方法，用来从异常信息中获取文件名：
If you iterate over a set of files, you can pass the filename to the
``tokenize()`` method to get the filename in the exception message::

    foreach ($files as $file) {
        try {
            $twig->parse($twig->tokenize(new Twig_Source($template, $file->getFilename(), $file)));

            // the $template is valid
        } catch (Twig_Error_Syntax $e) {
            // $template contains one or more syntax errors
        }
    }

.. note::

    This method won't catch any sandbox policy violations because the policy
    is enforced during template rendering (as Twig needs the context for some
    checks like allowed methods on objects).
    这个方法不会捕获任何沙盒策略的违规，因为沙河策略是在模板渲染过程中执行的（因为Twig需要对context进行一些检查，比如已被允许的对象方法）。

启用OPcache或APC后刷新修改的模板
------------------------------------------------------------

在使用opcache.validate_timestamps 设为 0的OPcache，或apc.stat设为0的APC，并且Twig 缓存被启用的情况下，清除模板缓存并不会更新缓存。
When using OPcache with ``opcache.validate_timestamps`` set to ``0`` or APC
with ``apc.stat`` set to ``0`` and Twig cache enabled, clearing the template
cache won't update the cache.

为了解决这个问题，需要强行让Twig将字节码缓存无效化：
To get around this, force Twig to invalidate the bytecode cache::

    $twig = new Twig_Environment($loader, array(
        'cache' => new Twig_Cache_Filesystem('/some/cache/path', Twig_Cache_Filesystem::FORCE_BYTECODE_INVALIDATION),
        // ...
    ));

重用有状态的节点访问器
-------------------------------

将访问者附加到Twig_Environment实例时，Twig使用它来访问它编译的所有模板。如果你需要保留一些状态信息，你可能希望在访问新模板时重置它。
When attaching a visitor to a ``Twig_Environment`` instance, Twig uses it to
visit *all* templates it compiles. If you need to keep some state information
around, you probably want to reset it when visiting a new template.

使用以下代码可以轻松实现此目的：
This can be easily achieved with the following code::

    protected $someTemplateState = array();

    public function enterNode(Twig_Node $node, Twig_Environment $env)
    {
        if ($node instanceof Twig_Node_Module) {
            // reset the state as we are entering a new template
            $this->someTemplateState = array();
        }

        // ...

        return $node;
    }

用数据库来存放模板
-----------------------------------

如果你在开发一款CMS，模板通常被存放在数据库里。
这里为你提供一个简单的PDO模板加载器，你可以以之为基础创建你自己的加载器。

首先创建一个临时的内存数据库SQLite3::

    $dbh = new PDO('sqlite::memory:');
    $dbh->exec('CREATE TABLE templates (name STRING, source STRING, last_modified INTEGER)');
    $base = '{% block content %}{% endblock %}';
    $index = '
    {% extends "base.twig" %}
    {% block content %}Hello {{ name }}{% endblock %}
    ';
    $now = time();
    $dbh->exec("INSERT INTO templates (name, source, last_modified) VALUES ('base.twig', '$base', $now)");
    $dbh->exec("INSERT INTO templates (name, source, last_modified) VALUES ('index.twig', '$index', $now)");

我们创建了一个简单的 ``templates`` 表，它寄存了两个模板：``base.twig`` 和 ``index.twig``。

现在，定义一个能使用这个数据库的加载器::

    class DatabaseTwigLoader implements Twig_LoaderInterface
    {
        protected $dbh;

        public function __construct(PDO $dbh)
        {
            $this->dbh = $dbh;
        }

        public function getSourceContext($name)
        {
            if (false === $source = $this->getValue('source', $name)) {
                throw new Twig_Error_Loader(sprintf('Template "%s" does not exist.', $name));
            }

            return new Twig_Source($source, $name);
        }

        public function exists($name)
        {
            return $name === $this->getValue('name', $name);
        }

        public function getCacheKey($name)
        {
            return $name;
        }

        public function isFresh($name, $time)
        {
            if (false === $lastModified = $this->getValue('last_modified', $name)) {
                return false;
            }

            return $lastModified <= $time;
        }

        protected function getValue($column, $name)
        {
            $sth = $this->dbh->prepare('SELECT '.$column.' FROM templates WHERE name = :name');
            $sth->execute(array(':name' => (string) $name));

            return $sth->fetchColumn();
        }
    }

最后，这是一个关于如何使用它的例子::

    $loader = new DatabaseTwigLoader($dbh);
    $twig = new Twig_Environment($loader);

    echo $twig->render('index.twig', array('name' => 'Fabien'));

使用不同的模板来源
--------------------------------

这是前面一条使用技巧的延续。即使你已经将模板存储在数据库中，你可能还希望能将原始的基础模板存放在文件系统中。要想从不同的来源加载模板，你需要使用 Twig_Loader_Chain 加载器。
This recipe is the continuation of the previous one. Even if you store the
contributed templates in a database, you might want to keep the original/base
templates on the filesystem. When templates can be loaded from different
sources, you need to use the ``Twig_Loader_Chain`` loader.

正如你在上一个配方中看到的那样，我们以与使用常规文件系统加载器完成相同的方式引用模板。这是能够混合和匹配来自数据库，文件系统或任何其他加载器的模板的关键：模板名称应该是逻辑名称，而不是文件系统的路径：
As you can see in the previous recipe, we reference the template in the exact
same way as we would have done it with a regular filesystem loader. This is
the key to be able to mix and match templates coming from the database, the
filesystem, or any other loader for that matter: the template name should be a
logical name, and not the path from the filesystem::

    $loader1 = new DatabaseTwigLoader($dbh);
    $loader2 = new Twig_Loader_Array(array(
        'base.twig' => '{% block content %}{% endblock %}',
    ));
    $loader = new Twig_Loader_Chain(array($loader1, $loader2));

    $twig = new Twig_Environment($loader);

    echo $twig->render('index.twig', array('name' => 'Fabien'));

现在base.twig模板是在数组加载器中定义的，你可以将其从数据库中删除，其他所有内容仍然可以像以前一样工作。
Now that the ``base.twig`` templates is defined in an array loader, you can
remove it from the database, and everything else will still work as before.

从字符串中加载模版
--------------------------------

对于模板，你可以使用template_from_string函数轻松加载储存在字符串中的模版（需要Twig_Extension_StringLoader扩展的支持，在Twig1.1以上可用）：
From a template, you can easily load a template stored in a string via the
``template_from_string`` function (via the ``Twig_Extension_StringLoader``
extension):

.. code-block:: jinja

    {{ include(template_from_string("Hello {{ name }}")) }}

对于PHP，同样可以通过Twig_Environment::createTemplate()来加载存储再字符串中的模板（Twig 1.18以上可用）：
From PHP, it's also possible to load a template stored in a string via
``Twig_Environment::createTemplate()``::

    $template = $twig->createTemplate('hello {{ name }}');
    echo $template->render(array('name' => 'Fabien'));

在同一个模板中使用的Twig和AngularJS
----------------------------------------------

建议不要在同一文件中混合使用不同的模板语法，因为AngularJS和Twig在语法中使用相同的分隔符： {{和}}。
Mixing different template syntaxes in the same file is not a recommended
practice as both AngularJS and Twig use the same delimiters in their syntax:
``{{`` and ``}}``.

尽管如此，如果你想在同一个模板中使用AngularJS和Twig，有两种方法可以让它工作，具体取决于你需要在模板中包含的AngularJS的数量：
Still, if you want to use AngularJS and Twig in the same template, there are
two ways to make it work depending on the amount of AngularJS you need to
include in your templates:

* Escaping the AngularJS delimiters by wrapping AngularJS sections with the
  ``{% verbatim %}`` tag or by escaping each delimiter via ``{{ '{{' }}`` and
  ``{{ '}}' }}``;
  通过使用{% verbatim %}标记包装AngularJS部分或通过{{ '{{' }}和 转义每个分隔符来转义AngularJS分隔 符{{ '}}' }};

* Changing the delimiters of one of the template engines (depending on which
  engine you introduced last):
  更改其中一个模板引擎的分隔符（取决于你最后介绍的引擎）：

  * 对于AngularJS，使用 ``interpolateProvider`` 服务更改插值标签，例如在模块初始化时：

    ..  code-block:: javascript

        angular.module('myApp', []).config(function($interpolateProvider) {
            $interpolateProvider.startSymbol('{[').endSymbol(']}');
        });

  * 对于Twig，可以通过 ``tag_variable`` 词法分析器选项来更改分隔符：

    ..  code-block:: php

        $env->setLexer(new Twig_Lexer($env, array(
            'tag_variable' => array('{[', ']}'),
        )));

.. _回调: https://secure.php.net/manual/en/function.is-callable.php
