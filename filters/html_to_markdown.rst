``html_to_markdown``
====================

.. versionadded:: 2.12

    Twig 2.12中添加了 ``html_to_markdown`` 过滤器。

``html_to_markdown`` 过滤器将HTML区块转换为Markdown：

.. code-block:: html+twig

    {% apply html_to_markdown %}
        <html>
            <h1>Hello!</h1>
        </html>
    {% endapply %}

您还可以对 ``include`` 的整个模板使用过滤器：

.. code-block:: twig

    {{ include('some_template.html.twig')|html_to_markdown }}

.. note::

    ``html_to_markdown`` 过滤器是默认情况下未安装的 ``MarkdownExtension`` 的一部分。首先安装它：

    .. code-block:: bash

        $ composer require twig/markdown-extra

    在Symfony项目上，您可以通过安装 ``twig/extra-bundle`` 来自动启用它：

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

``html_to_markdown`` 只是一个前端；实际转换由以下兼容库之一完成，您可以从中选择：

* `league/html-to-markdown`_
* `michelf/php-markdown`_
* `erusev/parsedown`_

根据库的不同，还可以通过将一些选项作为参数传递给过滤器来添加它们。
``league/html-to-markdown`` 示例：

.. code-block:: html+twig

    {% apply html_to_markdown({hard_break: false}) %}
        <html>
            <h1>Hello!</h1>
        </html>
    {% endapply %}
    
.. _league/html-to-markdown: https://github.com/thephpleague/html-to-markdown
.. _michelf/php-markdown: https://github.com/michelf/php-markdown
.. _erusev/parsedown: https://github.com/erusev/parsedown
