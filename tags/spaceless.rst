``spaceless``
=============

.. tip::

    从 Twig 2.7 开始, 使用 :doc:`spaceless <../filters/spaceless>` 过滤器代替。

使用 ``spaceless`` 标签可以删除 *HTML标签之间* 的空白，但不是HTML标签中或纯文本中的空白：

.. code-block:: html+twig

    {% spaceless %}
        <div>
            <strong>foo</strong>
        </div>
    {% endspaceless %}

    {# 输出会是：<div><strong>foo</strong></div> #}

此标签并非旨在“优化”生成的HTML内容的大小，而仅仅是为了避免HTML标签之间的额外空格，以避免浏览器在某些情况下渲染怪异(quirk)。

.. tip::

    如果要优化生成的HTML内容的大小，请使用gzip来压缩输出。

.. tip::

    如果你想创建一个实际删除HTML字符串中所有额外空白的标签，请注意这并不像它看起来那么容易（想一下
    ``textarea`` 或 ``pre`` 示例）。使用像Tidy这样的第三方库可能是个更好的主意。

.. tip::

    有关空白控制的更多信息，请阅读文档的
    :ref:`专门章节 <templates-whitespace-control>`，并了解如何在标签上使用空格来控制修饰符。
