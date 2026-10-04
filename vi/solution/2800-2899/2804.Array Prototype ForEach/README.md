---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2804. Array Prototype ForEach 🔒](https://leetcode.com/problems/array-prototype-foreach)

[中文文档](/solution/2800-2899/2804.Array%20Prototype%20ForEach/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy viết phiên bản của phương thức&nbsp;<code>forEach</code>&nbsp;để mở rộng tất cả các mảng, sao cho bạn có thể gọi phương thức&nbsp;<code>array.forEach(callback, context)</code>&nbsp;trên bất kỳ mảng nào và phương thức này sẽ thực thi <code>callback</code> trên từng phần tử của mảng.&nbsp;Phương thức&nbsp;<code>forEach</code> không được trả về giá trị nào.</p>

<p><code>callback</code> nhận các đối số sau:</p>

<ul>
	<li><code>currentValue</code> -&nbsp;đại diện cho phần tử hiện tại đang được xử lý trong mảng. Đây là giá trị của phần tử ở lần lặp hiện tại.</li>
	<li><code>index</code> -&nbsp;đại diện cho chỉ số của phần tử hiện tại đang được xử lý trong mảng.</li>
	<li><code>array</code> -&nbsp;đại diện cho chính mảng đó, cho phép truy cập toàn bộ mảng bên trong hàm callback.</li>
</ul>

<p><code>context</code> là đối tượng được truyền làm tham số context của hàm cho hàm&nbsp;<code>callback</code>, đảm bảo từ khóa&nbsp;<code>this</code>&nbsp;bên trong hàm&nbsp;<code>callback</code> tham chiếu đến đối tượng&nbsp;<code>context</code> này.</p>

<p>Hãy thử cài đặt mà không sử dụng các phương thức có sẵn của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
arr = [1,2,3],
callback = (val, i, arr) =&gt; arr[i] = val * 2,
context = {&quot;context&quot;:true}
<strong>Đầu ra:</strong> [2,4,6]
<strong>Giải thích:</strong>
arr.forEach(callback, context)&nbsp;
console.log(arr) // [2,4,6]

Callback được thực thi trên từng phần tử của mảng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
arr = [true, true, false, false],
callback = (val, i, arr) =&gt; arr[i] = this,
context = {&quot;context&quot;: false}
<strong>Đầu ra:</strong> [{&quot;context&quot;:false},{&quot;context&quot;:false},{&quot;context&quot;:false},{&quot;context&quot;:false}]
<strong>Giải thích:</strong>
arr.forEach(callback, context)&nbsp;
console.log(arr) // [{&quot;context&quot;:false},{&quot;context&quot;:false},{&quot;context&quot;:false},{&quot;context&quot;:false}]

Callback được thực thi trên từng phần tử của mảng với context chính xác.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
arr = [true, true, false, false],
callback = (val, i, arr) =&gt; arr[i] = !val,
context = {&quot;context&quot;: 5}
<strong>Đầu ra:</strong> [false,false,true,true]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>arr</code> là một mảng JSON hợp lệ</li>
	<li><code>context</code> là một đối tượng JSON hợp lệ</li>
	<li><code>fn</code> là một hàm</li>
	<li><code>0 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần cài đặt `forEach` theo đúng thứ tự chỉ số và hỗ trợ liên kết `this` tùy chọn. Duyệt $i$ trong $[0,n)$ và gọi `callback.call(context, this[i], i, this)` để truyền giá trị, chỉ số và mảng theo yêu cầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
Array.prototype.forEach = function (callback: Function, context: any): void {
    for (let i = 0; i < this.length; ++i) {
        callback.call(context, this[i], i, this);
    }
};

/**
 *  const arr = [1,2,3];
 *  const callback = (val, i, arr) => arr[i] = val * 2;
 *  const context = {"context":true};
 *
 *  arr.forEach(callback, context)
 *
 *  console.log(arr) // [2,4,6]
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
