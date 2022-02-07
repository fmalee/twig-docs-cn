``same as``
===========

``same as`` 检查变量是否与另一个变量相同。这相当于PHP的 ``===``：

.. code-block:: twig

    {% if foo.attribute is same as(false) %}
        the foo attribute really is the 'false' PHP value
    {% endif %}
