``merge``
=========

``merge`` 过滤器可以合并两个数组：

.. code-block:: twig

    {% set values = [1, 2] %}

    {% set values = values|merge(['apple', 'orange']) %}

    {# values 现在包含： [1, 2, 'apple', 'orange'] #}

现在新值将会添加到现有值得末尾。

``merge`` 过滤器也适用于一个散列：

.. code-block:: twig

    {% set items = { 'apple': 'fruit', 'orange': 'fruit', 'peugeot': 'unknown' } %}

    {% set items = items|merge({ 'peugeot': 'car', 'renault': 'car' }) %}

    {# items 现在包含： { 'apple': 'fruit', 'orange': 'fruit', 'peugeot': 'car', 'renault': 'car' } #}

对于散列，合并过程发生在键上：如果对应的键尚不存在，则添加它，但如果该键已存在，则重写其值。

.. tip::

    如果要确保保留住在数组中定义某些值（通过给定的默认值），请反转调用中的两个元素：

    .. code-block:: twig

        {% set items = { 'apple': 'fruit', 'orange': 'fruit' } %}

        {% set items = { 'apple': 'unknown' }|merge(items) %}

        {# items now contains { 'apple': 'fruit', 'orange': 'fruit' } #}

.. note::

    在内部，Twig使用PHP的 `array_merge`_ 函数。它通过将Traversable转换为数组来支持它们。

.. _`array_merge`: https://www.php.net/array_merge
