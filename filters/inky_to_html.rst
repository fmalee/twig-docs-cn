``inky_to_html``
================

.. versionadded:: 2.12

    Twig 2.12中添加了 ``inky_to_html`` 过滤器。

``inky_to_html`` 过滤器用于处理 `inky电子邮件模板 <https://github.com/zurb/inky>`_：

.. code-block:: html+twig

    {% apply inky_to_html %}
        <row>
            <columns large="6"></columns>
            <columns large="6"></columns>
        </row>
    {% endapply %}

您还可以对引入的文件使用过滤器：

.. code-block:: twig

    {{ include('some_template.inky.twig')|inky_to_html }}

.. note::

    ``inky_to_html`` 过滤器是默认情况下未安装的 ``InkyExtension`` 的一部分。首先安装它：

    .. code-block:: bash

        $ composer require twig/inky-extra

    然后，在Symfony项目上，安装 ``twig/extra-bundle``：

    .. code-block:: bash

        $ composer require twig/extra-bundle

    或者，在Twig环境上显式添加该扩展::

        use Twig\Extra\Inky\InkyExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new InkyExtension());
