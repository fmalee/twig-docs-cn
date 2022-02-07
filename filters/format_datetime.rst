``format_datetime``
===================

.. versionadded:: 2.12

    Twig 2.12中添加了 ``format_datetime`` 过滤器。

``format_datetime`` 过滤器格式化一个日期时间：

.. code-block:: twig

    {# Aug 7, 2019, 11:39:12 PM #}
    {{ '2019-08-07 23:39:12'|format_datetime() }}

可以调整日期部分和时间部分的输出：

.. code-block:: twig

    {# 23:39 #}
    {{ '2019-08-07 23:39:12'|format_datetime('none', 'short', locale='fr') }}

    {# 07/08/2019 #}
    {{ '2019-08-07 23:39:12'|format_datetime('short', 'none', locale='fr') }}

    {# mercredi 7 août 2019 23:39:12 UTC #}
    {{ '2019-08-07 23:39:12'|format_datetime('full', 'full', locale='fr') }}

支持的值： ``none``、 ``short``、 ``medium``、 ``long`` 以及 ``full``。

为了获得更大的灵活性，您甚至可以定义自己的模式（有关支持的模式，请参阅 `ICU用户指南 <https://unicode-org.github.io/icu/userguide/format_parse/datetime/#datetime-format-syntax>`_）。

.. code-block:: twig

    {# 11 oclock PM, GMT #}
    {{ '2019-08-07 23:39:12'|format_datetime(pattern="hh 'oclock' a, zzzz") }}

默认情况下，过滤器使用当前的区域设置。您可以显式地传递它：

.. code-block:: twig

    {# 7 août 2019 23:39:12 #}
    {{ '2019-08-07 23:39:12'|format_datetime(locale='fr') }}

.. note::

    ``format_datetime`` 过滤器是默认情况下未安装的 ``IntlExtension`` 的一部分。首先安装它：

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
* ``timeFormat``: 时间格式
* ``pattern``: 日期时间模式
