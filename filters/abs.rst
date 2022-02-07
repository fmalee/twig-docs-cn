``abs``
=======

``abs`` 过滤器返回值得绝对值。

.. code-block:: twig

    {# number = -5 #}

    {{ number|abs }}

    {# 输出： 5 #}

.. note::

    在内部，Twig使用PHP的 `abs`_ 函数。

.. _`abs`: https://www.php.net/abs
