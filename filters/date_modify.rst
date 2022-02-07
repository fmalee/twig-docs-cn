``date_modify``
===============

``date_modify`` 过滤器使用一个修改字符串来修改一个日期：

.. code-block:: twig

    {{ post.published_at|date_modify("+1 day")|date("m/d/Y") }}

``date_modify`` 过滤器接受字符串（它必须是 `strtotime`_ 函数所支持的格式）或 `DateTime`_ 实例。
你可以将其与 :doc:`date<date>` 过滤器组合以进行格式化。

参数
---------

* ``modifier``: 修饰符

.. _`strtotime`: https://www.php.net/strtotime
.. _`DateTime`:  https://www.php.net/DateTime
