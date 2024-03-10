---
title: Tutorial
order: 3
hidden: true
---

# 教程

## 引言

[在此查看已完成的应用版本][stackblitz-app]。

React Router 是一个功能完备的客户端和服务器端路由库，适用于 React（一个用于构建用户界面的 JavaScript 库）。React Router 在任何支持 React 的环境中都能运行，包括 Web、使用 node.js 的服务器端以及 React Native。

如果您刚开始接触 React，我们建议您遵循官方文档中[出色的入门指南][reactjs-getting-started]。该文档中有大量信息可帮助您快速上手。React Router 与 React >= 16.8 兼容。

本教程将保持简洁明了。通过本教程，您将了解日常使用 React Router 需要打交道的 API。之后，您可以深入阅读其他文档以获得更深入的理解。

在构建一个小型记账应用的过程中，我们将涵盖以下内容：

- 配置路由
- 使用 Link 进行导航
- 创建具有活动样式效果的链接
- 使用嵌套路由实现布局
- 程序化导航
- 利用 URL 参数进行数据加载
- 使用 URL 搜索参数
- 通过组合创建自定义行为
- 服务器端渲染

## 安装

### 推荐：StackBlitz

为了完成本教程，您需要一个可用的 React 应用。我们建议跳过打包器并使用 [StackBlitz 上的演示项目][stackblitz-template] 在浏览器中编码：

