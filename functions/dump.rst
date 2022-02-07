``dump``
========

``dump`` 函数转储有关模板变量的信息。这对于通过内省其变量来调试不符合预期的模板非常有用：

.. code-block:: twig

    {{ dump(user) }}

.. note::

    默认情况下，``dump`` 功能不可用。
    你必须在创建Twig环境时显式添加 ``\Twig\ExtensionDebug`` 扩展::

        $twig = new \Twig\Environment($loader, [
            'debug' => true,
            // ...
        ]);
        $twig->addExtension(new \Twig\ExtensionDebug());

    即使启用了，如果未启用环境中的 ``debug``
    选项（以避免在生产服务器上泄漏调试信息），``dump`` 函数也不会显示任何内容。

可以在一个HTML上下文中使用一个 ``pre`` 标签来封装输出以使其更易于阅读：

.. code-block:: html+twig

    <pre>
        {{ dump(user) }}
    </pre>

.. tip::

    已启用 `XDebug`_ 并且 ``html_errors`` 为  ``on`` 时，不需要使用 ``pre``
    标签; 而且在启用XDebug的情况下输出也更友好。

你可以通过将变量作为附加参数传递来调试多个变量：

.. code-block:: twig

    {{ dump(user, categories) }}

如果未传递任何值，则转储当前上下文中的所有变量：

.. code-block:: twig

    {{ dump() }}

.. note::

    在内部，Twig使用PHP的 `var_dump`_ 函数。

参数
---------

* ``context``: 要转储的上下文

.. _`XDebug`:   https://xdebug.org/docs/display
.. _`var_dump`: https://www.php.net/var_dump
