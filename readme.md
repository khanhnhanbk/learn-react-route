# Learning React-route-dom

Tổng kết lại những gì đã học từ [React Router Tutorial]

[React Router Tutorial]: https://reactrouter.com/en/main/start/tutorial

## Tại sao lại sử dụng react-route-dom

Route nói chung là cách điều hướng khi ở phía client. Theo lý thuyết thông thường học ở nhà trường thì mỗi khi link (url) thay đổi, ta lại gửi một **request** đến server, server sẽ gửi về **Response** và phải **render** lại trang web. Điều ấy làm giảm hiệu năng của những trang web có nhiều phân trang.

Ý tưởng của **react-route-dom** là chỉ gửi resquest những components chưa có, còn cái có rồi thì không cần phải render lại. Cụ thể là phần **Outlet**

Ngoài ra **react-route-dom** còn cung cấp **loader** và **action** cho url. **Loader** gần giống như **middleware** trước khi truy cập trang web. Còn action để xử lý một số form đơn giản mà không cần phải gửi request về cho server.

> 💡 Hạn chế hết mức viết gửi Request về server, cố gắng xử lý phía client.

## Những tính năng chính

### Client Side Routing

- **Client Side Routing**: Cập nhật URL và giao diện người dùng mới mà không cần yêu cầu tài liệu từ máy chủ.
- **Trải nghiệm nhanh hơn và động hơn**: Không cần tải tài liệu mới và đánh giá lại tài nguyên, tạo ra trải nghiệm người dùng nhanh và động.
- **React Router**: Thư viện hỗ trợ định tuyến phía máy khách trong ứng dụng React.
- **Tính linh hoạt và tích cực**: Định tuyến phía máy khách mang lại tính linh hoạt và tích cực với với những hiệu ứng và trải nghiệm người dùng động (Dynamic user).

Sử dụng tính năng này với `Router`, `Link` và `Form` (🎗 Nhớ viết hoa chữ Form).

### Nested Routes

Mọi thành phần của URL đều phải có ý nghĩa (mỗi phần tách ra bởi dấu `/`)

Mỗi thành phần cần xác định:

- Các bố cục (layouts) được hiển thị trên trang
- Các phụ thuộc dữ liệu của những bố cục đó

Tham khảo tại link [Visualization]

[Visualization]: https://remix.run/_docs/routing

### Dynamic Segments (Đường link động (có thể thay đổi))

Ta có thể coi đó là một tham số khi người dùng truyền nội dung vào URL.

Ta xét ví dụ dưới đây:

```javascript
<Route path="projects/:projectId/tasks/:taskId" />
```

Dấu `:` thể hiện đó là một tham số khi truyền vào. Ví dụ một đường link thực tế ứng với định nghĩa trên là: `projects/dsfa9ef/tasks/222kd`

Khi render ta sẽ có cách lấy hai tham số trong một object gọi là Params.

```json
{
  "projectId": "dsfa9ef",
  "taskId": "222kd"
}
```

### Ranked Route Matching (Một vấn đề liên quan đến sử dụng tham số ở trên)

✋ When matching URLs to routes, React Router will rank the routes according to the number of segments, static segments, dynamic segments, splats, etc. and pick the most specific match.

Nói tóm lại ý đoạn trên là URL của chúng ta sẽ match với static segments trước. Xem ví dụ sẽ hiểu mà.

For example, consider these two routes:

```jsx
<Route path="/teams/:teamId" />
<Route path="/teams/new" />
```

Now consider the URL is `http://example.com/teams/new`.

Even though both routes technically match the URL (new could be the :teamId), you intuitively know that we want the second route (/teams/new) to be picked. React Router's matching algorithm knows that, too.

With ranked routes, **you don't have to worry about route ordering**.

### Active link

Này là highlight trên navbar á mà.

### Relative Link

Này là gì không rõ nữa, nói dùng là dùng `./` với dùng `../`.

Ví dụ:

Với một cấu trúc HTML như sau:

```html
<Home>
  <Project />
</Home>
```

Thì ta có bảng link tương ứng bên dưới