[[https://developer.stackblitz.com/img/open_in_stackblitz.svg](https://developer.stackblitz.com/img/open_in_stackblitz.svg)][stackblitz-template]

随着您编辑文件，教程会实时更新。

### 使用打包器

您可以自由选择喜欢的打包器，如 [Create React App][cra] 或 [Vite][vite]。

```sh
# 使用 Create React App
npx create-react-app router-tutorial

# 使用 Vite
npm init vite@latest router-tutorial --template react
```

然后安装 React Router 的依赖项：

```sh
cd router-tutorial
npm install react-router-dom@6
```

接下来，将 App.js 编辑得相当简单：

```tsx filename=src/App.js
export default function App() {
  return (
    <div>
      <h1>记账软件！</h1>
    </div>
  );
}
```

实际上，那个“！”一点也不单调。这非常令人兴奋。我们在全球疫情后调整业务方向期间对 React Router v6 测试版进行了超过一年的打磨。这是我们最近做的最激动人心的事情！

最后，请确保 `index.js` 或 `main.jsx`（取决于您使用的打包器）确实很简单：

```tsx filename=src/main.jsx
import * as ReactDOM from "react-dom/client";
import App from "./App";

const root = ReactDOM.createRoot(
  document.getElementById("root")
);
root.render(<App />);
```

最后，启动您的应用：

```sh
# 可能是这个命令
npm start

# 或者这个命令
npm run dev
```

## 连接 URL

首先要做的是将您的应用连接到浏览器的 URL：导入 `BrowserRouter` 并将其包裹在您的整个应用外部。

```tsx lines=[2,9-11] filename=src/main.jsx
import * as ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import App from "./App";

const root = ReactDOM.createRoot(
  document.getElementById("root")
);
root.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

应用本身不会发生变化，但此时我们已准备好开始操作 URL。

## 添加一些链接

打开 `src/App.js`，导入 `Link` 并添加全局导航。注：在这个教程中，样式仅供参考，我们仅使用内联样式以便于演示，您可以按照自己的喜好为应用设置样式。

```tsx lines=[1,7-15] filename=src/App.js
import { Link } from "react-router-dom";

export default function App() {
  return (
    <div>
      <h1>记账软件</h1>
      <nav
        style={{
          borderBottom: "solid 1px",
          paddingBottom: "1rem",
        }}
      >
        <Link to="/invoices">发票</Link> |{" "}
        <Link to="/expenses">支出</Link>
      </nav>
    </div>
  );
}
```

现在点击这些链接以及前进/后退按钮（如果您使用 StackBlitz，请点击内嵌浏览器工具栏中的“在新窗口中打开”按钮）。React Router 现在正在控制 URL！

虽然我们还没有设置当 URL 改变时渲染的路由，但是 Link 已经能够在不触发整页刷新的情况下更改 URL。

## 添加一些路由

新增两个文件：

- `src/routes/invoices.jsx`
- `src/routes/expenses.jsx`

（文件的位置无关紧要，但当您决定为此应用添加自动后台 API、服务器端渲染、代码分割打包器等功能时，像这样命名文件可以使您更容易地将此应用移植到我们的另一个项目 [Remix][remix] 😜）

现在填充它们的内容：

```tsx filename=src/routes/expenses.jsx
export default function Expenses() {
  return (
    <main style={{ padding: "1rem 0" }}>
      <h2>支出</h2>
    </main>
  );
}
```

```tsx filename=src/routes/invoices.jsx
export default function Invoices() {
  return (
    <main style={{ padding: "1rem 0" }}>
      <h2>发票</h2>
    </main>
  );
}
```

最后，让我们教会 React Router 如何根据不同的 URL 渲染我们的应用，方法是在 `main.jsx` 或 `index.js` 中创建第一个“路由配置”。

```tsx lines=[2,4-5,8-9,15-21] filename=src/main.jsx
import * as ReactDOM from "react-dom/client";
import {
  BrowserRouter,
  Routes,
  Route,
} from "react-router-dom";
import App from "./App";
import Expenses from "./routes/expenses";
import Invoices from "./routes/invoices";

const root = ReactDOM.createRoot(
  document.getElementById("root")
);
root.render(
  <BrowserRouter>
    <Routes>
      <Route path="/" element={<App />} />
      <Route path="expenses" element={<Expenses />} />
      <Route path="invoices" element={<Invoices />} />
    </Routes>
  </BrowserRouter>
);
```

请注意，在 `"/"` 路径下它渲染 `<App>`，而在 `"/invoices"` 路径下则渲染 `<Invoices>`。干得好！

<docs-info>如果您使用 StackBlitz，请记得点击内嵌浏览器工具栏中的“在新窗口中打开”按钮，这样才能在浏览器中使用前进/后退按钮。</docs-info>

## 嵌套路由

您可能已经注意到，在点击链接时，`App` 中的布局消失了。重复共享布局是一件很麻烦的事。我们了解到大多数 UI 是一系列嵌套的布局，几乎总是映射到 URL 的部分，因此这个想法已经被内置到了 React Router 中。

只需做两件事，我们就能轻松处理自动、持久的布局：

1. 将路由嵌套在 App 路由内部
2. 渲染 Outlet 组件

首先，我们将费用和发票路由嵌套到 App 路由内。目前，这两个路由是 App 的同级，我们希望将它们变成 App 路由的 _子_ 路由：

```jsx lines=[17-20] filename=src/main.jsx
import * as ReactDOM from "react-dom/client";
import {
  BrowserRouter,
  Routes,
  Route,
} from "react-router-dom";
import App from "./App";
import Expenses from "./routes/expenses";
import Invoices from "./routes/invoices";

const root = ReactDOM.createRoot(
  document.getElementById("root")
);
root.render(
  <BrowserRouter>
    <Routes>
      <Route path="/" element={<App />}>
        <Route path="expenses" element={<Expenses />} />
        <Route path="invoices" element={<Invoices />} />
      </Route>
    </Routes>
  </BrowserRouter>
);
```

当路由具有子路由时，会发生两件事：

1. 它们会嵌套 URL（`"/" + "expenses"` 和 `"/" + "invoices"`）
2. 当子路由匹配时，它将在父级路由组件中嵌套 UI 组件以共享布局：

然而，在 (2) 生效之前，我们需要在 `App.jsx` 的“父级”路由中渲染一个 `Outlet` 组件。

```jsx lines=[1,16] filename=src/App.jsx
import { Outlet, Link } from "react-router-dom";

export default function App() {
  return (
    <div>
      <h1>记账软件</h1>
      <nav
        style={{
          borderBottom: "solid 1px",
          paddingBottom: "1rem",
        }}
      >
        <Link to="/invoices">发票</Link> |{" "}
        <Link to="/expenses">支出</Link>
      </nav>
      <Outlet />
    </div>
  );
}
```

现在再次点击各个链接。父级路由（`App.js`）将会持续存在，而 `<Outlet>` 会在两个子路由（`<Invoices>` 和 `<Expenses>`）之间切换！

正如我们稍后将看到的，这种机制在路由层次结构的任何级别上都能工作，并且极其强大。

## 列出发票

通常情况下，您会从某个服务器获取数据，但在本教程中，我们为了专注于路由功能，先硬编码一些模拟数据。

在 `src/data.js` 创建一个文件，并复制粘贴以下内容：

```js filename=src/data.js
let invoices = [
  {
    name: "Santa Monica",
    number: 1995,
    amount: "$10,800",
    due: "12/05/1995",
  },
  {
    name: "Stankonia",
    number: 2000,
    amount: "$8,000",
    due: "10/31/2000",
  },
  {
    name: "Ocean Avenue",
    number: 2003,
    amount: "$9,500",
    due: "07/22/2003",
  },
  {
    name: "Tubthumper",
    number: 1997,
    amount: "$14,000",
    due: "09/01/1997",
  },
  {
    name: "Wide Open Spaces",
    number: 1998,
    amount: "$4,600",
    due: "01/27/1998",
  },
];

export function getInvoices() {
  return invoices;
}
```

现在我们可以在发票路由中使用这个数据。同时，我们为发票页面添加一点样式，以实现侧边栏导航布局。您可以直接复制粘贴所有代码，但特别注意 `<Link>` 元素的 `to` 属性：

```js lines=[17] filename=src/routes/invoices.jsx
import { Link } from "react-router-dom";
import { getInvoices } from "../data";

export default function Invoices() {
  let invoices = getInvoices();
  return (
    <div style={{ display: "flex" }}>
      <nav
        style={{
          borderRight: "solid 1px",
          padding: "1rem",
        }}
      >
        {invoices.map((invoice) => (
          <Link
            style={{ display: "block", margin: "1rem 0" }}
            to={`/invoices/${invoice.number}`}
            key={invoice.number}
          >
            {invoice.name}
          </Link>
        ))}
      </nav>
      {/* 在这里渲染具体的发票详情 */}
    </div>
  );
}
```

太棒了！现在点击一个发票链接并查看会发生什么。

## 获取帮助

恭喜！您已完成本教程。我们希望它能帮助您掌握 React Router 的基本使用方法。

如果您遇到问题，请查看[资源](/resources)页面以获取帮助。祝您好运！

[stackblitz-app]: https://stackblitz.com/edit/github-agqlf5?file=src/App.jsx
[stackblitz-template]: https://stackblitz.com/github/remix-run/react-router/tree/main/tutorial?file=src/App.jsx
[reactjs-getting-started]: https://reactjs.org/docs/getting-started.html
[cra]: https://create-react-app.dev/
[vite]: https://vitejs.dev/guide/#scaffolding-your-first-vite-project
[remix]: https://remix.run
