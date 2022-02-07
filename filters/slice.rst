``slice``
===========

``slice`` 过滤器提取一个序列、映射或字符串的一个切片：

.. code-block:: twig

    {% for i in [1, 2, 3, 4, 5]|slice(1, 2) %}
        {# 将会迭代 2 和 3 #}
    {% endfor %}

    {{ '12345'|slice(1, 2) }}

    {# 输出： 23 #}

你可以对起始和长度使用任何有效表达式：

.. code-block:: twig

    {% for i in [1, 2, 3, 4, 5]|slice(start, length) %}
        {# ... #}
    {% endfor %}

作为语法糖，你也可以使用 ``[]`` 符号：

.. code-block:: twig

    {% for i in [1, 2, 3, 4, 5][start:length] %}
        {# ... #}
    {% endfor %}

    {{ '12345'[1:2] }} {# 显示： "23" #}

    {# 你可以省略第一个参数 - 它与0相同 #}
    {{ '12345'[:2] }} {# 输出： "12" #}

    {# 你可以省略最后一个参数 - 它会选择一直到最后 #}
    {{ '12345'[2:] }} {# 输出： "345" #}

``slice`` 滤器操作数组时使用PHP的 `array_slice`_ 函数，操作字符串时使用PHP的 `substr`_。

如果起始是非负数，则序列将从变量的起始处开始。如果起始为负数，则序列将从变量的末尾开始。

如果长度已给定并且是正数，则序列将包含足够的变量元素。如果变量短于给定长度，则仅保存可用的变量元素。
如果长度已给定并且为负数，则序列将从变量末尾的足够元素处停止。
如果省略，那么序列将包含从偏移开始至变量末尾的所有元素。

.. note::

    它还适用于实现了 `Traversable`_ 接口的对象。

参数
---------

* ``start``:         切片的起始
* ``length``:        切片的长度
* ``preserve_keys``: 是否保留键（当输入是数组时）

.. _`Traversable`: https://www.php.net/manual/en/class.traversable.php
.. _`array_slice`: https://www.php.net/array_slice
.. _`mb_substr`:   https://www.php.net/mb-substr
.. _`substr`:      https://www.php.net/substr
