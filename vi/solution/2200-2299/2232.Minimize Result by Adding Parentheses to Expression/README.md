---
comments: true
difficulty: Medium
rating: 1611
source: Weekly Contest 288 Q2
tags:
    - String
    - Enumeration
---

<!-- problem:start -->

# [2232. Minimize Result by Adding Parentheses to Expression](https://leetcode.com/problems/minimize-result-by-adding-parentheses-to-expression)

[Tài liệu tiếng Trung](/solution/2200-2299/2232.Minimize%20Result%20by%20Adding%20Parentheses%20to%20Expression/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>expression</code> được đánh chỉ số từ <strong>0</strong>, có dạng <code>&quot;&lt;num1&gt;+&lt;num2&gt;&quot;</code>, trong đó <code>&lt;num1&gt;</code> và <code>&lt;num2&gt;</code> biểu diễn các số nguyên dương.</p>

<p>Hãy thêm một cặp dấu ngoặc đơn vào <code>expression</code> sao cho sau khi thêm, <code>expression</code> là một biểu thức toán học <strong>hợp lệ</strong> và có giá trị <strong>nhỏ nhất</strong>. Dấu ngoặc trái <strong>phải</strong> được thêm vào bên trái <code>&#39;+&#39;</code>, còn dấu ngoặc phải <strong>phải</strong> được thêm vào bên phải <code>&#39;+&#39;</code>.</p>

<p>Trả về <code>expression</code><em> sau khi thêm một cặp dấu ngoặc sao cho </em><code>expression</code><em> có giá trị <strong>nhỏ nhất</strong>.</em> Nếu có nhiều đáp án cho cùng một kết quả, hãy trả về bất kỳ đáp án nào.</p>

<p>Giá trị ban đầu của <code>expression</code> và giá trị của <code>expression</code> sau khi thêm bất kỳ cặp dấu ngoặc nào thỏa mãn yêu cầu đều nằm trong phạm vi của số nguyên 32-bit có dấu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;247+38&quot;
<strong>Đầu ra:</strong> &quot;2(47+38)&quot;
<strong>Giải thích:</strong> <code>expression</code> có giá trị là 2 * (47 + 38) = 2 * 85 = 170.
Lưu ý rằng &quot;2(4)7+38&quot; không hợp lệ vì dấu ngoặc phải phải nằm bên phải <code>&#39;+&#39;</code>.
Có thể chứng minh rằng 170 là giá trị nhỏ nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;12+34&quot;
<strong>Đầu ra:</strong> &quot;1(2+3)4&quot;
<strong>Giải thích:</strong> expression có giá trị là 1 * (2 + 3) * 4 = 1 * 5 * 4 = 20.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;999+999&quot;
<strong>Đầu ra:</strong> &quot;(999+999)&quot;
<strong>Giải thích:</strong> <code>expression</code> có giá trị là 999 + 999 = 1998.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= expression.length &lt;= 10</code></li>
	<li><code>expression</code> chỉ bao gồm các chữ số từ <code>&#39;1&#39;</code> đến <code>&#39;9&#39;</code> và <code>&#39;+&#39;</code>.</li>
	<li><code>expression</code> bắt đầu và kết thúc bằng một chữ số.</li>
	<li><code>expression</code> chứa đúng một <code>&#39;+&#39;</code>.</li>
	<li>Giá trị ban đầu của <code>expression</code> và giá trị của <code>expression</code> sau khi thêm bất kỳ cặp dấu ngoặc nào thỏa mãn yêu cầu đều nằm trong phạm vi của số nguyên 32-bit có dấu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Biểu thức có dạng $A{+}B$. Ta chèn một cặp dấu ngoặc để tối thiểu hóa $a(c)b$, trong đó $c$ là tổng bên trong. Chuỗi có độ dài không quá $10$, nên chỉ có $O(|A|\cdot|B|)$ cách đặt ngoặc.
>
> Dấu ngoặc trái nằm trước chỉ số $i$ của $A$, còn dấu ngoặc phải nằm sau chỉ số $j$ của $B$. Phần bên trong là $l[i:]+r[:j+1]$; nếu một phía rỗng thì phía đó đóng góp hệ số $1$. Ta giữ lại cách đặt có tích nhỏ nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimizeResult(self, expression: str) -> str:
        l, r = expression.split("+")
        m, n = len(l), len(r)
        mi = inf
        ans = None
        for i in range(m):
            for j in range(n):
                c = int(l[i:]) + int(r[: j + 1])
                a = 1 if i == 0 else int(l[:i])
                b = 1 if j == n - 1 else int(r[j + 1 :])
                if (t := a * b * c) < mi:
                    mi = t
                    ans = f"{l[:i]}({l[i:]}+{r[: j + 1]}){r[j + 1:]}"
        return ans
```

#### Java

```java
class Solution {
    public String minimizeResult(String expression) {
        int idx = expression.indexOf('+');
        String l = expression.substring(0, idx);
        String r = expression.substring(idx + 1);
        int m = l.length(), n = r.length();
        int mi = Integer.MAX_VALUE;
        String ans = "";
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int c = Integer.parseInt(l.substring(i)) + Integer.parseInt(r.substring(0, j + 1));
                int a = i == 0 ? 1 : Integer.parseInt(l.substring(0, i));
                int b = j == n - 1 ? 1 : Integer.parseInt(r.substring(j + 1));
                int t = a * b * c;
                if (t < mi) {
                    mi = t;
                    ans = String.format("%s(%s+%s)%s", l.substring(0, i), l.substring(i),
                        r.substring(0, j + 1), r.substring(j + 1));
                }
            }
        }
        return ans;
    }
}
```

#### TypeScript

```ts
function minimizeResult(expression: string): string {
    const [n1, n2] = expression.split('+');
    let minSum = Number.MAX_SAFE_INTEGER;
    let ans = '';
    let arr1 = [],
        arr2 = n1.split(''),
        arr3 = n2.split(''),
        arr4 = [];
    while (arr2.length) {
        ((arr3 = n2.split('')), (arr4 = []));
        while (arr3.length) {
            let cur = (getNum(arr2) + getNum(arr3)) * getNum(arr1) * getNum(arr4);
            if (cur < minSum) {
                minSum = cur;
                ans = `${arr1.join('')}(${arr2.join('')}+${arr3.join('')})${arr4.join('')}`;
            }
            arr4.unshift(arr3.pop());
        }
        arr1.push(arr2.shift());
    }
    return ans;
}

function getNum(arr: Array<string>): number {
    return arr.length ? Number(arr.join('')) : 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
