# Twig文档

根据Google机译和自己的理解翻译的Twig 2的文档。

限于有限的水平：

- 英语一般，仅能尽可能将原语句翻译明确，但中文下的语句可能不够通顺。
- 非程序猿、设计狮，翻译的术语可能有些变味，已经尽量避免，并整理出术语对译。
- Twig水平有限，有些不确定的句子没有翻译。
- 时间有限，仅翻译当下本人会用上的章节，其余回头再译（一般情况下是回不了头的）。
- 每次修改、调整本文档时，都会顺手同步官方的文档。

## 同步记录

- `2.X`
  - `1e04274f34e9f80894865a1478e72b363e19179b`（2022-02-04）
  - `c8020aa5d848fcb934b047d85304544b174a95a7`（2018-10-12）

## 未译章节

- 修改了 `index.rst`，以适应新主题
- `advanced.rst`
- `recipesrst`
- `tags/embed.rst`
- `tags/extends.rst`


## 术语约定

- `Application`：应用
- `Literal`：字面值
- `Loader`：加载器
- `Escaper`：转义器
- `Profiler`：分析器
- `Optimizer`：优化器
- `Subscript`：下标
- `Statement`：语句、声明
- `Item`：单元
- `Filter`：过滤器
- `Tag`：标签
- `Named argument`：具名实参
- `Block`：区块
- `Context`：上下文、语境
- `Skeleton`：骨架
- `Override`：重写、复写
- `Ealuate`：评估
- `Scope`：作用域
- `Horizontal reuse`：横向重用
- `Escape`：转义
- `Auto-escaping`：自动转义
- `Hash`：散列、哈希
- `String interpolation`：字符串插值
- `Modifier`：修改器
- `Environment`：环境
- `Deprecate`：弃用、不推荐使用
- `Lexer`：词法分析器
- `Policy`：策略

## 生成主题

临时使用了Doctrine ORM文档的主题，但未修改主题本身。

### 如何生成

官方在2020/11/5的`c9328ad613a171dd3c084ad9ab8aadcb853009ea` 提交上才引入了：DOCtor-RST config + Sphinx build + GithubActions



> 官方没有提供生成说明，需要自己配置。

## 资源

- [官方Github](https://github.com/twigphp/Twig/tree/2.x/doc)
- [翻译参考](https://www.kancloud.cn/yunye/twig-cn/159454)
