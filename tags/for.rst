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
