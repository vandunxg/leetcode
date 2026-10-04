---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2635. Apply Transform Over Each Element in Array](https://leetcode.com/problems/apply-transform-over-each-element-in-array)

[中文文档](/solution/2600-2699/2635.Apply%20Transform%20Over%20Each%20Element%20in%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên&nbsp;<code>arr</code>&nbsp;và một hàm ánh xạ&nbsp;<code>fn</code>, hãy trả về&nbsp;một mảng mới trong đó mỗi phần tử được biến đổi.</p>

<p>Mảng được trả về phải được tạo sao cho&nbsp;<code>returnedArray[i] = fn(arr[i], i)</code>.</p>

<p>Hãy giải bài toán mà không sử dụng phương thức tích hợp sẵn <code>Array.map</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,3], fn = function plusone(n) { return n + 1; }
<strong>Đầu ra:</strong> [2,3,4]
<strong>Giải thích:</strong>
const newArray = map(arr, plusone); // [2,3,4]
Hàm tăng mỗi giá trị trong mảng lên một đơn vị.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,3], fn = function plusI(n, i) { return n + i; }
<strong>Đầu ra:</strong> [1,3,5]
<strong>Giải thích:</strong> Hàm tăng mỗi giá trị bằng chỉ số của phần tử đó trong mảng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [10,20,30], fn = function constant() { return 42; }
<strong>Đầu ra:</strong> [42,42,42]
<strong>Giải thích:</strong> Hàm luôn trả về 42.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= arr.length &lt;= 1000</code></li>
	<li><code><font face="monospace">-10<sup>9</sup>&nbsp;&lt;= arr[i] &lt;= 10<sup>9</sup></font></code></li>
	<li><code>fn</code> trả về một số nguyên.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Không được sử dụng `Array.map`. Có thể ghi đè tại chỗ, nên ta thay phần tử tại mỗi chỉ số bằng $fn(arr[i],i)$ rồi trả về chính mảng đó.

<!-- thinking:end -->

Duyệt qua mảng $arr$, với mỗi phần tử $arr[i]$, thay thế nó bằng $fn(arr[i], i)$. Cuối cùng, trả về mảng $arr$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $arr$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### TypeScript

```ts
function map(arr: number[], fn: (n: number, i: number) => number): number[] {
    for (let i = 0; i < arr.length; ++i) {
        arr[i] = fn(arr[i], i);
    }
    return arr;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
