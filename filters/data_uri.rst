``data_uri``
============

.. versionadded:: 2.12

    Twig 2.12中添加了 ``data_uri`` 过滤器。

``data_uri`` 过滤器使用 `RFC 2397`_ 中定义的数据方案来生成URL：

.. code-block:: html+twig

    {{ image_data|data_uri }}

    {{ source('path_to_image')|data_uri }}

    {# 强制mime类型，禁用对mime类型的猜测 #}
    {{ image_data|data_uri(mime="image/svg") }}

    {# 也适用于纯文本 #}
    {{ '<b>foobar</b>'|data_uri(mime="text/html") }}

    {# 添加一些额外的参数 #}
    {{ '<b>foobar</b>'|data_uri(mime="text/html", parameters={charset: "ascii"}) }}

.. note::

    ``data_uri`` 过滤器是默认情况下未安装的 ``HtmlExtension`` 的一部分。首先安装它：

    .. code-block:: bash

        $ composer require twig/html-extra

    然后，在Symfony项目上，安装 ``twig/extra-bundle``：

    .. code-block:: bash

        $ composer require twig/extra-bundle

    或者，在Twig环境上显式添加该扩展::

        use Twig\Extra\Html\HtmlExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new HtmlExtension());

.. note::

    筛选器不会特意地执行任何长度验证（限制取决于使用上下文），应在调用此筛选器之前进行验证。

参数
---------

* ``mime``: mime类型
* ``parameters``: 参数数组

.. _RFC 2397: https://tools.ietf.org/html/rfc2397
