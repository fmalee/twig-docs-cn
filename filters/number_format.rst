``number_format``
=================

``number_format`` 过滤器用来格式化数字。它是PHP的 `number_format`_ 函数的封装：

.. code-block:: jinja

    {{ 200.35|number_format }}

你可以使用额外参数来控制小数位数、小数点和千位分隔符：

.. code-block:: jinja

    {{ 9800.333|number_format(2, '.', ',') }}

要格式化负数，请用括号括起数字（因为Twig的 :ref:`运算符优先级<twig-expressions>` 需要）：

.. code-block:: jinja

    {{ -9800.333|number_format(2, '.', ',') }} {# 输出 : -9 #}
    {{ (-9800.333)|number_format(2, '.', ',') }} {# 输出 : -9,800.33 #}

如果未提供格式化选项，则Twig将使用以下默认格式选项：

* 0 位小数。
* ``.`` 作为小数点。
* ``,`` 作为千位分隔符。

可以通过核心扩展轻松更改这些默认值：

.. code-block:: php

    $twig = new Twig_Environment($loader);
    $twig->getExtension('Twig_Extension_Core')->setNumberFormat(3, '.', ',');

在每次调用 ``number_format`` 时可以使用额外参数来重写为其默认值设置。

参数
---------

* ``decimal``:       要显示的小数点数
* ``decimal_point``: 用于表示小数点的字符
* ``thousand_sep``:  用于表示千位分隔符的字符

.. _`number_format`: https://secure.php.net/number_format
