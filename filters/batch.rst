``batch``
=========

``batch`` 过滤器通过返回具有给定项目数的列表来“批处理”项目。
可以使用第二个参数来填充缺失项：

.. code-block:: html+twig

    {% set items = ['a', 'b', 'c', 'd'] %}

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

.. code-block:: html+twig

    <table>
        <tr>
            <td>a</td>
            <td>b</td>
            <td>c</td>
        </tr>
        <tr>
            <td>d</td>
            <td>No item</td>
            <td>No item</td>
        </tr>
    </table>

参数
---------

* ``size``: 批量的大小; 小数将被四舍五入
* ``fill``: 用于填充缺失的项
* ``preserve_keys``: 是否保留键
