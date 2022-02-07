``date``
========

将一个参数转换为日期以允许进行日期比较：

.. code-block:: html+twig

    {% if date(user.created_at) < date('-2days') %}
        {# 执行一些操作 #}
    {% endif %}

该参数必须采用PHP支持的 `日期和时间格式`_ 中的一个。

你可以将时区作为第二个参数传递：

.. code-block:: html+twig

    {% if date(user.created_at) < date('-2days', 'Europe/Paris') %}
        {# 执行一些操作 #}
    {% endif %}

如果没有传递参数，则该函数返回当前日期：

.. code-block:: html+twig

    {% if date(user.created_at) < date() %}
        {# 还是如此！ #}
    {% endif %}

.. note::

    你可以通过调用 ``core`` 扩展实例上的 ``setTimezone()`` 来全局的设置默认时区::

        $twig = new \Twig\Environment($loader);
        $twig->getExtension('\Twig\Extension\CoreExtension::class')->setTimezone('Europe/Paris');

参数
---------

* ``date``:     日期
* ``timezone``: 时区

.. _`日期和时间格式`: https://www.php.net/manual/en/datetime.formats.php
