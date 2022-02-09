``spaceless``
=============

使用 ``spaceless`` 过滤器可以删除 *HTML标签之间* 的空白，但不是HTML标签中或纯文本中的空白：

.. code-block:: html+twig

    {{
        "<div>
            <strong>foo</strong>
        </div>
        "|spaceless }}

    {# 输出将是：<div><strong>foo</strong></div> #}

对于大批量HTML的转换，您可以将 ``spaceless`` 与 ``apply`` 标签组合使用：

.. code-block:: html+twig

    {% apply spaceless %}
        <div>
            <strong>foo</strong>
        </div>
    {% endapply %}

    {# 输出将是：<div><strong>foo</strong></div> #}

此标签并非旨在“优化”生成的HTML内容的大小，而仅仅是为了避免HTML标签之间的额外空白，
以避免浏览器在某些情况下渲染怪异(quirk)。

.. caution::

    由于过滤器在幕后使用正则表达式，因此其性能直接与正在处理的文本大小有关
    （请记住，过滤器是在运行时执行的）。

.. tip::

    如果要优化生成的HTML内容的大小，请使用gzip来压缩输出。

.. tip::

    如果你想创建一个实际删除HTML字符串中所有额外空白的标签，请注意这并不像它看起来那么容易（想一下
    ``textarea`` 或 ``pre`` 示例）。使用像Tidy这样的第三方库可能是个更好的主意。

.. tip::

    有关空白控制的更多信息，请阅读文档的
    :ref:`专门章节 <templates-whitespace-control>`，并了解如何在标签上使用空格来控制修饰符。
