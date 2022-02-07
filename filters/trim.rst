``trim``
========

``trim`` 过滤器用于剥离字符串的开头和末尾的空格（或其它字符）：

.. code-block:: twig

    {{ '  I like Twig.  '|trim }}

    {# 输出：'I like Twig.' #}

    {{ '  I like Twig.'|trim('.') }}

    {# 输出：'  I like Twig' #}

    {{ '  I like Twig.  '|trim(side='left') }}

    {# 输出：'I like Twig.  ' #}

    {{ '  I like Twig.  '|trim(' ', 'right') }}

    {# 输出：'  I like Twig.' #}

.. note::

    在内部，Twig使用PHP的 `trim`_、`ltrim`_ 以及 `rtrim`_ 函数。

参数
---------

* ``character_mask``: 要剥离的字符

* ``side``: 默认是从左侧和右侧（`两侧`）剥离，但 `左侧` 和 `右侧` 仅从左侧或右侧剥离

.. _`trim`: https://www.php.net/trim
.. _`ltrim`: https://www.php.net/ltrim
.. _`rtrim`: https://www.php.net/rtrim
