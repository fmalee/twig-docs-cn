``u``
=====

.. versionadded:: 2.12.1

    Twig 2.12.1中添加了 ``u`` 过滤器。

``u`` 过滤器将文本封装在一个 Unicode 对象（一个 `Symfony UnicodeString 实例
<https://symfony.com/doc/current/components/string.html>`_）中，该对象公开了“操作”字符串的方法。

让我们看看一些常见的用例。

将文本换行到给定的字符数：

.. code-block:: twig

    {{ 'Symfony String + Twig = <3'|u.wordwrap(5) }}
    Symfony
    String
    +
    Twig
    = <3

截断字符串：

.. code-block:: twig

    {{ 'Lorem ipsum'|u.truncate(8) }}
    Lorem ip

    {{ 'Lorem ipsum'|u.truncate(8, '...') }}
    Lorem...

``truncate`` 方法还接受第三个参数来保留整个单词：

.. code-block:: twig

    {{ 'Lorem ipsum dolor'|u.truncate(10, '...', false) }}
    Lorem ipsum...

将字符串转换为 *snake* 格式或 *camelCase*：

.. code-block:: twig

    {{ 'SymfonyStringWithTwig'|u.snake }}
    symfony_string_with_twig

    {{ 'symfony_string with twig'|u.camel.title }}
    SymfonyStringWithTwig

您还可以链接方法：

.. code-block:: twig

    {{ 'Symfony String + Twig = <3'|u.wordwrap(5).upper }}
    SYMFONY
    STRING
    +
    TWIG
    = <3

要操作大字符串，使用 ``apply`` 标签：

.. code-block:: twig

    {% apply u.wordwrap(5) %}
        大量的文本...
    {% endapply %}

.. note::

    ``u`` 过滤器是默认情况下未安装的 ``StringExtension`` 的一部分。首先安装它：

    .. code-block:: bash

        $ composer require twig/string-extra

    然后，在Symfony项目上，安装 ``twig/extra-bundle``：

    .. code-block:: bash

        $ composer require twig/extra-bundle

    或者，在Twig环境上显式添加该扩展::

        use Twig\Extra\String\StringExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new StringExtension());
