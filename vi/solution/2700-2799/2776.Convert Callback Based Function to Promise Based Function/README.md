---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2776. Convert Callback Based Function to Promise Based Function 🔒](https://leetcode.com/problems/convert-callback-based-function-to-promise-based-function)

[中文文档](/solution/2700-2799/2776.Convert%20Callback%20Based%20Function%20to%20Promise%20Based%20Function/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy viết một hàm nhận một hàm khác <code>fn</code> và chuyển hàm dựa trên callback thành hàm dựa trên Promise.</p>

<p>Hàm <code>fn</code> nhận một callback làm đối số đầu tiên, cùng với mọi đối số bổ sung <code>args</code> được truyền vào dưới dạng các input riêng biệt.</p>

<p>Hàm <code>promisify</code> trả về một hàm mới có nhiệm vụ trả về một promise. Promise phải resolve với đối số được truyền vào tham số đầu tiên của callback khi callback được gọi mà không có lỗi, và reject với lỗi khi callback được gọi với lỗi ở đối số thứ hai.</p>

<p>Sau đây là một ví dụ về hàm có thể được truyền vào <code>promisify</code>.</p>

<pre>
function sum(callback, a, b) {
  if (a &lt; 0 || b &lt; 0) {
&nbsp;   const err = Error(&#39;a and b must be positive&#39;);
    callback(undefined, err);
&nbsp; } else {
    callback(a + b);
&nbsp; }
}
</pre>

<p>Đây là code tương đương sử dụng promise:</p>

<pre>
async function sum(a, b) {
  if (a &lt; 0 || b &lt; 0) {
    throw Error(&#39;a and b must be positive&#39;);
&nbsp; } else {
    return a + b;
&nbsp; }
}
</pre>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
fn = (callback, a, b, c) =&gt; {
    callback(a * b * c);
}
args = [1, 2, 3]
<strong>Đầu ra:</strong> {&quot;resolved&quot;: 6}
<strong>Giải thích:</strong>
const asyncFunc = promisify(fn);
asyncFunc(1, 2, 3).then(console.log); // 6

fn được gọi với một callback ở vị trí đầu tiên và args ở các vị trí còn lại. Phiên bản dựa trên promise của fn sẽ resolve giá trị 6 khi được gọi với (1, 2, 3).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
fn = (callback, a, b, c) =&gt; {
    callback(a * b * c, &quot;Promise Rejected&quot;);
}
args = [4, 5, 6]
<strong>Đầu ra:</strong> {&quot;rejected&quot;: &quot;Promise Rejected&quot;}
<strong>Giải thích:</strong>
const asyncFunc = promisify(fn);
asyncFunc(4, 5, 6).catch(console.log); // &quot;Promise Rejected&quot;

fn được gọi với một callback ở vị trí đầu tiên và args ở các vị trí còn lại. Đối số thứ hai của callback là một thông báo lỗi, vì vậy khi fn được gọi, promise sẽ bị reject với thông báo lỗi được cung cấp trong callback. Lưu ý rằng đối số đầu tiên truyền vào callback là gì cũng không ảnh hưởng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= args.length &lt;= 100</code></li>
	<li><code>0 &lt;= args[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Trước hết, cần chuyển một hàm nhận $next(data, error)$ thành hàm trả về Promise. Nếu chỉ gọi hàm gốc như cũ, ta không thể nối kết quả thành công và thất bại với $then/catch$.
>
> Hàm async được trả về sẽ tạo một Promise, bọc $resolve/reject$ thành $next$, rồi truyền các đối số còn lại vào $fn$. Nếu có $error$, Promise bị reject; nếu không, $data$ được resolve.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type CallbackFn = (next: (data: number, error: string) => void, ...args: number[]) => void;
type Promisified = (...args: number[]) => Promise<number>;

function promisify(fn: CallbackFn): Promisified {
    return async function (...args) {
        return new Promise((resolve, reject) => {
            fn(
                (data, error) => {
                    if (error) {
                        reject(error);
                    } else {
                        resolve(data);
                    }
                },
                ...args,
            );
        });
    };
}

/**
 * const asyncFunc = promisify(callback => callback(42));
 * asyncFunc().then(console.log); // 42
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
