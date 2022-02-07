``source``
==========

``source`` 函数返回一个模板的内容而不渲染它：

.. code-block:: twig

    {{ source('template.html') }}
    {{ source(some_var) }}

设置了 ``ignore_missing`` 标志后，如果对应模板不存在，Twig将返回一个空字符串：

.. code-block:: twig

    {{ source('template.html', ignore_missing = true) }}

该函数使用与用于引入(include)模板的模板加载器相同的模板加载器。
因此，如果你使用的是文件系统加载器，则会在其定义的路径中查找该模板。

参数
---------

* ``name``: 要读取的模板的名称
* ``ignore_missing``: 是否忽略丢失的模板
