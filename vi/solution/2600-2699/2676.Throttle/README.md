---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2676. Throttle 🔒](https://leetcode.com/problems/throttle)

[中文文档](/solution/2600-2699/2676.Throttle/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một hàm <code>fn</code> và một khoảng thời gian tính bằng mili giây <code>t</code>, hãy trả về một phiên bản <strong>throttled</strong> của hàm đó.</p>

<p>Hàm <strong>throttled</strong> được gọi lần đầu mà không bị trì hoãn. Sau đó, trong khoảng thời gian <code>t</code> mili giây, hàm không thể được thực thi nhưng phải lưu lại các tham số mới nhất để gọi <code>fn</code> với chúng sau khi thời gian trì hoãn kết thúc.</p>

<p>Ví dụ, với <code>t = 50ms</code>, hàm được gọi tại các thời điểm <code>30ms</code>, <code>40ms</code> và <code>60ms</code>.</p>

<p>Tại <code>30ms</code>, không bị trì hoãn, hàm <strong>throttled</strong> <code>fn</code> phải được gọi với các tham số tương ứng, đồng thời việc gọi hàm <strong>throttled</strong> <code>fn</code> sẽ bị chặn trong <code>t</code> mili giây tiếp theo.</p>

<p>Tại <code>40ms</code>, hàm chỉ cần lưu lại các tham số.</p>

<p>Tại <code>60ms</code>, các tham số phải ghi đè lên các tham số đang được lưu từ lần gọi thứ hai vì lần gọi thứ hai và thứ ba đều xảy ra trước <code>80ms</code>. Khi thời gian trì hoãn kết thúc, hàm <strong>throttled</strong> <code>fn</code> phải được gọi với các tham số mới nhất được cung cấp trong thời gian trì hoãn, đồng thời tạo thêm một khoảng thời gian trì hoãn khác là <code>80ms + t</code>.</p>

<p><img alt="Throttle Diagram" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2676.Throttle/images/screen-shot-2023-04-08-at-120313-pm.png" style="width: 1156px; height: 372px;" />Sơ đồ trên cho thấy throttle biến đổi các sự kiện như thế nào. Mỗi hình chữ nhật biểu thị 100ms và thời gian throttle là 400ms. Mỗi màu biểu thị một nhóm đầu vào khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
t = 100,
calls = [
  {&quot;t&quot;:20,&quot;inputs&quot;:[1]}
]
<strong>Đầu ra:</strong> [{&quot;t&quot;:20,&quot;inputs&quot;:[1]}]
<strong>Giải thích:</strong> Lần gọi thứ nhất luôn được thực thi mà không bị trì hoãn
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
t = 50,
calls = [
  {&quot;t&quot;:50,&quot;inputs&quot;:[1]},
  {&quot;t&quot;:75,&quot;inputs&quot;:[2]}
]
<strong>Đầu ra:</strong> [{&quot;t&quot;:50,&quot;inputs&quot;:[1]},{&quot;t&quot;:100,&quot;inputs&quot;:[2]}]
<strong>Giải thích:</strong>
Lần gọi thứ nhất gọi hàm với các tham số (1) mà không bị trì hoãn.
Lần gọi thứ hai xảy ra tại 75ms, trong thời gian trì hoãn vì 50ms + 50ms = 100ms, nên lần gọi tiếp theo có thể thực hiện tại 100ms. Do đó, ta lưu các tham số từ lần gọi thứ hai để sử dụng trong callback của lần gọi thứ nhất.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
t = 70,
calls = [
  {&quot;t&quot;:50,&quot;inputs&quot;:[1]},
  {&quot;t&quot;:75,&quot;inputs&quot;:[2]},
  {&quot;t&quot;:90,&quot;inputs&quot;:[8]},
  {&quot;t&quot;: 140, &quot;inputs&quot;:[5,7]},
  {&quot;t&quot;: 300, &quot;inputs&quot;: [9,4]}
]
<strong>Đầu ra:</strong> [{&quot;t&quot;:50,&quot;inputs&quot;:[1]},{&quot;t&quot;:120,&quot;inputs&quot;:[8]},{&quot;t&quot;:190,&quot;inputs&quot;:[5,7]},{&quot;t&quot;:300,&quot;inputs&quot;:[9,4]}]
<strong>Giải thích:</strong>
Lần gọi thứ nhất gọi hàm với các tham số (1) mà không bị trì hoãn.
Lần gọi thứ hai xảy ra tại 75ms trong thời gian trì hoãn vì 50ms + 70ms = 120ms, nên chỉ cần lưu lại các tham số.
Lần gọi thứ ba cũng xảy ra trong thời gian trì hoãn. Vì ta chỉ cần các tham số mới nhất của hàm, các tham số trước đó sẽ bị ghi đè. Sau thời gian trì hoãn, ta thực hiện callback tại 120ms với các tham số đã lưu. Callback này tạo thêm một khoảng thời gian trì hoãn, kết thúc tại 120ms + 70ms = 190ms, để lần gọi hàm tiếp theo có thể thực hiện tại 190ms.
Lần gọi thứ tư xảy ra tại 140ms trong thời gian trì hoãn, nên sẽ được gọi trong callback tại 190ms. Lần gọi đó tạo thêm một khoảng thời gian trì hoãn, kết thúc tại 190ms + 70ms = 260ms.
Lần gọi thứ năm xảy ra tại 300ms, sau 260ms, nên được gọi ngay lập tức và tạo thêm một khoảng thời gian trì hoãn, kết thúc tại 300ms + 70ms = 370ms.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= t &lt;= 1000</code></li>
	<li><code>1 &lt;= calls.length &lt;= 10</code></li>
	<li><code>0 &lt;= calls[i].t &lt;= 1000</code></li>
	<li><code>0 &lt;= calls[i].inputs[j], calls[i].inputs.length &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Throttle thực thi ngay ở lần gọi đầu tiên trong một cửa sổ, sau đó gọi lại với các tham số mới nhất. Không giống debounce, cờ `pending` đánh dấu khoảng thời gian chờ.
>
> Các lần gọi trong khoảng thời gian chờ chỉ cập nhật `nextArgs`; khi timer kết thúc, nếu còn tham số thì gọi đệ quy wrapper để mở cửa sổ tiếp theo.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type F = (...args: any[]) => void;

const throttle = (fn: F, t: number): F => {
    let pending = false;
    let nextArgs;
    const wrapper = (...args) => {
        nextArgs = args;
        if (!pending) {
            fn(...args);
            pending = true;
            nextArgs = undefined;
            setTimeout(() => {
                pending = false;
                if (nextArgs) wrapper(...nextArgs);
            }, t);
        }
    };
    return wrapper;
};

/**
 * const throttled = throttle(console.log, 100);
 * throttled("log"); // logged immediately.
 * throttled("log"); // logged at t=100ms.
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
