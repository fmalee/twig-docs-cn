``locale_name``
===============

``locale_name`` 过滤器返回给定其两个字母代码的区域名称：

.. code-block:: twig

    {# German #}
    {{ 'de'|locale_name }}

默认情况下，过滤器使用当前的区域设置。您可以显式地传递它：

.. code-block:: twig

    {# allemand #}
    {{ 'de'|locale_name('fr') }}

    {# français (Canada) #}
    {{ 'fr_CA'|locale_name('fr_FR') }}

.. note::

    ``locale_name`` 过滤器是默认情况下未安装的 ``IntlExtension`` 的一部分。首先安装它：

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
