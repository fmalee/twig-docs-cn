``title``
=========

``title`` 过滤器返回一个值的titlecased版本。单词将以大写字母开头，所有剩余字符均为小写：

.. code-block:: jinja

    {{ 'my first car'|title }}

    {# outputs 'My First Car' #}
