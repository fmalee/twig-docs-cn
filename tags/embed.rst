``embed``
=========

``embed`` 标签是 :doc:`include<include>` 与 :doc:`extends<extends>`
行为的组合。它允许你引入另一个模板的内容，就像 ``include`` 做的一样。
但它还允许你重写引入的模板中定义的任何区块，例如继承一个模板时。
The ``embed`` tag combines the behaviour of :doc:`include<include>` and
:doc:`extends<extends>`.
It allows you to include another template's contents, just like ``include``
does. But it also allows you to override any block defined inside the
included template, like when extending a template.

可以将一个嵌入式模板视为一个“微型布局骨架”。
Think of an embedded template as a "micro layout skeleton".

.. code-block:: jinja

    {% embed "teasers_skeleton.twig" %}
        {# These blocks are defined in "teasers_skeleton.twig" #}
        {# and we override them right here:                    #}
        {% block left_teaser %}
            Some content for the left teaser box
        {% endblock %}
        {% block right_teaser %}
            Some content for the right teaser box
        {% endblock %}
    {% endembed %}

``embed`` 标签带有模板继承的思想内容片段的水平。
虽然模板继承允许“文档骨架”（由子模板填充生命），但embed标记允许你为较小的内容单元创建“骨架”并重复使用并将其填充到你喜欢的任何位置。
The ``embed`` tag takes the idea of template inheritance to the level of
content fragments. While template inheritance allows for "document skeletons",
which are filled with life by child templates, the ``embed`` tag allows you to
create "skeletons" for smaller units of content and re-use and fill them
anywhere you like.

由于用例可能不明显，让我们看一个简化的例子。想象一下由多个HTML页面共享的基本模板，定义一个名为“content”的块：
Since the use case may not be obvious, let's look at a simplified example.
Imagine a base template shared by multiple HTML pages, defining a single block
named "content":

.. code-block:: text

    ┌─── page layout ─────────────────────┐
    │                                     │
    │           ┌── block "content" ──┐   │
    │           │                     │   │
    │           │                     │   │
    │           │ (child template to  │   │
    │           │  put content here)  │   │
    │           │                     │   │
    │           │                     │   │
    │           └─────────────────────┘   │
    │                                     │
    └─────────────────────────────────────┘

某些页面（“foo”和“bar”）共享相同的内容结构 - 两个垂直堆叠的框：
Some pages ("foo" and "bar") share the same content structure -
two vertically stacked boxes:

.. code-block:: text

    ┌─── page layout ─────────────────────┐
    │                                     │
    │           ┌── block "content" ──┐   │
    │           │ ┌─ block "top" ───┐ │   │
    │           │ │                 │ │   │
    │           │ └─────────────────┘ │   │
    │           │ ┌─ block "bottom" ┐ │   │
    │           │ │                 │ │   │
    │           │ └─────────────────┘ │   │
    │           └─────────────────────┘   │
    │                                     │
    └─────────────────────────────────────┘

而其他页面（“繁荣”和“巴兹”）共享不同的内容结构 - 两个并排的框：
While other pages ("boom" and "baz") share a different content structure -
two boxes side by side:

.. code-block:: text

    ┌─── page layout ─────────────────────┐
    │                                     │
    │           ┌── block "content" ──┐   │
    │           │                     │   │
    │           │ ┌ block ┐ ┌ block ┐ │   │
    │           │ │"left" │ │"right"│ │   │
    │           │ │       │ │       │ │   │
    │           │ │       │ │       │ │   │
    │           │ └───────┘ └───────┘ │   │
    │           └─────────────────────┘   │
    │                                     │
    └─────────────────────────────────────┘

没有embed标记，你有两种方法来设计模板：
Without the ``embed`` tag, you have two ways to design your templates:

* Create two "intermediate" base templates that extend the master layout
  template: one with vertically stacked boxes to be used by the "foo" and
  "bar" pages and another one with side-by-side boxes for the "boom" and
  "baz" pages.
  创建两个扩展主布局模板的“中间”基础模板：一个具有垂直堆叠的框，用于“foo”和“bar”页面，另一个用于“boom”和“baz”的并排框“页面。

* Embed the markup for the top/bottom and left/right boxes into each page
  template directly.
  将顶部/底部和左/右框的标记直接嵌入到每个页面模板中。

这两种解决方案不能很好地扩展，因为它们各自都有一个主要缺点：
These two solutions do not scale well because they each have a major drawback:

* The first solution may indeed work for this simplified example. But imagine
  we add a sidebar, which may again contain different, recurring structures
  of content. Now we would need to create intermediate base templates for
  all occurring combinations of content structure and sidebar structure...
  and so on.
  第一种解决方案可能确实适用于这个简化的例子。但想象一下，我们添加了一个侧边栏，它可能会再次包含不同的，反复出现的内容结构。现在我们需要为所有出现的内容结构和侧边栏结构组合创建中间基础模板......等等。

* The second solution involves duplication of common code with all its negative
  consequences: any change involves finding and editing all affected copies
  of the structure, correctness has to be verified for each copy, copies may
  go out of sync by careless modifications etc.
  第二种解决方案涉及复制公共代码及其所有负面后果：任何更改都涉及查找和编辑所有受影响的结构副本，必须验证每个副本的正确性，副本可能会因粗心修改等而不同步。

在这种情况下，embed标签就派上用场了。公共布局代码可以存在于单个基本模板中，而两个不同的内容结构，我们将它们称为“微布局”，进入必要时嵌入的单独模板：
In such a situation, the ``embed`` tag comes in handy. The common layout
code can live in a single base template, and the two different content structures,
let's call them "micro layouts" go into separate templates which are embedded
as necessary:

页面模板foo.twig：
Page template ``foo.twig``:

.. code-block:: jinja

    {% extends "layout_skeleton.twig" %}

    {% block content %}
        {% embed "vertical_boxes_skeleton.twig" %}
            {% block top %}
                Some content for the top box
            {% endblock %}

            {% block bottom %}
                Some content for the bottom box
            {% endblock %}
        {% endembed %}
    {% endblock %}

这是代码vertical_boxes_skeleton.twig：
And here is the code for ``vertical_boxes_skeleton.twig``:

.. code-block:: html+jinja

    <div class="top_box">
        {% block top %}
            Top box default content
        {% endblock %}
    </div>

    <div class="bottom_box">
        {% block bottom %}
            Bottom box default content
        {% endblock %}
    </div>

vertical_boxes_skeleton.twig模板的目标是分解框的HTML标记。
The goal of the ``vertical_boxes_skeleton.twig`` template being to factor
out the HTML markup for the boxes.

该embed标签带有精确相同的参数include标签：
The ``embed`` tag takes the exact same arguments as the ``include`` tag:

.. code-block:: jinja

    {% embed "base" with {'foo': 'bar'} %}
        ...
    {% endembed %}

    {% embed "base" with {'foo': 'bar'} only %}
        ...
    {% endembed %}

    {% embed "base" ignore missing %}
        ...
    {% endembed %}

.. warning::

    As embedded templates do not have "names", auto-escaping strategies based
    on the template name won't work as expected if you change the context (for
    instance, if you embed a CSS/JavaScript template into an HTML one). In that
    case, explicitly set the default auto-escaping strategy with the
    ``autoescape`` tag.
    由于嵌入式模板没有“名称”，因此如果更改上下文（例如，如果将CSS / JavaScript模板嵌入到HTML模板中），则基于模板名称的自动转义策略将无法按预期工作。在这种情况下，使用autoescape标记显式设置默认的自动转义策略 。

.. seealso:: :doc:`include<../tags/include>`
