``verbatim``
============

``verbatim`` 标签用于标记不应该被解析的原始文本。
例如，要将Twig语法作为示例放入一个模板，你可以使用此代码片段：

.. code-block:: html+twig

    {% verbatim %}
        <ul>
        {% for item in seq %}
            <li>{{ item }}</li>
        {% endfor %}
        </ul>
    {% endverbatim %}
