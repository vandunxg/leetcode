---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2726. Calculator with Method Chaining](https://leetcode.com/problems/calculator-with-method-chaining)

[中文文档](/solution/2700-2799/2726.Calculator%20with%20Method%20Chaining/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một class <code>Calculator</code>. Class này cần cung cấp các phép toán cộng, trừ, nhân, chia và lũy thừa. Ngoài ra, class cũng cho phép thực hiện các phép toán liên tiếp bằng method chaining. Constructor của class <code>Calculator</code> nhận một số, được dùng làm giá trị ban đầu của <code>result</code>.</p>

<p>Class <font face="monospace"><code>Calculator</code>&nbsp;</font>của bạn cần có các method sau:</p>

<ul>
	<li><code>add</code> - Method này cộng số <code>value</code> đã cho vào <code>result</code> và trả về <code>Calculator</code> đã được cập nhật.</li>
	<li><code>subtract</code> -&nbsp;Method này trừ số <code>value</code>&nbsp;đã cho khỏi <code>result</code>&nbsp;và trả về <code>Calculator</code> đã được cập nhật.</li>
	<li><code>multiply</code> -&nbsp;Method này nhân <code>result</code>&nbsp; với số <code>value</code>&nbsp;đã cho và trả về <code>Calculator</code> đã được cập nhật.</li>
	<li><code>divide</code> -&nbsp;Method này chia <code>result</code> cho số <code>value</code>&nbsp;đã cho và trả về <code>Calculator</code> đã được cập nhật. Nếu giá trị được truyền vào là <code>0</code>, cần ném lỗi <code>&quot;Division by zero is not allowed&quot;</code>.</li>
	<li><code>power</code> -&nbsp;Method này nâng&nbsp;<code>result</code> lên lũy thừa là số <code>value</code>&nbsp;đã cho và trả về <code>Calculator</code> đã được cập nhật.</li>
	<li><code>getResult</code> -&nbsp;Method này trả về <code>result</code>.</li>
</ul>

<p>Kết quả có sai lệch không quá&nbsp;<code>10<sup>-5</sup></code>&nbsp;so với kết quả thực được xem là đúng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
actions = [&quot;Calculator&quot;, &quot;add&quot;, &quot;subtract&quot;, &quot;getResult&quot;],
values = [10, 5, 7]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong>
new Calculator(10).add(5).subtract(7).getResult() // 10 + 5 - 7 = 8
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong>
actions = [&quot;Calculator&quot;, &quot;multiply&quot;, &quot;power&quot;, &quot;getResult&quot;],
values = [2, 5, 2]
<strong>Đầu ra:</strong> 100
<strong>Giải thích:</strong>
new Calculator(2).multiply(5).power(2).getResult() // (2 * 5) ^ 2 = 100
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong>
actions = [&quot;Calculator&quot;, &quot;divide&quot;, &quot;getResult&quot;],
values = [20, 0]
<strong>Đầu ra:</strong> &quot;Division by zero is not allowed&quot;
<strong>Giải thích:</strong>
new Calculator(20).divide(0).getResult() // 20 / 0

Lỗi phải được ném ra vì không thể chia cho 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>actions</code> là một mảng JSON hợp lệ gồm các chuỗi</li>
	<li><code>values</code>&nbsp;là một mảng JSON hợp lệ gồm các số</li>
	<li><code>2 &lt;= actions.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= values.length &lt;= 2 * 10<sup>4</sup>&nbsp;- 1</code></li>
	<li><code>actions[i]</code> là một trong các giá trị &quot;Calculator&quot;, &quot;add&quot;, &quot;subtract&quot;, &quot;multiply&quot;, &quot;divide&quot;, &quot;power&quot; và&nbsp;&quot;getResult&quot;</li>
	<li>Action đầu tiên luôn là &quot;Calculator&quot;</li>
	<li>Action cuối cùng luôn là &quot;getResult&quot;</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Calculator phải hỗ trợ chaining các phép add, subtract, multiply, divide và power trên cùng một instance, đồng thời cung cấp result; phép chia cho 0 phải ném một lỗi cố định. Mỗi lần trả về một object mới vẫn cho phép chaining, nhưng sẽ không duy trì một state duy nhất.
>
> Lưu giá trị hiện tại vào $x$, cập nhật nó trong từng method rồi trả về $this$. $divide$ sẽ ném lỗi khi số chia là $0$. $getResult$ đọc giá trị $x$.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
class Calculator {
    private x: number;

    constructor(value: number) {
        this.x = value;
    }

    add(value: number): Calculator {
        this.x += value;
        return this;
    }

    subtract(value: number): Calculator {
        this.x -= value;
        return this;
    }

    multiply(value: number): Calculator {
        this.x *= value;
        return this;
    }

    divide(value: number): Calculator {
        if (value === 0) {
            throw new Error('Division by zero is not allowed');
        }
        this.x /= value;
        return this;
    }

    power(value: number): Calculator {
        this.x **= value;
        return this;
    }

    getResult(): number {
        return this.x;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
