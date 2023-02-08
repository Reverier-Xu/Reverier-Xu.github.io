---
title: Marked JS 集成 Katex 数学公式渲染
date: 2023-02-08T19:00:14+08:00
tags: [Web, Development]
categories: [Development]
---

## 前言

在内容网站中支持 Markdown 渲染已经是一个很常见的需求了，相比较 [Vditor](https://github.com/Vanessa219/vditor)、[markdown-it](https://github.com/markdown-it/markdown-it) 等重量级 markdown 编辑器与渲染工具来说，用 [marked](https://marked.js.org) 这类更轻量级的渲染库会带来更好的体验，网站的样式也都可以自己控制。但是 [Marked JS](https://marked.js.org) 仅支持将基本 Markdown 语法渲染成 HTML 标记，对于 代码块高亮、数学公式还是无能为力的。有关代码高亮官方给出了与[highlightJS](https://highlightjs.org/)集成的[方式](https://marked.js.org/using_advanced)，但是有关集成数学公式渲染的我只搜到了几个issue和一些奇怪的实现：

- https://github.com/markedjs/marked/issues/722
- https://github.com/linxiaowu66/marked-kaTex （甚至是直接fork了改的，项目也过期很久了）
- https://gist.github.com/tajpure/47c65cf72c44cb16f3a5df0ebc045f2f （拦截render实现，并提前渲染，会出一些奇怪的问题）
- https://www.xiaog.info/blog/post/marked_js_katex （上面那个的中文版，似乎做了一点改进，但还是很奇怪）

看了后两个现有方案，基本上是用正则表达式给数学公式提取出来，然后塞到 katex 里一顿处理成 html，然后塞回 marked 当成 html 块无脑再渲染一遍。我试了试是能用的，但是行为很奇怪，marked 在处理已经渲染好的 html 块时还会做一些额外的工作，例如转义什么的，最后某些字符总是显示的有问题。

还是看看远处的插件文档，自己写一个插件吧。

## Marked JS 插件实现

我打算集成 Katex 而不是 MathJax。因为网站本身不是为了专业的 Markdown 渲染开发的，支持数学公式只是为了让文章阅读更加方便。MathJax 支持很多高级特性，还支持渲染到不同的格式，似乎功能有些冗余，Katex 足够轻量，看起来完全符合我的需求。

### Marked 工作机制

在写插件之前，要先了解一下 [marked 的工作机制](https://marked.js.org/using_pro)。marked 的渲染流程如下：

- 用户输入 markdown 格式的纯文本内容；
- `lexer` 会把输入的一些片段依次发送给不同的 `tokenizer`，并从这些 `tokenizer` 中生成一系列的 `token`，储存到一个嵌套的树结构中；
- 每个 `tokenizer` 接收到文本片段后，便会进行判断这个片段是否匹配某个标记格式，如果匹配的话，便会生成一个包含相关信息的 `token`，如果没有匹配的片段，就返回一个空值；
- `walkTokens` 函数会遍历所有的 `token`，然后将这些 `token` 送入对应的 `renderer` 中进行渲染，并把渲染的结果拼接成最终的 HTML；

在了解这些之后，应该可以发现，只要实现一个能够提取数学公式块的 `tokenizer` 和一个能够渲染的 `renderer`，并整合进 marked 的工作流程中，就能够实现数学公式的渲染了。

### 相关 API

marked 提供了[相关的 API](https://marked.js.org/using_pro#extensions)，这里就不当翻译官了。

### 实现 `tokenizer`

`tokenizer` 需要两个，一个用来解决 `$f(x)=x+y$` 这样的行内公式，一类用来对付

```markdown
$$
f(x) = \frac{1}{x}
$$
```

这类的行间公式。匹配这些我们只需要两个正则表达式就可以了，一个匹配单个 `$`，一个匹配 `$$`。

### 实现 `render`

直接一把梭 `katex.renderToString(token.text, options)`

## 代码片段

```typescript
import katex, {type KatexOptions} from 'katex'
import 'katex/dist/katex.css'
import type {marked} from 'marked'

export default function (options: KatexOptions = {}): marked.MarkedExtension {
    return {
        extensions: [
            inlineKatex(options),
            blockKatex(options)
        ]
    }
}

function inlineKatex(options: KatexOptions): marked.TokenizerAndRendererExtension {
    return {
        name: 'inlineKatex',
        level: 'inline',
        start(src: string) {
            return src.indexOf('$')
        },
        tokenizer(src: string, _tokens) {
            const match = src.match(/^\$+([^$\n]+?)\$+/)
            if (match) {
                return {
                    type: 'inlineKatex',
                    raw: match[0],
                    text: match[1].trim()
                }
            }
        },
        renderer(token) {
            return katex.renderToString(token.text, options)
        }
    }
}

function blockKatex(options: KatexOptions): marked.TokenizerAndRendererExtension {
    return {
        name: 'blockKatex',
        level: 'block',
        start(src: string) {
            return src.indexOf('$$')
        },
        tokenizer(src: string, _tokens) {
            const match = src.match(/^\$\$+\n([^$]+?)\n\$\$/)
            if (match) {
                return {
                    type: 'blockKatex',
                    raw: match[0],
                    text: match[1].trim()
                }
            }
        },
        renderer(token) {
            options.displayMode = true
            return `<p>${katex.renderToString(token.text, options)}</p>`
        }
    }
}
```

保存到 `katex_extension.ts` 中，使用时只需要导入后 `marked.use(KatexExtension({}))` 即可，参数中接收的是 Katex 的设置项。

如果需要 lazy load，也可以

```typescript
  const katex = await import('@/path/to/katex_extension.ts')
  marked.use(katex.default({strict: false}))
```

我先使用不带任何插件的 marked 将基础内容渲染出来，然后再加载katex与highlightJS重新渲染一遍，在某些网速不佳的环境下能提供更好的用户体验。
