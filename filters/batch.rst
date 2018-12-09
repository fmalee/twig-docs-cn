``batch``
=========

The ``batch`` filter "batches" items by returning a list of lists with the given number of items.
可以使用第二个参数来填充缺失项：

.. code-block:: jinja

    {% set items = ['a', 'b', 'c', 'd', 'e', 'f', 'g'] %}

    <table>
    {% for row in items|batch(3, 'No item') %}
        <tr>
            {% for column in row %}
                <td>{{ column }}</td>
            {% endfor %}
        </tr>
    {% endfor %}
    </table>

以上示例将渲染为：

.. code-block:: jinja

    <table>
        <tr>
            <td>a</td>
            <td>b</td>
            <td>c</td>
        </tr>
        <tr>
            <td>d</td>
            <td>e</td>
            <td>f</td>
        </tr>
        <tr>
            <td>g</td>
            <td>No item</td>
            <td>No item</td>
        </tr>
    </table>

参数
---------

* ``size``: 批量的大小; 小数将被四舍五入
* ``fill``: 用于填充缺失的项
