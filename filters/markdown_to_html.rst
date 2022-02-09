``markdown_to_html``
====================

``markdown_to_html`` 过滤器将Markdown区块转换为HTML：

.. code-block:: twig

    {% apply markdown_to_html %}
    Title
    ======

    Hello!
    {% endapply %}

请注意，您可以缩进Markdown内容，因为在转换之前，前导空格将被统一删除：

.. code-block:: twig

    {% apply markdown_to_html %}
        Title
        ======

        Hello!
    {% endapply %}

还可以对引入的文件或变量使用过滤器：

.. code-block:: twig

    {{ include('some_template.markdown.twig')|markdown_to_html }}

    {{ changelog|markdown_to_html }}

.. note::

    ``markdown_to_html`` 过滤器是默认情况下未安装的 ``MarkdownExtension`` 的一部分。首先安装它：

    .. code-block:: bash

        $ composer require twig/markdown-extra

    然后，在Symfony项目上，安装 ``twig/extra-bundle``：

    .. code-block:: bash

        $ composer require twig/extra-bundle

    或者，在Twig环境上显式添加该扩展::

        use Twig\Extra\Markdown\MarkdownExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new MarkdownExtension());

    如果不使用Symfony，还必须注册扩展运行时::

        use Twig\Extra\Markdown\DefaultMarkdown;
        use Twig\Extra\Markdown\MarkdownRuntime;
        use Twig\RuntimeLoader\RuntimeLoaderInterface;

        $twig->addRuntimeLoader(new class implements RuntimeLoaderInterface {
            public function load($class) {
                if (MarkdownRuntime::class === $class) {
                    return new MarkdownRuntime(new DefaultMarkdown());
                }
            }
        });

    之后，需要安装您选择的markdown库。
    其中一些在 ``twig/markdown-extra`` 包的 ``require-dev`` 部分有说明。

.. note::

    如果使用 Symfony（全栈），并用 ``twig/extra-bundle`` 和 ``league/commonmark``
    作为你的 Markdown 库，你可以配置 CommonMark 扩展。将所需的扩展注册为服务，然后对服务使用
    ``twig.markdown.league_extension`` 标签。
