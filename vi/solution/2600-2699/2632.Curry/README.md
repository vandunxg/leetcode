---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2632. Curry 🔒](https://leetcode.com/problems/curry)

[中文文档](/solution/2600-2699/2632.Curry/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một hàm&nbsp;<code>fn</code>, hãy trả về phiên bản&nbsp;<strong>curried</strong>&nbsp;của hàm đó.</p>

<p>Hàm&nbsp;<strong>curried</strong>&nbsp;là một hàm nhận ít hơn hoặc bằng số tham số của hàm ban đầu và trả về một hàm&nbsp;<strong>curried</strong>&nbsp;khác hoặc chính giá trị mà hàm ban đầu sẽ trả về.</p>

<p>Trên thực tế, nếu gọi hàm ban đầu như&nbsp;<code>sum(1,2,3)</code>, bạn sẽ gọi phiên bản&nbsp;<strong>curried</strong>&nbsp;như <code>csum(1)(2)(3)<font face="sans-serif, Arial, Verdana, Trebuchet MS">,&nbsp;</font></code><code>csum(1)(2,3)</code>,&nbsp;<code>csum(1,2)(3)</code>, hoặc&nbsp;<code>csum(1,2,3)</code>. Tất cả các cách gọi hàm&nbsp;<strong>curried</strong>&nbsp;này đều phải trả về cùng giá trị như hàm ban đầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
fn = function sum(a, b, c) { return a + b + c; }
inputs = [[1],[2],[3]]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
Đoạn code được thực thi là:
const curriedSum = curry(fn);
curriedSum(1)(2)(3) === 6;
curriedSum(1)(2)(3) phải trả về cùng giá trị như sum(1, 2, 3).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
fn = function sum(a, b, c) { return a + b + c; }
inputs = [[1,2],[3]]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
curriedSum(1, 2)(3) phải trả về cùng giá trị như sum(1, 2, 3).</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
fn = function sum(a, b, c) { return a + b + c; }
inputs = [[],[],[1,2,3]]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
Bạn có thể truyền các tham số theo bất kỳ cách nào, bao gồm truyền tất cả cùng lúc hoặc không truyền gì.
curriedSum()()(1, 2, 3) phải trả về cùng giá trị như sum(1, 2, 3).
</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong>
fn = function life() { return 42; }
inputs = [[]]
<strong>Đầu ra:</strong> 42
<strong>Giải thích:</strong>
Currying một hàm không nhận tham số về cơ bản không làm gì cả.
curriedLife() === 42
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= inputs.length &lt;= 1000</code></li>
	<li><code>0 &lt;= inputs[i][j] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= fn.length &lt;= 1000</code></li>
	<li><code>inputs.flat().length == fn.length</code></li>
	<li>các tham số của hàm được định nghĩa tường minh</li>
	<li>Nếu <code>fn.length &gt; 0</code>&nbsp;thì mảng cuối cùng trong <code>inputs</code> không rỗng</li>
	<li>Nếu&nbsp;<code>fn.length === 0</code>&nbsp;thì <code>inputs.length === 1</code>&nbsp;</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các đối số có thể được truyền qua nhiều lần gọi và chỉ được tính toán sau khi đạt đủ số lượng tham số của hàm ban đầu. Gọi hàm ngay lập tức sẽ không hỗ trợ được `csum(1)(2)`.
>
> So sánh số đối số đã thu thập với `fn.length`: nếu chưa đủ thì trả về một hàm tiếp tục thu thập, nếu đủ thì gọi hàm đúng một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function curry(fn: Function): Function {
    return function curried(...args) {
        if (args.length >= fn.length) {
            return fn(...args);
        }
        return (...nextArgs) => curried(...args, ...nextArgs);
    };
}

/**
 * function sum(a, b) { return a + b; }
 * const csum = curry(sum);
 * csum(1)(2) // 3
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
