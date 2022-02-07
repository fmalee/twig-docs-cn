``slug``
========

``slug`` 过滤器将给定字符串转换为另一个仅包含安全ASCII字符的字符串。

下面是一个例子：

.. code-block:: twig

    {{ 'Wôrķšƥáçè ~~sèťtïñğš~~'|slug }}
    Workspace-settings

单词之间的默认分隔符是破折号（``-``），但您可以通过将其作为参数传递来定义所选的选择器：

.. code-block:: twig

    {{ 'Wôrķšƥáçè ~~sèťtïñğš~~'|slug('/') }}
    Workspace/settings

过滤器会自动检测原始字符串的语言，但也可以使用第二个参数显式指定：

.. code-block:: twig

    {{ '...'|slug('-', 'ko') }}

``slug`` 过滤器使用的是Symfony的 `AsciiSlugger <https://symfony.com/doc/current/components/string.html#slugger>`_ 中相同名称的方法。

.. note::

    ``slug`` 过滤器是默认情况下未安装的 ``StringExtension`` 的一部分。首先安装它：

    .. code-block:: bash

        $ composer require twig/string-extra

    然后，在Symfony项目上，安装 ``twig/extra-bundle``：

    .. code-block:: bash

        $ composer require twig/extra-bundle

    或者，在Twig环境上显式添加该扩展::

        use Twig\Extra\String\StringExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new StringExtension());

参数
---------

* ``separator``: 用于连接单词的分隔符（默认为 ``-``）
* ``locale``: 原始字符串的区域设置（如果未指定，将自动检测）
