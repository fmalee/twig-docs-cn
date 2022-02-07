``format_number``
=================

.. versionadded:: 2.12

    Twig 2.12中添加了 ``format_number`` 过滤器。

``format_number`` 过滤器用于格式化数字：

.. code-block:: twig

    {{ '12.345'|format_number }}

可以传递属性来调整输出：

.. code-block:: twig

    {# 12.34 #}
    {{ '12.345'|format_number({rounding_mode: 'floor'}) }}

    {# 1000000.0000 #}
    {{ '1000000'|format_number({fraction_digit: 4}) }}

支持的选项列表：

* ``grouping_used``;
* ``decimal_always_shown``;
* ``max_integer_digit``;
* ``min_integer_digit``;
* ``integer_digit``;
* ``max_fraction_digit``;
* ``min_fraction_digit``;
* ``fraction_digit``;
* ``multiplier``;
* ``grouping_size``;
* ``rounding_mode``;
* ``rounding_increment``;
* ``format_width``;
* ``padding_position``;
* ``secondary_grouping_size``;
* ``significant_digits_used``;
* ``min_significant_digits_used``;
* ``max_significant_digits_used``;
* ``lenient_parse``.

除了原生数字，过滤器还可以将数字格式化为各种样式：

.. code-block:: twig

    {# 1,234% #}
    {{ '12.345'|format_number(style='percent') }}

    {# twelve point three four five #}
    {{ '12.345'|format_number(style='spellout') }}

    {# 12 sec. #}
    {{ '12'|format_duration_number }}

支持的样式列表：

* ``decimal``;
* ``currency``;
* ``percent``;
* ``scientific``;
* ``spellout``;
* ``ordinal``;
* ``duration``.

作为一种快捷方式，您可以通过将 `*` 替换为样式来使用 ``format_*_number`` 过滤器：

.. code-block:: twig

    {# 1,234% #}
    {{ '12.345'|format_percent_number }}

    {# twelve point three four five #}
    {{ '12.345'|format_spellout_number }}

可以传递属性来调整输出：

.. code-block:: twig

    {# 12.3% #}
    {{ '0.12345'|format_percent_number({rounding_mode: 'floor', fraction_digit: 1}) }}

默认情况下，过滤器使用当前的区域设置。您可以显式地传递它：

.. code-block:: twig

    {# 12,345 #}
    {{ '12.345'|format_number(locale='fr') }}

.. note::

    ``format_number`` 过滤器是默认情况下未安装的 ``IntlExtension`` 的一部分。首先安装它：

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
* ``attrs``: 属性映射
* ``style``: 数字输出的样式
