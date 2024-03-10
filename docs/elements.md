---
title: Markdown 元素
hidden: true
order: 2
---

# Markdown 元素

这是为了测试所有可能存在的不同类型的 markdown 而创建的。每当我发现存在样式边缘情况时，我都会将其添加到此文档中。这是我对需要在不同上下文中进行样式化的各种元素的视觉回归形式。

## 标题

4、5 和 6 大小的标题都被同等对待。如果我们开始编写需要这些标题的散文，我们应该重新评估我们的生活。

# 一级标题

## 二级标题

### 三级标题

#### 四级标题

##### 五级标题

###### 六级标题

## 表格

| 语法   | 描述       |
| ------ | ---------- |
| 第一行 | 第二列     |
| 第二行 | 第二列     |
| 第三行 | 第二列     |

## Callouts

Callouts 可以与 `<docs-*>` 元素一起使用。它们专门用于引起文档正常流程之外的信息的特别关注。

这些元素有三种支持的变体：

1. `<docs-info>` - 用于一般信息的 callout。
2. `<docs-warning>` - 用于警告读者应该知道的事情。
3. `<docs-error>` - 用于告诉用户他们不应该做某事。

示例：

<docs-info>`<Link to>` 在当前 URL 以 `/` 结尾时与普通的 `<a href>` 行为不同。`<Link to>` 忽略尾部斜杠，并对每个 `..` 删除一个 URL 段。但当当前 URL 以 `/` 结尾时，`<a href>` 值对 `..` 的处理与其不以 `/` 结尾时不同。</docs-info>

<docs-warning>`useMatches` 仅适用于数据路由器，如 [`createBrowserRouter`][createbrowserrouter]，因为它们在开始时就知道完整的路由树，并且可以提供所有当前匹配项。此外，`useMatches` 不会匹配任何后代路由树，因为路由器不知道后代路由。</docs-warning>

<docs-error>不要这样做</docs-error>

<docs-info>这个标记有点丑陋，因为（目前）这些都必须在没有任何换行符的情况下位于 `<docs-*>` 元素内 _但_ 在这些内部可能有一个图像。 <img src="https://picsum.photos/480/270" width="480" height="270" /></docs-info>

注意：也许这些的语义并不完全正确。在文档的情况下可能有其他名词是有意义的，比如：

- `<docs-info>` 可以变成 `<docs-tip>`
- `<docs-warning>` 可以变成 `<docs-important>`
- `<docs-error>` 可以变成 `<docs-warning>` 或 `<docs-danger>`

## 块引用

这是一个带有多行和样式的 `<blockquote>`：

> 这是我的引用。
>
> 它可以有 [链接]($link)，**粗体文本**，_斜体文本_，甚至 `<code>`，所有这些都应该被考虑到。哦，别忘了列表：
>
> - 列表项 1
> - 列表项 2
> - 列表项 3
>
> 无序的，或有序的：
>
> 1. 列表项
> 2. 另一个列表项
> 3. 还有一个列表项

## 列表

这是一个链接列表，其中一些是代码：

- 这是我的第一个列表项
- [这是我的第二个列表项，是一个链接][$link]
- 这是我的第三个项，带有 `<code>` 和 [`<LinkedCode>` 与文本混合][$link]

别忘了对于没有 `href` 的 `<a>` 标签也要进行适当的样式设置：<a>就像这个链接</a>。

然后是 `<dl>` 列表：

<dl>
  <dt>React</dt>
  <dd>对某事作出反应或以某种方式行事</dd>
  <dt>Router</dt>
  <dd>将数据包转发到计算机网络的适当部分的设备。</dd>
  <dt>Library</dt>
  <dd>一个包含书籍、期刊，有时还有电影和录制音乐集合的建筑物或房间，供人们阅读、借阅或参考。</dd>
  <dd>一组通常可用的程序和软件包，通常加载并存储在磁盘上以供立即使用。</dd>
</dl>

## 代码

普通代码：

```tsx
<WhateverRouter initialEntries={["/events/123"]}>
  <Route path="/" element={<Root />} loader={rootLoader}>
    <Route
      path="events/:id"
      element={<Event />}
      loader={eventLoader}
    />
  </Route>
</WhateverRouter>
```

带有多个高亮行：

```tsx lines=[1-2,5]
<WhateverRouter initialEntries={["/events/123"]}>
  <Route path="/" element={<Root />} loader={rootLoader}>
    <Route
      path="events/:id"
      element={<Event />}
      loader={eventLoader}
    />
  </Route>
</WhateverRouter>
```

带有文件名：

```tsx filename=src/main.jsx
<WhateverRouter initialEntries={["/events/123"]}>
  <Route path="/" element={<Root />} loader={rootLoader}>
    <Route
      path="events/:id"
      element={<Event />}
      loader={eventLoader}
    />
  </Route>
</WhateverRouter>
```

错误代码：

```tsx bad
<WhateverRouter initialEntries={["/events/123"]}>
  <Route path="/" element={<Root />} loader={rootLoader}>
    <Route
      path="events/:id"
      element={<Event />}
      loader={eventLoader}
    />
  </Route>
</WhateverRouter>
```

带有高亮行和文件名的错误代码：

```tsx filename=src/main.jsx bad lines=[2-5]
<WhateverRouter initialEntries={["/events/123"]}>
  <Routes>
    <Route path="/" element={<Root />} loader={rootLoader}>
      <Route
        path="events/:id"
        element={<Event />}
        loader={eventLoader}
      />
    </Route>
  </Routes>
</WhateverRouter>
```

溢出的行：

```html
<!-- 应用程序的其他 HTML 代码放在这里 -->
<!-- prettier-ignore -->
<script src="https://unpkg.com/react@>=16.8/umd/react.development.js" crossorigin></script>
```

---

[$link]: https://www.youtube.com/watch?v=dQw4w9WgXcQ
[createbrowserrouter]: ./routers/create-browser-router
```