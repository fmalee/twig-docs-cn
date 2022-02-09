``format_date``
===============

``format_date`` 过滤器设置日期格式。它的行为方式与
:doc:`format_datetime<format_datetime>` 过滤器完全相同，但没有时间。

.. note::

    ``format_date`` 过滤器是默认情况下未安装的 ``IntlExtension`` 的一部分。首先安装它：

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
* ``dateFormat``: 日期格式
* ``pattern``: 日期时间模式
* ``timezone``: 日期时区
* ``calendar``: 日历（默认为公历）
