``round``
=========

``round`` 过滤器将一个数字舍入到给定精度：

.. code-block:: jinja

    {{ 42.55|round }}
    {# outputs 43 #}

    {{ 42.55|round(1, 'floor') }}
    {# outputs 42.5 #}

``round`` 过滤器有两个可选参数; 第一个指定精度（默认为 ``0``），第二个指定舍入方法（默认为 ``common``）：

* ``common`` rounds either up or down (rounds the value up to precision decimal places away from zero, when it is half way there -- making 1.5 into 2 and -1.5 into -2);
  四舍五入（将值舍入到远离零的精确小数位，当小数点离零有一半时 -- 将1.5变为2，将-1.5变为-2;

* ``ceil`` 总是上舍入

* ``floor`` 总是下舍入

.. note::

    ``//`` 操作符相当于 ``|round(0, 'floor')``。

参数
---------

* ``precision``: 舍入精度
* ``method``: 舍入方法