| In `<Project> @ /home/project/123` | Resolved `<a href>`     |
| ---------------------------------- | ----------------------- |
| `<Link to="abc">`                  | `/home/project/123/abc` |
| `<Link to=".">`                    | `/home/project/123`     |
| `<Link to="..">`                   | `/home`                 |
| `<Link to=".." relative="path">`   | `/home/project`         |

### Data loading

Này thì chắc chưa dùng tới đâu.

### Redirect

Hàm điều hướng (có thể điều hướng dựa trên một số điều kiện nào đó) chẳng hạn như user chưa login sẽ được điều hướng đi login trước khi tiếp tục truy cập.

### Data Mutation

Khi một biểu mẫu HTML được gửi đi, React Router hỗ trợ quá trình định tuyến phía máy khách cho các luồng công việc của biểu mẫu đó.

Khi một biểu mẫu được gửi đi, sự kiện điều hướng thông thường của trình duyệt sẽ bị ngăn chặn và một Yêu cầu (Request) sẽ được tạo ra, với một phần thân chứa dữ liệu của biểu mẫu (FormData) đã gửi đi. Yêu cầu này được gửi đến `<Route action>` tương ứng với `<Form action>` của biểu mẫu.

Thuộc tính "name" của các phần tử trong biểu mẫu sẽ được gửi đi trong yêu cầu:

```html
<form action="/project/new">
  <label>
    Tiêu đề dự án
    <br />
    <input type="text" name="title" />
  </label>

  <label>
    Ngày hoàn thành mục tiêu
    <br />
    <input type="date" name="due" />
  </label>
</form>
```

Yêu cầu HTML thông thường sẽ bị ngăn chặn và gửi đến hành động tương ứng của tuyến (`<Route path>`) (nơi khớp với `<form action>`), bao gồm dữ liệu của yêu cầu (`request.formData`).

```jsx
<Route
  path="/project/new"
  action={async ({ request }) => {
    const formData = await request.formData();
    const newProject = await createProject({
      title: formData.get("title"),
      due: formData.get("due"),
    });
    return redirect(`/projects/${newProject.id}`);
  }}
/>
```

Trong hành động của tuyến, chúng ta có thể sử dụng `request.formData()` để lấy dữ liệu từ yêu cầu gửi đi. Dữ liệu này sau đó có thể được sử dụng để tạo mới một dự án (`createProject`) hoặc thực hiện các thao tác khác. Trong ví dụ trên, chúng ta tạo một dự án mới với tiêu đề và ngày hoàn thành từ dữ liệu biểu mẫu, sau đó chuyển hướng đến trang dự án mới tạo.

### Data Fetcher

Fetchers (trình lấy dữ liệu) cho phép tương tác với các hành động và trình tải dữ liệu trong định tuyến mà không gây ra điều hướng trong trình duyệt, nhưng vẫn mang lại các lợi ích thông thường như xử lý lỗi, kiểm tra dữ liệu mới, xử lý gián đoạn và xử lý điều kiện cạnh tranh.

Hãy tưởng tượng một danh sách các nhiệm vụ:

```jsx
function Tasks() {
  const tasks = useLoaderData();
  return tasks.map((task) => (
    <div>
      <p>{task.name}</p>
      <ToggleCompleteButton task={task} />
    </div>
  ));
}
```

Mỗi nhiệm vụ có thể được đánh dấu hoàn thành một cách độc lập, với trạng thái đang chờ riêng và không gây ra điều hướng với trình lấy dữ liệu (fetcher):

```jsx
function ToggleCompleteButton({ task }) {
  const fetcher = useFetcher();

  return (
    <fetcher.Form method="post" action="/toggle-complete">
      <fieldset disabled={fetcher.state !== "idle"}>
        <input type="hidden" name="id" value={task.id} />
        <input
          type="hidden"
          name="status"
          value={task.complete ? "incomplete" : "complete"}
        />
        <button type="submit">
          {task.status === "complete"
            ? "Đánh dấu Chưa hoàn thành"
            : "Đánh dấu Hoàn thành"}
        </button>
      </fieldset>
    </fetcher.Form>
  );
}
```

