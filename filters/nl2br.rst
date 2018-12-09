``nl2br``
=========

``nl2br`` 过滤器在一个字符串中的所有换行符之前插入HTML换行：

.. code-block:: jinja

    {{ "I like Twig.\nYou will like it too."|nl2br }}
    {# 输出：

        I like Twig.<br />
        You will like it too.

    #}

.. note::

    在应用转换之前，``nl2br`` 过滤器会预先转义输入。
