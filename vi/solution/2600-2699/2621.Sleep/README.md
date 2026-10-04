---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2621. Sleep](https://leetcode.com/problems/sleep)

[中文文档](/solution/2600-2699/2621.Sleep/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>millis</code>, hãy viết một hàm bất đồng bộ để tạm dừng trong <code>millis</code>&nbsp;milliseconds. Hàm có thể resolve bất kỳ giá trị nào.</p>

<p><strong>Lưu ý</strong> rằng độ lệch <em>nhỏ</em> so với <code>millis</code> trong thời gian tạm dừng thực tế là có thể chấp nhận được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> millis = 100
<strong>Đầu ra:</strong> 100
<strong>Giải thích:</strong> Hàm cần trả về một promise được resolve sau 100ms.
let t = Date.now();
sleep(100).then(() =&gt; {
  console.log(Date.now() - t); // 100
});
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> millis = 200
<strong>Đầu ra:</strong> 200
<strong>Giải thích:</strong> Hàm cần trả về một promise được resolve sau 200ms.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= millis &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần một khoảng trễ có thể chờ; `setTimeout` không phải là một Promise. Một vòng lặp bận sẽ chặn event loop.
>
> Bọc timer trong một Promise để Promise được resolve khi timer chạy xong, nhờ đó bên gọi có thể tiếp tục thực thi bất đồng bộ.
>
> Vì vậy, `sleep` trả về `new Promise(r => setTimeout(r, millis))`.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
async function sleep(millis: number): Promise<void> {
    return new Promise(r => setTimeout(r, millis));
}

/**
 * let t = Date.now()
 * sleep(100).then(() => console.log(Date.now() - t)) // 100
 */
```

#### JavaScript

```js
/**
 * @param {number} millis
 * @return {Promise}
 */
async function sleep(millis) {
    return new Promise(r => setTimeout(r, millis));
}

/**
 * let t = Date.now()
 * sleep(100).then(() => console.log(Date.now() - t)) // 100
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
