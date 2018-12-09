``import``
==========

Twig支持将常用代码放入 :doc:`宏<../tags/macro>` 中。这些宏可以进入不同的模板并从导入到那里。

导入模板有两种方法。你可以将完整模板导入到一个变量或从中请求特定的宏。

想象一下，我们有一个用于渲染表单（名为 ``forms.html``）的辅助模块：

.. code-block:: jinja

    {% macro input(name, value, type, size) %}
        <input type="{{ type|default('text') }}" name="{{ name }}" value="{{ value|e }}" size="{{ size|default(20) }}" />
    {% endmacro %}

    {% macro textarea(name, value, rows, cols) %}
        <textarea name="{{ name }}" rows="{{ rows|default(10) }}" cols="{{ cols|default(40) }}">{{ value|e }}</textarea>
    {% endmacro %}

最简单、最灵活的是将整个模块导入到一个变量。这样你就可以访问这些属性：

.. code-block:: jinja

    {% import 'forms.html' as forms %}

    <dl>
        <dt>Username</dt>
        <dd>{{ forms.input('username') }}</dd>
        <dt>Password</dt>
        <dd>{{ forms.input('password', null, 'password') }}</dd>
    </dl>
    <p>{{ forms.textarea('comment') }}</p>

或者，你可以将模板中的宏名称导入到当前名称空间：

.. code-block:: jinja

    {% from 'forms.html' import input as input_field, textarea %}

    <dl>
        <dt>Username</dt>
        <dd>{{ input_field('username') }}</dd>
        <dt>Password</dt>
        <dd>{{ input_field('password', '', 'password') }}</dd>
    </dl>
    <p>{{ textarea('comment') }}</p>

.. tip::

    要从当前文件导入宏，请使用特殊的 ``_self`` 变量作为来源。

.. seealso:: :doc:`macro<../tags/macro>`, :doc:`from<../tags/from>`
