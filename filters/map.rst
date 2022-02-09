``map``
=======

``map`` 过滤器将箭头函数应用于序列或映射的元素。箭头函数接收序列或映射的值：

.. code-block:: twig

    {% set people = [
        {first: "Bob", last: "Smith"},
        {first: "Alice", last: "Dupond"},
    ] %}

    {{ people|map(p => "#{p.first} #{p.last}")|join(', ') }}
    {# 输出： Bob Smith, Alice Dupond #}

箭头函数还接收键作为第二个参数：

.. code-block:: twig

    {% set people = {
        "Bob": "Smith",
        "Alice": "Dupond",
    } %}

    {{ people|map((value, key) => "#{key} #{value}")|join(', ') }}
    {# 输出： Bob Smith, Alice Dupond #}

请注意，箭头函数可以访问当前上下文。

参数
---------

* ``arrow``: 箭头函数
