``if``
======

Twig中的 ``if`` 语句相当于PHP的if语句。

在最简单的形式中，你可以使用它来测试表达式的计算结果是否为 ``true``：

.. code-block:: jinja

    {% if online == false %}
        <p>Our website is in maintenance mode. Please, come back later.</p>
    {% endif %}

你还可以测试一个数组是否为空：

.. code-block:: jinja

    {% if users %}
        <ul>
            {% for user in users %}
                <li>{{ user.username|e }}</li>
            {% endfor %}
        </ul>
    {% endif %}

.. note::

    如果要测试该变量是否已定义，请改用 ``if users is defined``。

你还可以用 ``not`` 来检查一个值是否等于 ``false``：

.. code-block:: jinja

    {% if not user.subscribed %}
        <p>You are not subscribed to our mailing list.</p>
    {% endif %}

对于多个条件，可以使用 ``and`` 和 ``or``：

.. code-block:: jinja

    {% if temperature > 18 and temperature < 27 %}
        <p>It's a nice day for a walk in the park.</p>
    {% endif %}

对于多个分支，可以像PHP一样使用 ``elseif`` 和 ``else``。你也可以使用更复杂的 ``expressions``：

.. code-block:: jinja

    {% if product.stock > 10 %}
       Available
    {% elseif product.stock > 0 %}
       Only {{ product.stock }} left!
    {% else %}
       Sold-out!
    {% endif %}

.. note::

    确定一个表达式是否为 ``true`` 或 ``false`` 的规则与PHP相同; 这里是边缘情况的规则：

    ====================== ====================
    值                      Boolean evaluation
    ====================== ====================
    空字符串                 false
    数字0                   false
    NAN (非数字)            true
    INF (无穷)              true
    仅有空格的字符串          true
    "0" 或 '0' 字符串        false
    空数组                  false
    null                   false
    非空数组                 true
    对象                    true
    ====================== ====================
