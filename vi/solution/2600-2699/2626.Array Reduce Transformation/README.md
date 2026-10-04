---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2626. Array Reduce Transformation](https://leetcode.com/problems/array-reduce-transformation)

[中文文档](/solution/2600-2699/2626.Array%20Reduce%20Transformation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, một hàm reducer <code>fn</code> và một giá trị ban đầu <code>init</code>, hãy trả về kết quả cuối cùng nhận được bằng cách thực thi hàm <code>fn</code> trên từng phần tử của mảng theo thứ tự, truyền giá trị trả về từ phép tính trên phần tử trước đó vào lần tính tiếp theo.</p>

<p>Kết quả này được tạo ra qua các phép tính sau: <code>val = fn(init, nums[0]), val = fn(val, nums[1]), val = fn(val, nums[2]), ...</code> cho đến khi mọi phần tử trong mảng đều được xử lý. Sau đó, trả về giá trị cuối cùng của <code>val</code>.</p>

<p>Nếu độ dài của mảng bằng 0, hàm phải trả về <code>init</code>.</p>

<p>Hãy giải bài toán mà không sử dụng phương thức tích hợp sẵn <code>Array.reduce</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
nums = [1,2,3,4]
fn = function sum(accum, curr) { return accum + curr; }
init = 0
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong>
ban đầu, giá trị là init=0.
(0) + nums[0] = 1
(1) + nums[1] = 3
(3) + nums[2] = 6
(6) + nums[3] = 10
Đáp án cuối cùng là 10.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
nums = [1,2,3,4]
fn = function sum(accum, curr) { return accum + curr * curr; }
init = 100
<strong>Đầu ra:</strong> 130
<strong>Giải thích:</strong>
ban đầu, giá trị là init=100.
(100) + nums[0] * nums[0] = 101
(101) + nums[1] * nums[1] = 105
(105) + nums[2] * nums[2] = 114
(114) + nums[3] * nums[3] = 130
Đáp án cuối cùng là 130.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
nums = []
fn = function sum(accum, curr) { return 0; }
init = 25
<strong>Đầu ra:</strong> 25
<strong>Giải thích:</strong> Với mảng rỗng, đáp án luôn là init.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>0 &lt;= init &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Không được sử dụng `reduce` tích hợp sẵn; chúng ta thực hiện phép fold bắt đầu từ giá trị khởi tạo. Với mảng rỗng, phải trả về `init`, nên vòng lặp có thể không thực hiện lần nào.
>
> Bắt đầu từ $init$ và thay giá trị tích lũy bằng $fn(acc,x)$ sau mỗi phần tử, đúng theo thứ tự left-fold của hàm native.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type Fn = (accum: number, curr: number) => number;

function reduce(nums: number[], fn: Fn, init: number): number {
    let acc: number = init;
    for (const x of nums) {
        acc = fn(acc, x);
    }
    return acc;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
