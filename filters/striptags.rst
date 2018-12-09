``striptags``
=============

``striptags`` 过滤器剥离SGML/XML标签和用一个空白替换相邻的空格：

.. code-block:: jinja

    {{ some_html|striptags }}

你还可以提供不应剥离的标签：

.. code-block:: jinja

    {{ some_html|striptags('<br><p>') }}

在这个例子中，``<br/>``、``<br>``、``<p>`` 以及 ``</p>`` 标签将不会从字符串中删除。

.. note::

    在内部，Twig使用PHP的 `strip_tags`_ 函数。

参数
---------

* ``allowable_tags``: 不应剥离的标签

.. _`strip_tags`: https://secure.php.net/strip_tags
