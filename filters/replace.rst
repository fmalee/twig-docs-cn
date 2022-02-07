``replace``
===========

``replace`` 过滤器通过替换占位符（占位符是自由形式）来格式化给定的字符串：

.. code-block:: twig

    {{ "I like %this% and %that%."|replace({'%this%': foo, '%that%': "bar"}) }}

    {# 如果foo参数等于foo字符串，则输出： I like foo and bar #}

    {# 使用％作为分隔符纯粹是传统的和可选的 #}

    {{ "I like this and --that--."|replace({'this': foo, '--that--': "bar"}) }}

    {# 输出： I like foo and bar #}

参数
---------

* ``from``: 占位符值

.. seealso::

    :doc:`format<format>`
