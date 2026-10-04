---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2620. Counter](https://leetcode.com/problems/counter)

[中文文档](/solution/2600-2699/2620.Counter/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên&nbsp;<code>n</code>, hãy trả về một hàm <code>counter</code>. Hàm <code>counter</code> này ban đầu trả về&nbsp;<code>n</code>, sau đó mỗi lần được gọi sẽ trả về giá trị lớn hơn lần trước 1 đơn vị (<code>n</code>, <code>n + 1</code>, <code>n + 2</code>, v.v.).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
n = 10
[&quot;call&quot;,&quot;call&quot;,&quot;call&quot;]
<strong>Đầu ra:</strong> [10,11,12]
<strong>Giải thích:
</strong>counter() = 10 // The first time counter() is called, it returns n.
counter() = 11 // Returns 1 more than the previous time.
counter() = 12 // Returns 1 more than the previous time.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
n = -2
[&quot;call&quot;,&quot;call&quot;,&quot;call&quot;,&quot;call&quot;,&quot;call&quot;]
<strong>Đầu ra:</strong> [-2,-1,0,1,2]
<strong>Giải thích:</strong> counter() ban đầu trả về -2. Sau đó tăng thêm 1 sau mỗi lần gọi tiếp theo.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>-1000<sup>&nbsp;</sup>&lt;= n &lt;= 1000</code></li>
	<li><code>0 &lt;= calls.length &lt;= 1000</code></li>
	<li><code>calls[i] === &quot;call&quot;</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần gọi phải trả về số nguyên kế tiếp, bắt đầu từ $n$ được capture. Nếu dùng một biến đếm global, các instance `createCounter` sẽ ảnh hưởng lẫn nhau.
>
> Closure lưu $i$; phép hậu tăng trả về giá trị hiện tại, nên lần gọi đầu tiên trả về $n$.
>
> Hàm bên ngoài chỉ khởi tạo state; hàm bên trong sở hữu state có thể thay đổi.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function createCounter(n: number): () => number {
    let i = n;
    return function () {
        return i++;
    };
}

/**
 * const counter = createCounter(10)
 * counter() // 10
 * counter() // 11
 * counter() // 12
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
