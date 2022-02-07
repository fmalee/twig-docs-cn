``convert_encoding``
====================

``convert_encoding`` 过滤器将一个字符串从一个编码转换到另一个编码。
第一个参数是预期的输出字符集，第二个参数是输入字符集：

.. code-block:: twig

    {{ data|convert_encoding('UTF-8', 'iso-2022-jp') }}

.. note::

    此过滤器依赖 `iconv`_ 扩展名。

参数
---------

* ``to``:   输出字符集
* ``from``: 输入字符集

.. _`iconv`: https://www.php.net/iconv
