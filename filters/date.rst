``date``
========

``date`` 过滤器格式化一个日期为给定的格式：

.. code-block:: twig

    {{ post.published_at|date("m/d/Y") }}

格式说明符与 `date`_ 支持的格式说明符相同，除非筛选数据的类型为
`DateInterval`_，那么格式则必须符合 `DateInterval::format`_。

``date`` 过滤器接受字符串（它必须是在 `strtotime`_ 函数所支持的格式）、`DateTime`_
实例，或 `DateInterval`_ 实例。例如，要显示当前日期，请过滤 “now” 一词：

.. code-block:: twig

    {{ "now"|date("m/d/Y") }}

要在日期格式中转义单词和字符，请在每个字符前面使用 ``\\`` ：

.. code-block:: twig

    {{ post.published_at|date("F jS \\a\\t g:ia") }}

如果传递给 ``date`` 过滤器的值是 ``null``，则默认返回当前日期。
如果需要空字符串而不是当前日期，请使用一个三元运算符：

.. code-block:: twig

    {{ post.published_at is empty ? "" : post.published_at|date("m/d/Y") }}

如果没有提供格式，Twig将使用默认格式： ``F j, Y H:i`` 。通过调用 ``core`` 扩展实例上的
``setDateFormat()`` 方法可以更改此默认值。
第一个参数是日期的默认格式，第二个参数是日期间隔的默认格式::

    $twig = new \Twig\Environment($loader);
    $twig->getExtension(\Twig\Extension\CoreExtension::class)->setDateFormat('d/m/Y', '%d days');

时区
--------

默认情况下，通过应用默认时区（在php.ini中指定的或在Twig中声明的时区 - 见下文）来显示日期，但你可以通过显式指定时区来重写它：

.. code-block:: twig

    {{ post.published_at|date("m/d/Y", "Europe/Paris") }}

如果日期已经是一个 `DateTime`_ 对象，并且如果要保留其当前时区，则传递 ``false`` 为时区值：

.. code-block:: twig

    {{ post.published_at|date("m/d/Y", false) }}

也可以通过调用 ``setTimezone()`` 来全局设置默认时区::

    $twig = new \Twig\Environment($loader);
    $twig->getExtension(\Twig\Extension\CoreExtension::class)->setTimezone('Europe/Paris');

参数
---------

* ``format``:   日期格式
* ``timezone``: 日期时区

.. _`strtotime`:            https://www.php.net/strtotime
.. _`DateTime`:             https://www.php.net/DateTime
.. _`DateInterval`:         https://www.php.net/DateInterval
.. _`date`:                 https://www.php.net/date
.. _`DateInterval::format`: https://www.php.net/DateInterval.format
