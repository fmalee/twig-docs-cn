``for``
=======

循环遍历一个序列中的每个项。例如，要显示一个名为 ``users`` 的变量提供的用户列表：

.. code-block:: html+twig

    <h1>Members</h1>
    <ul>
        {% for user in users %}
            <li>{{ user.username|e }}</li>
        {% endfor %}
    </ul>

.. note::

    一个序列可以是一个数组或一个实现了 ``Traversable`` 接口的对象。

如果你确实需要迭代一系列数字，则可以使用 ``..`` 运算符：

.. code-block:: twig

    {% for i in 0..10 %}
        * {{ i }}
    {% endfor %}

上面的代码片段将打印0到10之间的所有数字。

它也适用于字母：

.. code-block:: twig

    {% for letter in 'a'..'z' %}
        * {{ letter }}
    {% endfor %}

``..`` 运算符的两边都可以采取任何表达：

.. code-block:: twig

    {% for letter in 'a'|upper..'z'|upper %}
        * {{ letter }}
    {% endfor %}

.. tip:

    如果需要一个从1开始的不同步进，则可以使用 ``range`` 函数。

`loop` 变量
-------------------

在 ``for`` 循环块内部，你可以访问一些特殊变量：

===================== =============================================================
变量                   说明
===================== =============================================================
``loop.index``        循环的当前迭代。（1索引）
``loop.index0``       循环的当前迭代。（0索引）
``loop.revindex``     循环结束时的迭代次数（1个索引）
``loop.revindex0``    循环结束时的迭代次数（0索引）
``loop.first``        如果第一次迭代，则为True
``loop.last``         如果是最后一次迭代，则为True
``loop.length``       序列中的项的数量
``loop.parent``       父上下文
===================== =============================================================

.. code-block:: twig

    {% for user in users %}
        {{ loop.index }} - {{ user.username }}
    {% endfor %}

.. note::

    ``loop.length``、``loop.revindex``、``loop.revindex0`` 以及 ``loop.last``
    变量仅适用于PHP数组，或实现了 ``Countable`` 接口的对象。
    在使用一个条件进行循环时它们也不可用。

添加条件
------------------

.. tip::

    从Twig 2.10开始，使用 :doc:`filter <../filters/filter>` 过滤器代替，
    或者在 ``for`` 主体中使用一个 ``if`` 条件
    （如果您的条件依赖于循环中更新的变量，并且您没有使用 ``loop`` 变量）。

与PHP不同，它不可能在循环中 ``break`` 或 ``continue``。
但是，你可以在迭代期间过滤该序列，以便跳过指定项。以下示例将跳过所有未激活的用户：

.. code-block:: html+twig

    <ul>
        {% for user in users if user.active %}
            <li>{{ user.username|e }}</li>
        {% endfor %}
    </ul>

优点是指定的循环变量将正确计数，因此不计算未迭代的用户。
请记住，使用循环条件时不会定义类似 ``loop.last`` 的属性。

.. note::

    建议不要在条件中使用 ``loop`` 变量，因为它可能不会按你的预期执行。
    例如，类似 ``loop.index > 4`` 的一个条件并不会生效，因为该索引只在该条件为真时递增（因此该条件永远不会匹配）。

`else` 条件
-----------------

如果由于序列为空而没有发生迭代，则可以使用 ``else`` 方法来渲染一个替换区块：

.. code-block:: html+twig

    <ul>
        {% for user in users %}
            <li>{{ user.username|e }}</li>
        {% else %}
            <li><em>no user found</em></li>
        {% endfor %}
    </ul>

迭代键
-------------------

默认情况下，一个循环会遍历序列的值。但你可以使用 ``keys`` 过滤器来迭代键：

.. code-block:: html+twig

    <h1>Members</h1>
    <ul>
        {% for key in users|keys %}
            <li>{{ key }}</li>
        {% endfor %}
    </ul>

迭代键和值
------------------------------

你还可以同时访问键和值：

.. code-block:: html+twig

    <h1>Members</h1>
    <ul>
        {% for key, user in users %}
            <li>{{ key }}: {{ user.username|e }}</li>
        {% endfor %}
    </ul>

迭代子集
-----------------------

你可能希望迭代值的一个子集。这可以通过使用 :doc:`slice <../filters/slice>` 过滤器来实现：

.. code-block:: html+twig

    <h1>Top Ten Members</h1>
    <ul>
        {% for user in users|slice(0, 10) %}
            <li>{{ user.username|e }}</li>
        {% endfor %}
    </ul>
