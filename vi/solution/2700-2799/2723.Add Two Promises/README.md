---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2723. Add Two Promises](https://leetcode.com/problems/add-two-promises)

[中文文档](/solution/2700-2799/2723.Add%20Two%20Promises/README.md)

## Mô tả

<!-- description:start -->

Cho hai promise <code>promise1</code> và <code>promise2</code>, hãy trả về một promise mới. Cả <code>promise1</code> và <code>promise2</code>&nbsp;đều sẽ resolve với một số. Promise được trả về phải resolve với tổng của hai số đó.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
promise1 = new Promise(resolve =&gt; setTimeout(() =&gt; resolve(2), 20)),
promise2 = new Promise(resolve =&gt; setTimeout(() =&gt; resolve(5), 60))
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Hai promise đầu vào lần lượt resolve với các giá trị 2 và 5. Promise được trả về phải resolve với giá trị 2 + 5 = 7. Thời điểm promise được resolve không được chấm điểm trong bài này.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
promise1 = new Promise(resolve =&gt; setTimeout(() =&gt; resolve(10), 50)),
promise2 = new Promise(resolve =&gt; setTimeout(() =&gt; resolve(-12), 30))
<strong>Đầu ra:</strong> -2
<strong>Giải thích:</strong> Hai promise đầu vào lần lượt resolve với các giá trị 10 và -12. Promise được trả về phải resolve với giá trị 10 + -12 = -2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>promise1</code> và <code>promise2</code> là các promise resolve với một số</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chờ cả hai promise hoàn tất, rồi trả về tổng của chúng. $Promise.all$ kết hợp với phép cộng là đúng, nhưng phải bọc kết quả trong một mảng thừa.
>
> Chỉ cần await lần lượt hai promise rồi cộng các giá trị; promise thứ hai đã bắt đầu chạy, nên tổng thời gian chờ vẫn là promise hoàn tất chậm hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
async function addTwoPromises(
    promise1: Promise<number>,
    promise2: Promise<number>,
): Promise<number> {
    return (await promise1) + (await promise2);
}

/**
 * addTwoPromises(Promise.resolve(2), Promise.resolve(2))
 *   .then(console.log); // 4
 */
```

#### JavaScript

```js
var addTwoPromises = async function (promise1, promise2) {
    return (await promise1) + (await promise2);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
