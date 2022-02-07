``parent``
==========

当一个模板使用继承时，可以使用 ``parent`` 函数在重写区块时渲染父区块的内容：

.. code-block:: html+twig

    {% extends "base.html" %}

    {% block sidebar %}
        <h3>Table Of Contents</h3>
        ...
        {{ parent() }}
    {% endblock %}

该 ``parent()`` 调用将返回 ``base.html`` 模板中定义的 ``sidebar`` 区块的内容。

.. seealso::

    :doc:`extends<../tags/extends>`, :doc:`block<../functions/block>`, :doc:`block<../tags/block>`
