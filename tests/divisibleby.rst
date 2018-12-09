``divisible by``
================

``divisible by`` 检查一个变量是否可被一个数字整除：

.. code-block:: jinja

    {% if loop.index is divisible by(3) %}
        ...
    {% endif %}
