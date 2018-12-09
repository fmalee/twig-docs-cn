``max``
=======

``max`` 返回一个序列或一组值中的最大值：

.. code-block:: jinja

    {{ max(1, 3, 2) }}
    {{ max([1, 3, 2]) }}

当调用一个映射时，``max`` 将忽略键并仅比较值：

.. code-block:: jinja

    {{ max({2: "e", 1: "a", 3: "b", 5: "d", 4: "c"}) }}
    {# returns "e" #}
