简介
============

欢迎来到Twig模板引擎的文档。Twig是一款灵活、快速、安全的PHP模板引擎。

Twig坚持PHP的原则，并为模板环境添加了有用的功能，使其同时保持对设计师和开发者友好。

Twig拥有以下关键特性：

* *快速*: Twig将模板编译为优化的原生PHP代码。它的开销与常规的PHP代码相比，已经降到了极低。

* *安全*: Twig拥有沙盒模式，用于评估未受信任的模板代码。这使得Twig可以用于允许用户自行修改模板设计的应用中。

* *灵活*: Twig由一个灵活的词法分析器(lexer)和解析器驱动。这使得开发者可以自定义标签和过滤器，并创建自己的DSL。

Twig已被用于许多开源项目，比如Symfony、Drupal8、eZPublish、phpBB、Matomo、OroCRM；并且许多框架也支持它，例如Slim、Yii、Laravel、Codeigniter以及Kohana — 就举这几个例子好了，有点多。

.. admonition:: Screencast

    喜欢从视频教程中学习吗？查看 `SymfonyCasts Twig教程`_！

先决条件
-------------

Twig 2.0 需至少需要 **PHP 7.2.5** 才能运行。

安装
------------

建议通过Composer来安装Twig：

.. code-block:: bash

    composer require "twig/twig:^2.0"

基础的API用法
---------------

本节为你简要介绍Twig的PHP API::

    require_once '/path/to/vendor/autoload.php';

    $loader = new \Twig\Loader\ArrayLoader([
        'index' => 'Hello {{ name }}!',
    ]);
    $twig = new \Twig\Environment($loader);

    echo $twig->render('index', ['name' => 'Fabien']);

Twig使用一个加载器（``\Twig\Loader\ArrayLoader``）来定位模板，以及一个环境（``\Twig\Environment``）来存储配置信息。

其中，``render()`` 方法通过其第一个参数载入模板，并通过第二个参数中的变量来渲染模板。

由于模板通常是存放在文件系统中的，Twig还有一个文件系统加载器::

    $loader = new \Twig\Loader\FilesystemLoader('/path/to/templates');
    $twig = new \Twig\Environment($loader, [
        'cache' => '/path/to/compilation_cache',
    ]);

    echo $twig->render('index.html', ['name' => 'Fabien']);

.. _`SymfonyCasts Twig教程`: https://symfonycasts.com/screencast/twig
