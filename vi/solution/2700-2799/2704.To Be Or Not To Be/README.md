---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2704. To Be Or Not To Be](https://leetcode.com/problems/to-be-or-not-to-be)

[中文文档](/solution/2700-2799/2704.To%20Be%20Or%20Not%20To%20Be/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một hàm&nbsp;<code>expect</code> giúp developer kiểm thử code của họ. Hàm nhận một giá trị bất kỳ&nbsp;<code>val</code>&nbsp;và trả về một object chứa hai hàm sau.</p>

<ul>
	<li><code>toBe(val)</code>&nbsp;nhận một giá trị khác và trả về&nbsp;<code>true</code>&nbsp;nếu hai giá trị bằng nhau theo toán tử&nbsp;<code>===</code>. Nếu chúng không bằng nhau, hàm phải ném lỗi&nbsp;<code>&quot;Not Equal&quot;</code>.</li>
	<li><code>notToBe(val)</code>&nbsp;nhận một giá trị khác và trả về&nbsp;<code>true</code>&nbsp;nếu hai giá trị khác nhau theo toán tử&nbsp;<code>!==</code>. Nếu chúng bằng nhau, hàm phải ném lỗi&nbsp;<code>&quot;Equal&quot;</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> func = () =&gt; expect(5).toBe(5)
<strong>Đầu ra:</strong> {&quot;value&quot;: true}
<strong>Giải thích:</strong> 5 === 5 nên biểu thức này trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> func = () =&gt; expect(5).toBe(null)
<strong>Đầu ra:</strong> {&quot;error&quot;: &quot;Not Equal&quot;}
<strong>Giải thích:</strong> 5 !== null nên biểu thức này ném lỗi &quot;Not Equal&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> func = () =&gt; expect(5).notToBe(null)
<strong>Đầu ra:</strong> {&quot;value&quot;: true}
<strong>Giải thích:</strong> 5 !== null nên biểu thức này trả về true.
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Object assertion phải ném đúng các lỗi được quy định trên hai nhánh bằng và không bằng, đồng thời trả về $true$ khi thành công. Nếu tách thành hai helper độc lập, ta sẽ phải lặp lại phép so sánh.
>
> Một closure lưu giá trị cần kiểm tra $val$ và trả về $\{toBe, notToBe\}$: hàm đầu tiên ném lỗi Not Equal khi $!==$, còn hàm thứ hai ném lỗi Equal khi $===$. Mọi state cần thiết cho việc chaining đều nằm trong closure đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
type ToBeOrNotToBe = {
    toBe: (val: any) => boolean;
    notToBe: (val: any) => boolean;
};

function expect(val: any): ToBeOrNotToBe {
    return {
        toBe: (toBeVal: any) => {
            if (val !== toBeVal) {
                throw new Error('Not Equal');
            }
            return true;
        },
        notToBe: (notToBeVal: any) => {
            if (val === notToBeVal) {
                throw new Error('Equal');
            }
            return true;
        },
    };
}

/**
 * expect(5).toBe(5); // true
 * expect(5).notToBe(5); // throws "Equal"
 */
```

#### JavaScript

```js
/**
 * @param {string} val
 * @return {Object}
 */
var expect = function (val) {
    return {
        toBe: function (expected) {
            if (val !== expected) {
                throw new Error('Not Equal');
            }
            return true;
        },
        notToBe: function (expected) {
            if (val === expected) {
                throw new Error('Equal');
            }
            return true;
        },
    };
};

/**
 * expect(5).toBe(5); // true
 * expect(5).notToBe(5); // throws "Equal"
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
