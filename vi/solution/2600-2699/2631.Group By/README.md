---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2631. Group By](https://leetcode.com/problems/group-by)

[中文文档](/solution/2600-2699/2631.Group%20By/README.md)

## Mô tả

<!-- description:start -->

<p>Viết code để mở rộng tất cả các mảng, cho phép bạn gọi phương thức&nbsp;<code>array.groupBy(fn)</code>&nbsp;trên bất kỳ mảng nào; phương thức này sẽ trả về một phiên bản <strong>đã nhóm</strong> của mảng.</p>

<p>Một mảng <strong>đã nhóm</strong> là một object trong đó mỗi&nbsp;key&nbsp;là kết quả của <code>fn(arr[i])</code> và mỗi value là một mảng chứa tất cả các phần tử trong mảng ban đầu tạo ra key đó.</p>

<p>Callback&nbsp;<code>fn</code>&nbsp;được cung cấp sẽ nhận một phần tử trong mảng và trả về một key dạng chuỗi.</p>

<p>Thứ tự của các phần tử trong mỗi danh sách value phải giống với thứ tự xuất hiện của chúng trong mảng. Thứ tự của các key có thể tùy ý.</p>

<p>Hãy giải bài toán mà không sử dụng hàm&nbsp;<code>_.groupBy</code>&nbsp;của lodash.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
array = [
&nbsp; {&quot;id&quot;:&quot;1&quot;},
&nbsp; {&quot;id&quot;:&quot;1&quot;},
&nbsp; {&quot;id&quot;:&quot;2&quot;}
],
fn = function (item) {
&nbsp; return item.id;
}
<strong>Đầu ra:</strong>
{
&nbsp; &quot;1&quot;: [{&quot;id&quot;: &quot;1&quot;}, {&quot;id&quot;: &quot;1&quot;}], &nbsp;
&nbsp; &quot;2&quot;: [{&quot;id&quot;: &quot;2&quot;}]
}
<strong>Giải thích:</strong>
Đầu ra được tạo ra từ array.groupBy(fn).
Hàm selector lấy trường &quot;id&quot; từ mỗi phần tử.
Có hai object có &quot;id&quot; bằng 1. Cả hai object được đưa vào mảng đầu tiên.
Có một object có &quot;id&quot; bằng 2. Object đó được đưa vào mảng thứ hai.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
array = [
&nbsp; [1, 2, 3],
&nbsp; [1, 3, 5],
&nbsp; [1, 5, 9]
]
fn = function (list) {
&nbsp; return String(list[0]);
}
<strong>Đầu ra:</strong>
{
&nbsp; &quot;1&quot;: [[1, 2, 3], [1, 3, 5], [1, 5, 9]]
}
<strong>Giải thích:</strong>
Mảng có thể chứa các kiểu dữ liệu bất kỳ. Trong trường hợp này, hàm selector xác định key là phần tử đầu tiên trong mảng.
Tất cả các mảng đều có 1 là phần tử đầu tiên nên được nhóm lại với nhau.
{
  &quot;1&quot;: [[1, 2, 3], [1, 3, 5], [1, 5, 9]]
}
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
array = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
fn = function (n) {
&nbsp; return String(n &gt; 5);
}
<strong>Đầu ra:</strong>
{
&nbsp; &quot;true&quot;: [6, 7, 8, 9, 10],
&nbsp; &quot;false&quot;: [1, 2, 3, 4, 5]
}
<strong>Giải thích:</strong>
Hàm selector chia mảng dựa trên việc mỗi số có lớn hơn 5 hay không.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= array.length &lt;= 10<sup>5</sup></code></li>
	<li><code>fn</code> trả về một chuỗi</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các phần tử phải được nhóm theo key do callback trả về. Nếu thu thập các key trước thì sẽ cần duyệt mảng lần thứ hai. Một lần `reduce` có thể vừa tính key vừa đưa phần tử vào bucket tương ứng.
>
> Tạo một danh sách khi gặp một key lần đầu, rồi thêm phần tử vào danh sách đó ở những lần sau.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
declare global {
    interface Array<T> {
        groupBy(fn: (item: T) => string): Record<string, T[]>;
    }
}

Array.prototype.groupBy = function (fn) {
    return this.reduce((acc, item) => {
        const key = fn(item);
        if (acc[key]) {
            acc[key].push(item);
        } else {
            acc[key] = [item];
        }
        return acc;
    }, {});
};

/**
 * [1,2,3].groupBy(String) // {"1":[1],"2":[2],"3":[3]}
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
