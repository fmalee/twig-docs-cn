``json_encode``
===============

The ``json_encode`` filter returns the JSON representation of a value:

.. code-block:: jinja

    {{ data|json_encode() }}

.. note::

    Internally, Twig uses the PHP `json_encode`_ function.

Arguments
---------

* ``options``: A bitmask of `json_encode options`_ (``{{
  data|json_encode(constant('JSON_PRETTY_PRINT')) }}``.
  Combine constants using `Twig's bitwise operators`: ``{{
  data|json_encode(constant('JSON_PRETTY_PRINT') b-or constant('JSON_HEX_QUOT') }}``)

.. _`json_encode`: https://secure.php.net/json_encode
.. _`json_encode options`: https://secure.php.net/manual/en/json.constants.php
.. _`Twig's bitwise operators`: https://twig.symfony.com/doc/2.x/templates.html#logic