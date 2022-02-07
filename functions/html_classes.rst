``html_classes``
================

.. versionadded:: 2.12

    Twig 2.12中添加了 ``html_classes`` 函数。

``html_classes`` 函数通过有条件地将类名拼接在一起来返回字符串：

.. code-block:: html+twig

    <p class="{{ html_classes('a-class', 'another-class', {
        'errored': object.errored,
        'finished': object.finished,
        'pending': object.pending,
    }) }}">How are you doing?</p>

.. note::

    ``html_classes`` 函数是默认情况下未安装的 ``HtmlExtension`` 的一部分。首先安装它：

    .. code-block:: bash

        $ composer require twig/html-extra

    然后，在Symfony项目上，安装 ``twig/extra-bundle``：

    .. code-block:: bash

        $ composer require twig/extra-bundle

    或者，在Twig环境上显式添加该扩展::

        use Twig\Extra\Html\HtmlExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new HtmlExtension());
