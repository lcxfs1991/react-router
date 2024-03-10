---
标题：Await
new: true
---

## `<Await>`

用于渲染带有自动错误处理功能的[deferred][defer]值。请务必查看[Deferred Data Guide][deferred guide]，因为有几个API与该组件协同工作。

```jsx lines=[1,10-18]
import { Await, useLoaderData } from "react-router-dom";

function Book() {
  const { book, reviews } = useLoaderData();
  return (
    <div>
      <h1>{book.title}</h1>
      <p>{book.description}</p>
      <React.Suspense fallback={<ReviewsSkeleton />}>
        <Await
          resolve={reviews}
          errorElement={
            <div>无法加载评论 😬</div>
          }
          children={(resolvedReviews) => (
            <Reviews items={resolvedReviews} />
          )}
        />
      </React.Suspense>
    </div>
  );
}
```

**注意：** `<Await>`期望在`<React.Suspense>`或`<React.SuspenseList>`父元素中进行渲染，以启用备选UI。

## 类型声明

```tsx
declare function Await(
  props: AwaitProps
): React.ReactElement;

interface AwaitProps {
  children: React.ReactNode | AwaitResolveRenderFunction;
  errorElement?: React.ReactNode;
  resolve: TrackedPromise | any;
}

interface AwaitResolveRenderFunction {
  (data: Awaited<any>): React.ReactElement;
}
```

## `children`

可以是React元素或函数。

当使用函数时，值将作为唯一参数提供。

```tsx [2]
<Await resolve={reviewsPromise}>
  {(resolvedReviews) => <Reviews items={resolvedReviews} />}
</Await>
```

当使用React元素时，将通过[`useAsyncValue`][useasyncvalue]提供数据：

```tsx [2]
<Await resolve={reviewsPromise}>
  <Reviews />
</Await>;

function Reviews() {
  const resolvedReviews = useAsyncValue();
  return <div>{/* ... */}</div>;
}
```

## `errorElement`

当promise被拒绝时，错误元素会代替children进行渲染。你可以通过[`useAsyncError`][useasyncerror]访问错误信息。

如果promise被拒绝，你可以提供一个可选的`errorElement`，通过`useAsyncError`钩子在上下文中显示错误信息。

```tsx [3,9]
<Await
  resolve={reviewsPromise}
  errorElement={<ReviewsError />}
>
  <Reviews />
</Await>;

function ReviewsError() {
  const error = useAsyncError();
  return <div>{error.message}</div>;
}
```

如果不提供`errorElement`，拒绝的值将会冒泡到最近的路由级[`errorElement`][routeerrorelement]，并可通过[`useRouteError`][userouteerror]钩子访问。

## `resolve`

接收从[deferred][defer] [loader][loader]返回的承诺，以便解决并渲染。

```jsx [12,15,24,32-33]
import {
  defer,
  Route,
  useLoaderData,
  Await,
} from "react-router-dom";

// 假设有这样一个路由
<Route
  loader={async () => {
    let book = await getBook();
    let reviews = getReviews(); // 不等待其完成
    return defer({
      book,
      reviews, // 这是一个promise
    });
  }}
  element={<Book />}
/>;

function Book() {
  const {
    book,
    reviews, // 这是同一个promise
  } = useLoaderData();
  return (
    <div>
      <h1>{book.title}</h1>
      <p>{book.description}</p>
      <React.Suspense fallback={<ReviewsSkeleton />}>
        <Await
          // 并将其传递给Await
          resolve={reviews}
        >
          <Reviews />
        </Await>
      </React.Suspense>
    </div>
  );
}
```

[useloaderdata]: ../hooks/use-loader-data
[userouteerror]: ../hooks/use-route-error
[defer]: ../utils/defer
[deferred guide]: ../guides/deferred
[useasyncvalue]: ../hooks/use-async-value
[useasyncerror]: ../hooks/use-async-error
[routeerrorelement]: ../route/error-element
[loader]: ../route/loader