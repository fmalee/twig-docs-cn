``iterable``
============

``iterable`` 检查变量是数组还是可遍历对象：

.. code-block:: jinja

    {# 如果foo变量是可迭代的，则求值为true #}
    {% if users is iterable %}
        {% for user in users %}
            Hello {{ user }}!
        {% endfor %}
    {% else %}
        {# users变量可能是一个字符串 #}
        Hello {{ users }}!
    {% endif %}
