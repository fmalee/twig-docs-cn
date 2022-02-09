``country_timezones``
=====================

``country_timezones`` 函数返回与给定国家代码关联的时区名称：

.. code-block:: twig

    {# Europe/Paris #}
    {{ country_timezones('FR')|join(', ') }}

.. note::

    ``country_timezones`` 函数是默认情况下未安装的 ``IntlExtension`` 的一部分。首先安装它：

    .. code-block:: bash

        $ composer require twig/intl-extra

    然后，在Symfony项目上，安装 ``twig/extra-bundle``：

    .. code-block:: bash

        $ composer require twig/extra-bundle

    或者，在Twig环境上显式添加该扩展::

        use Twig\Extra\Intl\IntlExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new IntlExtension());
