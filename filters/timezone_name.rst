``timezone_name``
=================

.. versionadded:: 2.12

    Twig 2.12中添加了 ``timezone_name`` 过滤器。

``timezone_name`` 过滤器返回给定时区标识符的时区名称：

.. code-block:: twig

    {# Central European Time (Paris) #}
    {{ 'Europe/Paris'|timezone_name }}

    {# Pacific Time (Los Angeles) #}
    {{ 'America/Los_Angeles'|timezone_name }}

默认情况下，过滤器使用当前的区域设置。您可以显式地传递它：

.. code-block:: twig

    {# heure du Pacifique nord-américain (Los Angeles) #}
    {{ 'America/Los_Angeles'|timezone_name('fr') }}

.. note::

    ``timezone_name`` 过滤器是默认情况下未安装的 ``IntlExtension`` 的一部分。首先安装它：

    .. code-block:: bash

        $ composer require twig/intl-extra

    然后，在Symfony项目上，安装 ``twig/extra-bundle``：

    .. code-block:: bash

        $ composer require twig/extra-bundle

    或者，在Twig环境上显式添加该扩展::

        use Twig\Extra\Intl\IntlExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new IntlExtension());

参数
---------

* ``locale``: 语言区域
