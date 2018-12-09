``json_encode``
===============

``json_encode`` 过滤器返回一个值的JSON表示：

.. code-block:: jinja

    {{ data|json_encode() }}

.. note::

    在内部，Twig使用PHP的 `json_encode`_ 函数。

参数
---------

* ``options``: 一个 `json_encode选项`_
  的位掩码：（``{{data|json_encode(constant('JSON_PRETTY_PRINT')) }}``。
  使用 `Twig's bitwise operators`_ 来组合常量：``{{
  data|json_encode(constant('JSON_PRETTY_PRINT') b-or constant('JSON_HEX_QUOT') }}``）

.. _`json_encode`: https://secure.php.net/json_encode
.. _`json_encode选项`: https://secure.php.net/manual/en/json.constants.php
.. _`Twig's bitwise operators`: https://twig.symfony.com/doc/2.x/templates.html#logic
