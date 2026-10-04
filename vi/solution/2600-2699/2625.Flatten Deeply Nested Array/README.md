---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2625. Flatten Deeply Nested Array](https://leetcode.com/problems/flatten-deeply-nested-array)

[中文文档](/solution/2600-2699/2625.Flatten%20Deeply%20Nested%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <strong>mảng đa chiều</strong> <code>arr</code> và độ sâu <code>n</code>, hãy trả về phiên bản <strong>đã làm phẳng</strong> của mảng đó.</p>

<p><strong>Mảng đa chiều</strong> là một cấu trúc dữ liệu đệ quy chứa các số nguyên hoặc các <strong>mảng đa chiều</strong> khác.</p>

<p><strong>Mảng đã làm phẳng</strong> là phiên bản của mảng đó trong đó một phần hoặc toàn bộ các mảng con được loại bỏ và thay thế bằng các phần tử thực tế trong những mảng con đó. Chỉ thực hiện thao tác làm phẳng này nếu độ sâu hiện tại của mảng lồng nhau nhỏ hơn <code>n</code>. Độ sâu của các phần tử trong mảng đầu tiên được xem là <code>0</code>.</p>

<p>Hãy giải bài toán mà không sử dụng phương thức dựng sẵn <code>Array.flat</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
arr = [1, 2, 3, [4, 5, 6], [7, 8, [9, 10, 11], 12], [13, 14, 15]]
n = 0
<strong>Đầu ra</strong>
[1, 2, 3, [4, 5, 6], [7, 8, [9, 10, 11], 12], [13, 14, 15]]

<strong>Giải thích</strong>
Truyền độ sâu n=0 sẽ luôn trả về mảng ban đầu. Đó là vì độ sâu nhỏ nhất có thể của một mảng con (0) không nhỏ hơn n=0. Do đó, không có mảng con nào được làm phẳng. </pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào</strong>
arr = [1, 2, 3, [4, 5, 6], [7, 8, [9, 10, 11], 12], [13, 14, 15]]
n = 1
<strong>Đầu ra</strong>
[1, 2, 3, 4, 5, 6, 7, 8, [9, 10, 11], 12, 13, 14, 15]

<strong>Giải thích</strong>
Các mảng con bắt đầu bằng 4, 7 và 13 đều được làm phẳng. Đó là vì độ sâu của chúng là 0, nhỏ hơn 1. Tuy nhiên, [9, 10, 11] vẫn chưa được làm phẳng vì độ sâu của nó là 1.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào</strong>
arr = [[1, 2, 3], [4, 5, 6], [7, 8, [9, 10, 11], 12], [13, 14, 15]]
n = 2
<strong>Đầu ra</strong>
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]

<strong>Giải thích</strong>
Độ sâu lớn nhất của mọi mảng con là 1. Vì vậy, tất cả chúng đều được làm phẳng.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= count of numbers in arr &lt;=&nbsp;10<sup>5</sup></code></li>
	<li><code>0 &lt;= count of subarrays in arr &lt;=&nbsp;10<sup>5</sup></code></li>
	<li><code>maxDepth &lt;= 1000</code></li>
	<li><code>-1000 &lt;= each number &lt;= 1000</code></li>
	<li><code><font face="monospace">0 &lt;= n &lt;= 1000</font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Làm phẳng tối đa $n$ cấp; các mảng ở sâu hơn vẫn giữ nguyên cấu trúc lồng nhau. `flat(Infinity)` sẽ làm phẳng quá mức. Độ sâu bị giới hạn khiến đệ quy trở thành lựa chọn tự nhiên.
>
> Nếu $n=0$, trả về mảng không thay đổi; nếu không, đệ quy trên từng phần tử con với $n-1$ và thêm các phần tử không phải mảng vào kết quả.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type MultiDimensionalArray = (number | MultiDimensionalArray)[];

var flat = function (arr: MultiDimensionalArray, n: number): MultiDimensionalArray {
    if (!n) {
        return arr;
    }
    const ans: MultiDimensionalArray = [];
    for (const x of arr) {
        if (Array.isArray(x) && n) {
            ans.push(...flat(x, n - 1));
        } else {
            ans.push(x);
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
