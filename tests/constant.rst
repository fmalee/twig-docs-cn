``constant``
============

``constant`` 检查一个变量是否与一个常量具有完全相同的值。你可以使用全局常量或类常量：

.. code-block:: twig

    {% if post.status is constant('Post::PUBLISHED') %}
        the status attribute is exactly the same as Post::PUBLISHED
    {% endif %}

你也可以测试对象实例中的常量：

.. code-block:: twig

    {% if post.status is constant('PUBLISHED', post) %}
        the status attribute is exactly the same as Post::PUBLISHED
    {% endif %}
