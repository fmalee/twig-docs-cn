# Twig 3.X中文文档

根据Google机译和自己的理解翻译的[Twig 3.X的文档](https://github.com/twigphp/Twig/tree/3.x/doc)。

限于有限的水平：

- 英语一般，仅能尽可能将原语句翻译明确，但中文下的语句可能不够通顺。
- 非程序猿、设计狮，翻译的术语可能有些变味，已经尽量避免，并整理出术语对译。
- Twig水平有限，有些不确定的句子没有翻译。
- 时间有限，仅翻译当下本人会用上的章节，其余回头再译（一般情况下是回不了头的）。
- 每次修改、调整本文档时，都会顺手同步官方的文档。

## 同步记录

- `3.X`
  - `237f1789a7db3a0cbb8e28a75832a45b7f9fc304`（2022-02-04）

## 未译章节

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

## 构建文档

原本临时使用了Doctrine ORM文档的主题，但未修改主题本身。因为官方在**2020/11/5**的`c9328ad613a171dd3c084ad9ab8aadcb853009ea` 提交上才引入了：DOCtor-RST config + Sphinx build + GithubActions。

### Docker

```shell
# 进入文档目录
$ cd twig-docs-cn
# 构建Docker镜像
# 因为有不少依赖，所以不能使用Sphinx官方的镜像
$ docker build -t twig:sphinx .
# 运行新生成的Docker镜像，并生成HTML文档
$ docker run --rm -v $(pwd):/docs twig:sphinx make html
```

#### Dockerfile修改

因为官方的Sphinx环境有一个依赖需要访问Github，所以在 `Dockerfile` 中加了代理环境配置。

如果**不需要代理**：可以将 `Dockerfile` 中的以 `ENV ` 开头的行删除，去掉代理。

如果**需要代理**：修改 `ENV MY_PROXY_URL=` 中的地址为自己的地址。现在的`docker.for.mac.host.internal` 是macOS下Docker访问本地的地址。

### 本机环境

```shell
# 进入文档目录
$ cd twig-docs-cn
# 安装相关依赖
$ python3 -m pip install --no-cache-dir -r .requirements.txt
# 生成HTML文档
$ make html
```

> 额外需要安装Git，同时保证能连同Github，有一个依赖需要克隆Github项目。

生成的文档在 **`twig-docs-cn/__build/html`** 目录

## 资源

- [官方Github](https://github.com/twigphp/Twig/tree/2.x/doc)
- [翻译参考](https://www.kancloud.cn/yunye/twig-cn/159454)
