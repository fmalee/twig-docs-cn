Twig内部
==============

Twig非常易于扩展，你可以轻松破解(hack)它。
请记住，你应该尝试在破解核心之前创建扩展，因为大多数功能和增强功能都可以通过扩展来处理。
本章对于想要了解Twig如何工作的人也很有用。

Twig是如何工作的？
-------------------

一个Twig模板的渲染可归纳为四个关键步骤：

* **加载** 模板：如果模板已经编译，请加载它并转到 *评估* 步骤，否则：

  * 首先，**词法分析器** 将模板源代码标记为小块以便于处理;
  * 然后，**解析器** 将令牌流转换为有意义的节点树（抽象语法树）;
  * 最终，**编译器** 将AST转换为PHP代码。

* **评估** 该模板：它基本上意味着调用已编译模板的 ``display()`` 方法并将其传递给上下文。

词法分析器
----------

词法分析器将一个模板源代码标记为令牌流（每个令牌是一个 ``\Twig\Token`` 实例，并且流是一个
``\Twig\TokenStream`` 实例）。默认词法分析器能识别13种不同的令牌类型：

* ``\Twig\Token::BLOCK_START_TYPE``、``\Twig\Token::BLOCK_END_TYPE``: 区块的分隔符（``{% %}``）;
* ``\Twig\Token::VAR_START_TYPE``、``\Twig\Token::VAR_END_TYPE``: 变量的分隔符（``{{ }}``）;
* ``\Twig\Token::TEXT_TYPE``: 一个表达式之外的文本;
* ``\Twig\Token::NAME_TYPE``: 一个表达式中的名称;
* ``\Twig\Token::NUMBER_TYPE``: 一个表达式中的数字;
* ``\Twig\Token::STRING_TYPE``: 一个表达式中的字符串;
* ``\Twig\Token::OPERATOR_TYPE``: 一个操作符;
* ``\Twig\Token::PUNCTUATION_TYPE``: 一个标点符号;
* ``\Twig\Token::INTERPOLATION_START_TYPE``、``\Twig\Token::INTERPOLATION_END_TYPE``: 用于字符串插值的分隔符;
* ``\Twig\Token::EOF_TYPE``: 模板结尾。

你可以通过调用一个环境的 ``tokenize()`` 方法来手动将源代码转换为令牌流::

    $stream = $twig->tokenize(new \Twig\Source($source, $identifier));

由于流有一个 ``__toString()`` 方法，你可以通过echoing对象来对其进行文本表示::

    echo $stream."\n";

以下是 ``Hello {{ name }}`` 模板的输出：

.. code-block:: text

    TEXT_TYPE(Hello )
    VAR_START_TYPE()
    NAME_TYPE(name)
    VAR_END_TYPE()
    EOF_TYPE()

.. note::

    可以通过调用 ``setLexer()`` 方法来更改默认的词法分析器（``\Twig\Lexer``）::

        $twig->setLexer($lexer);

解析器
----------

解析器将令牌流转换为一个AST（抽象语法树）或节点树（``\Twig\Node\Module`` 的一个实例）。
核心扩展定义像 ``for``、``if`` 等等的基本节点以及表达式节点。

你可以通过调用一个环境的 ``parse()`` 方法将令牌流手动转换为节点树::

    $nodes = $twig->parse($stream);

Echoing节点对象可以很好地表示树::

    echo $nodes."\n";

以下是 ``Hello {{ name }}`` 模板的输出：

.. code-block:: text

    \Twig\Node\Module(
      \Twig\Node\Text(Hello )
      \Twig\Node\Print(
        \Twig\Node\Expression\Name(name)
      )
    )

.. note::

    可以通过调用 ``setParser()`` 方法来更改默认的解析器（``\Twig\TokenParser\AbstractTokenParser``）::

        $twig->setParser($parser);

编译器
------------

最后一步由编译器完成。它将一个节点树作为输入，并生成可用于模板运行时执行的PHP代码。

你可以使用一个环境的 ``compile()`` 方法手动将节点树编译为PHP代码::

    $php = $twig->compile($nodes);

一个 ``Hello {{ name }}`` 模板生成的模模板如下所示（实际输出可能因你使用的Twig版本而异）::

    /* Hello {{ name }} */
    class __TwigTemplate_1121b6f109fe93ebe8c6e22e3712bceb extends Template
    {
        protected function doDisplay(array $context, array $blocks = [])
        {
            // line 1
            echo "Hello ";
            echo twig_escape_filter($this->env, (isset($context["name"]) ? $context["name"] : null), "html", null, true);
        }

        // some more code
    }

.. note::

    可以通过调用 ``setCompiler()`` 方法来更改默认的编译器（``\Twig\Compiler``）::

        $twig->setCompiler($compiler);
