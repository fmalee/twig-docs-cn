``attribute``
=============

``attribute`` 函数可用于访问变量的一个“动态”属性：

.. code-block:: twig

    {{ attribute(object, method) }}
    {{ attribute(object, method, arguments) }}
    {{ attribute(array, item) }}

此外，可以使用 ``defined`` 测试来检查是否存在一个动态属性：

.. code-block:: twig

    {{ attribute(object, method) is defined ? '方法存在' : '方法不存在' }}

.. note::

    该函数的解析算法与用 ``.`` 表示法的算法相同，只是该项可以是任何有效的表达式。
