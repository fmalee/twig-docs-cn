``json_encode``
===============

``json_encode`` 过滤器返回一个值的JSON表示：

.. code-block:: twig

    {{ data|json_encode() }}

.. note::

    在内部，Twig使用PHP的 `json_encode`_ 函数。

参数
---------

* ``options``: 一个 `json_encode选项`_ 的位掩码：
  ``{{ data|json_encode(constant('JSON_PRETTY_PRINT')) }}``。
  使用 :ref:`定义的位运算<template_logic>` 来组合常量：``{{
  ``{{ data|json_encode(constant('JSON_PRETTY_PRINT') b-or constant('JSON_HEX_QUOT')) }}``

.. _`json_encode`: https://www.php.net/json_encode
.. _`json_encode选项`: https://www.php.net/manual/en/json.constants.php
