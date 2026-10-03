---
comments: true
difficulty: Easy
rating: 1308
source: Weekly Contest 292 Q1
tags:
    - String
---

<!-- problem:start -->

# [2264. Largest 3-Same-Digit Number in String](https://leetcode.com/problems/largest-3-same-digit-number-in-string)

[中文文档](/solution/2200-2299/2264.Largest%203-Same-Digit%20Number%20in%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>num</code> biểu diễn một số nguyên lớn. Một số nguyên được gọi là <strong>good</strong> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Đó là một <strong>chuỗi con</strong> có độ dài <code>3</code> của <code>num</code>.</li>
	<li>Nó chỉ gồm duy nhất một chữ số.</li>
</ul>

<p>Hãy trả về <em>số nguyên <strong>good</strong> lớn nhất dưới dạng <strong>chuỗi</strong>, hoặc một chuỗi rỗng </em><code>&quot;&quot;</code><em> nếu không tồn tại số nguyên nào như vậy</em>.</p>

<p>Lưu ý:</p>

<ul>
	<li><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</li>
	<li><code>num</code> hoặc một số nguyên <strong>good</strong> có thể có các số 0 ở đầu.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;6<strong><u>777</u></strong>133339&quot;
<strong>Đầu ra:</strong> &quot;777&quot;
<strong>Giải thích:</strong> Có hai số nguyên good khác nhau: &quot;777&quot; và &quot;333&quot;.
&quot;777&quot; là số lớn hơn, nên ta trả về &quot;777&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;23<strong><u>000</u></strong>19&quot;
<strong>Đầu ra:</strong> &quot;000&quot;
<strong>Giải thích:</strong> &quot;000&quot; là số nguyên good duy nhất.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;42352338&quot;
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong> Không có chuỗi con nào có độ dài 3 chỉ gồm duy nhất một chữ số. Vì vậy, không tồn tại số nguyên good nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= num.length &lt;= 1000</code></li>
	<li><code>num</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mục tiêu là tìm chuỗi con gồm ba chữ số giống nhau lớn nhất của $num$. Vì độ dài là $10^3$, ta kiểm tra lần lượt các giá trị $999,\ldots,000$ từ lớn đến nhỏ; khi tìm thấy một giá trị xuất hiện, đó chính là đáp án lớn nhất.
>
> Với $i$ từ $9$ giảm xuống $0$, kiểm tra xem $\texttt{str}(i)*3$ có phải là chuỗi con hay không; nếu không tìm thấy giá trị nào thì trả về chuỗi rỗng.

<!-- thinking:end -->

Ta có thể liệt kê từng chữ số $i$ từ lớn đến nhỏ, với $0 \le i \le 9$, rồi kiểm tra xem chuỗi $s$ gồm ba chữ số $i$ liên tiếp có phải là chuỗi con của $num$ hay không. Nếu có, ta trả về ngay $s$.

Nếu đã liệt kê tất cả các giá trị có thể có của $i$ mà vẫn không tìm thấy chuỗi con nào thỏa mãn điều kiện, ta trả về một chuỗi rỗng.

Độ phức tạp thời gian là $O(10 \times n)$, trong đó $n$ là độ dài của chuỗi $num$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestGoodInteger(self, num: str) -> str:
        for i in range(9, -1, -1):
            if (s := str(i) * 3) in num:
                return s
        return ""
```

#### Java

```java
class Solution {
    public String largestGoodInteger(String num) {
        for (int i = 9; i >= 0; i--) {
            String s = String.valueOf(i).repeat(3);
            if (num.contains(s)) {
                return s;
            }
        }
        return "";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string largestGoodInteger(string num) {
        for (char i = '9'; i >= '0'; --i) {
            string s(3, i);
            if (num.find(s) != string::npos) {
                return s;
            }
        }
        return "";
    }
};
```

#### Go

```go
func largestGoodInteger(num string) string {
	for c := '9'; c >= '0'; c-- {
		if s := strings.Repeat(string(c), 3); strings.Contains(num, s) {
			return s
		}
	}
	return ""
}
```

#### TypeScript

```ts
function largestGoodInteger(num: string): string {
    for (let i = 9; i >= 0; i--) {
        const s = String(i).repeat(3);
        if (num.includes(s)) {
            return s;
        }
    }
    return '';
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