Trong ví dụ trên, chúng ta sử dụng trình lấy dữ liệu (fetcher) để tạo ra một biểu mẫu (Form) cho việc chuyển đổi trạng thái hoàn thành/nhận nhiệm vụ. Việc gửi biểu mẫu không gây ra điều hướng trong trình duyệt, nhưng vẫn cho phép xử lý các yêu cầu phía máy chủ, xử lý lỗi và cập nhật trạng thái. Mỗi nút đánh dấu hoàn thành/nhận nhiệm vụ sẽ hoạt động độc lập với nhau mà không gây ảnh hưởng đến các nút khác trên danh sách nhiệm vụ.

### Error handling

Hầu hết các lỗi trong ứng dụng của bạn được xử lý tự động bởi React Router. Nó sẽ bắt lỗi bất kỳ lỗi nào được ném ra trong quá trình:

- Hiển thị giao diện
- Tải dữ liệu
- Cập nhật dữ liệu

Thực tế, đây gần như là tất cả các lỗi trong ứng dụng của bạn trừ những lỗi được ném ra trong các bộ xử lý sự kiện (`<button onClick>`) hoặc useEffect. Các ứng dụng React Router thường ít gặp lỗi trong cả hai trường hợp này.

Khi một lỗi được ném ra, thay vì hiển thị phần tử định tuyến, phần tử lỗi (errorElement) sẽ được hiển thị.

```jsx
<Route
  path="/"
  loader={() => {
    something.that.throws.an.error();
  }}
  // phần tử này sẽ không được hiển thị
  element={<HappyPath />}
  // thay vào đó, phần tử lỗi này sẽ được hiển thị
  errorElement={<ErrorBoundary />}
/>
```

Nếu một tuyến đường không có phần tử lỗi (errorElement), lỗi sẽ lan tỏa đến tuyến cha gần nhất có phần tử lỗi:

```jsx
<Route path="/" element={<HappyPath />} errorElement={<ErrorBoundary />}>
  {/* Các lỗi ở đây sẽ lan tỏa lên tuyến cha */}
  <Route path="login" element={<Login />} />
</Route>
```

---

Trên đây là phần overview. Giờ sẽ đi qua một số code cần nhớ.

## Coding

Các API sau đã được giới thiệu trong React Router 6.4 và chỉ hoạt động khi sử dụng một data router:

- [`route.action`](https://reactrouter.com/en/main/route/action)
- [`route.errorElement`](https://reactrouter.com/en/main/route/error-element)
- [`route.lazy`](https://reactrouter.com/en/main/route/lazy)
- [`route.loader`](https://reactrouter.com/en/main/route/loader)
- [`route.shouldRevalidate`](https://reactrouter.com/en/main/route/should-revalidate)
- [`route.handle`](https://reactrouter.com/en/main/route/route#handle)
- [`<Await>`](https://reactrouter.com/en/main/components/await)
- [`<Form>`](https://reactrouter.com/en/main/components/form)
- [`<ScrollRestoration>`](https://reactrouter.com/en/main/components/scroll-restoration)
- [`useActionData`](https://reactrouter.com/en/main/hooks/use-action-data)
- [`useAsyncError`](https://reactrouter.com/en/main/hooks/use-async-error)
- [`useAsyncValue`](https://reactrouter.com/en/main/hooks/use-async-value)
- [`useFetcher`](https://reactrouter.com/en/main/hooks/use-fetcher)
- [`useFetchers`](https://reactrouter.com/en/main/hooks/use-fetchers)
- [`useLoaderData`](https://reactrouter.com/en/main/hooks/use-loader-data)
- [`useMatches`](https://reactrouter.com/en/main/hooks/use-matches)
- [`useNavigation`](https://reactrouter.com/en/main/hooks/use-navigation)
- [`useRevalidator`](https://reactrouter.com/en/main/hooks/use-revalidator)
- [`useRouteError`](https://reactrouter.com/en/main/hooks/use-route-error)
- [`useRouteLoaderData`](https://reactrouter.com/en/main/hooks/use-route-loader-data)
- [`useSubmit`](https://reactrouter.com/en/main/hooks/use-submit)

Các API này cung cấp các công cụ và hàm hỗ trợ cho việc xử lý dữ liệu, xử lý lỗi, tải dữ liệu, xử lý chuyển hướng, và nhiều tính năng khác trong React Router. Chúng giúp tăng khả năng linh hoạt và hiệu quả trong việc quản lý và tương tác với dữ liệu trong ứng dụng của bạn.
