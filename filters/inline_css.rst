``inline_css``
==============

.. versionadded:: 2.12

    Twig 2.12中添加了 ``inline_css`` 过滤器。

``inline_css`` 过滤器将CSS样式内联到HTML文档中：

.. code-block:: html+twig

    {% apply inline_css %}
        <html>
            <head>
                <style>
                    p { color: red; }
                </style>
            </head>
            <body>
                <p>Hello CSS!</p>
            </body>
        </html>
    {% endapply %}

您还可以通过将一些样式表作为参数传递给过滤器来添加它们：

.. code-block:: html+twig

    {% apply inline_css(source("some_styles.css"), source("another.css")) %}
        <html>
            <body>
                <p>Hello CSS!</p>
            </body>
        </html>
    {% endapply %}

通过过滤器加载的样式会覆盖HTML文档的 ``<style>`` 标签中定义的样式。

您还可以对引入的文件使用过滤器：

.. code-block:: twig

    {{ include('some_template.html.twig')|inline_css }}

    {{ include('some_template.html.twig')|inline_css(source("some_styles.css")) }}

请注意，CSS内联适用于整个HTML文档，而不是片段。

.. note::

    ``inline_css`` 过滤器是默认情况下未安装的 ``CssInlinerExtension`` 的一部分。首先安装它：

    .. code-block:: bash

        $ composer require twig/cssinliner-extra

     然后，在Symfony项目上，安装 ``twig/extra-bundle``：

    .. code-block:: bash

        $ composer require twig/extra-bundle

    或者，在Twig环境上显式添加该扩展::

        use Twig\Extra\CssInliner\CssInlinerExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new CssInlinerExtension());
