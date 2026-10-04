---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2695. Array Wrapper](https://leetcode.com/problems/array-wrapper)

[中文文档](/solution/2600-2699/2695.Array%20Wrapper/README.md)

## Mô tả

<!-- description:start -->

<p>Tạo một lớp <code>ArrayWrapper</code> nhận một mảng số nguyên trong constructor. Lớp này có hai tính năng:</p>

<ul>
	<li>Khi cộng hai instance của lớp này bằng toán tử <code>+</code>, giá trị nhận được là tổng tất cả phần tử trong cả hai mảng.</li>
	<li>Khi gọi hàm <code>String()</code> trên instance, hàm sẽ trả về một chuỗi các phần tử được phân tách bằng dấu phẩy và bao quanh bởi dấu ngoặc vuông. Ví dụ, <code>[1,2,3]</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [[1,2],[3,4]], operation = &quot;Add&quot;
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong>
const obj1 = new ArrayWrapper([1,2]);
const obj2 = new ArrayWrapper([3,4]);
obj1 + obj2; // 10
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [[23,98,42,70]], operation = &quot;String&quot;
<strong>Đầu ra:</strong> &quot;[23,98,42,70]&quot;
<strong>Giải thích:</strong>
const obj = new ArrayWrapper([23,98,42,70]);
String(obj); // &quot;[23,98,42,70]&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [[],[]], operation = &quot;Add&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
const obj1 = new ArrayWrapper([]);
const obj2 = new ArrayWrapper([]);
obj1 + obj2; // 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i]&nbsp;&lt;= 1000</code></li>
	<li><code>Note: nums is the array passed to the constructor</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Phép cộng phải cho ra tổng các phần tử, còn chuyển thành chuỗi phải có dạng `[a,b,...]`. Phép cộng mặc định giữa các object không tính tổng mảng. `valueOf` trả về tổng đã tính trước, còn `toString` ghép các phần tử bên trong dấu ngoặc vuông, nhờ đó toán tử sử dụng đúng hai cơ chế này.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
class ArrayWrapper {
    private nums: number[];
    private s: number;

    constructor(nums: number[]) {
        this.nums = nums;
        this.s = nums.reduce((a, b) => a + b, 0);
    }

    valueOf() {
        return this.s;
    }

    toString() {
        return `[${this.nums}]`;
    }
}

/**
 * const obj1 = new ArrayWrapper([1,2]);
 * const obj2 = new ArrayWrapper([3,4]);
 * obj1 + obj2; // 10
 * String(obj1); // "[1,2]"
 * String(obj2); // "[3,4]"
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
