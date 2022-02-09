``cache``
=========

.. versionadded:: 3.2

    Twig 3.2中添加了 ``cache`` 标签。

``cache`` 标签告诉 Twig 缓存一个模板片段：

.. code-block:: twig

    {% cache "cache key" %}
        永久缓存（取决于缓存实现）
    {% endcache %}

如果您想在一定时间后使缓存过期，请通过 ``ttl()`` 修饰符指定以秒为单位的过期时间：

.. code-block:: twig

    {% cache "cache key" ttl(300) %}
        缓存300秒
    {% endcache %}

缓存键可以是任何不使用保留字符（``{}()/\@:``）的字符串；最佳实践是在键中嵌入一些有用的信息，允许缓存在必须刷新时自动过期：

* 给每个缓存一个唯一的名称和命名空间，就像你的模板一样；

* 嵌入一个在模板代码更改时递增的整数（以自动使所有当前缓存无效）；

* 嵌入一个唯一键，只要模板代码中使用的变量发生更改，该键就会更新。

例如，我会使用 ``{% cache "blog_post;v1;" ~ post.id ~ ";" ~
post.updated_at %}`` 缓存博客内容的模板片段，其中 ``blog_post`` 描述模板片段，``v1``
代表模板代码的第一个版本，``post.id`` 代表博文的ID，``post.updated_at``
返回一个时间戳，表示博文最后一次修改的时间。

使用这种命名缓存键的策略可以避免使用 ``ttl``。这就像我们对 HTTP 缓存所做的那样，使用“验证”策略而不是“过期”策略。

如果你的缓存实现支持标签，你还可以标记你的缓存项：

.. code-block:: twig

    {% cache "cache key" tags('blog') %}
        一些代码
    {% endcache %}

    {% cache "cache key" tags(['cms', 'blog']) %}
        一些代码
    {% endcache %}

``cache`` 标签为变量创建了一个新的“作用域”，这意味着更改对于模板片段是局部的：

.. code-block:: twig

    {% set count = 1 %}

    {% cache "cache key" tags('blog') %}
        {# 不会影响缓存标签之外的计数值 #}
        {% set count = 2 %}
        一些代码
    {% endcache %}

    {# 显示：1 #}
    {{ count }}

.. note::

    ``cache`` 标签是默认情况下未安装的 ``CacheExtension`` 的一部分。首先安装它：

    .. code-block:: bash

        $ composer require twig/cache-extra

    在 Symfony 项目中，你可以通过安装 ``twig/extra-bundle`` 来自动启用它：

    .. code-block:: bash

        $ composer require twig/extra-bundle

    或者，在Twig环境上显式添加该扩展::

        use Twig\Extra\Cache\CacheExtension;

        $twig = new \Twig\Environment(...);
        $twig->addExtension(new CacheExtension());

    如果你不使用 Symfony，你还必须注册扩展运行时::

        use Symfony\Component\Cache\Adapter\FilesystemAdapter;
        use Symfony\Component\Cache\Adapter\TagAwareAdapter;
        use Twig\Extra\Cache\CacheRuntime;
        use Twig\RuntimeLoader\RuntimeLoaderInterface;

        $twig->addRuntimeLoader(new class implements RuntimeLoaderInterface {
            public function load($class) {
                if (CacheRuntime::class === $class) {
                    return new CacheRuntime(new TagAwareAdapter(new FilesystemAdapter()));
                }
            }
        });
