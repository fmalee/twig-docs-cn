``split``
=========

``split`` 过滤器将通过给定的分隔符分割字符串，并返回一个字符串列表：

.. code-block:: twig

    {% set foo = "one,two,three"|split(',') %}
    {# foo包含： ['one', 'two', 'three'] #}

你也可以传递一个 ``limit`` 参数：

* 如果 ``limit`` 为正数，则返回的数组将包含最大限制元素，并且最后一个元素将包含剩余字符串;

* 如果 ``limit`` 为负数，则返回除最后一个 -limit 之外的所有组件;

* 如果 ``limit`` 为零，则将其视为1。

.. code-block:: twig

    {% set foo = "one,two,three,four,five"|split(',', 3) %}
    {# foo 包含： ['one', 'two', 'three,four,five'] #}

如果 ``delimiter`` 是一个空字符串，则值将被相等的块拆分。长度由 ``limit`` 参数设置（默认为一个字符）。

.. code-block:: twig

    {% set foo = "123"|split('') %}
    {# foo 包含： ['1', '2', '3'] #}

    {% set bar = "aabbcc"|split('', 2) %}
    {# bar 包含： ['aa', 'bb', 'cc'] #}

.. note::

    在内部，Twig使用PHP的 `explode`_ 或 `str_split`_ （如果分隔符为空）函数进行字符串拆分。

Arguments
---------

* ``delimiter``: 分隔符
* ``limit``:     限制

.. _`explode`:   https://www.php.net/explode
.. _`str_split`: https://www.php.net/str_split
