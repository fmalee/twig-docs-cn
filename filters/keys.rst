``keys``
========

``keys`` 过滤器返回一个数组的键。当你想迭代数组的键时，它很有用：

.. code-block:: twig

    {% for key in array|keys %}
        ...
    {% endfor %}
