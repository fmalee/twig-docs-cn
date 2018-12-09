``date``
========

将一个参数转换为日期以允许进行日期比较：

.. code-block:: jinja

    {% if date(user.created_at) < date('-2days') %}
        {# do something #}
    {% endif %}

该参数必须采用PHP支持的 `日期和时间格式`_ 中的一个。

你可以将时区作为第二个参数传递：

.. code-block:: jinja

    {% if date(user.created_at) < date('-2days', 'Europe/Paris') %}
        {# do something #}
    {% endif %}

如果没有传递参数，则该函数返回当前日期：

.. code-block:: jinja

    {% if date(user.created_at) < date() %}
        {# always! #}
    {% endif %}

.. note::

    你可以通过调用 ``core`` 扩展实例上的 ``setTimezone()`` 来全局的设置默认时区：

    .. code-block:: php

        $twig = new Twig_Environment($loader);
        $twig->getExtension('Twig_Extension_Core')->setTimezone('Europe/Paris');

参数
---------

* ``date``:     日期
* ``timezone``: 时区

.. _`日期和时间格式`: https://secure.php.net/manual/en/datetime.formats.php
