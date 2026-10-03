---
comments: true
difficulty: Easy
rating: 1248
source: Weekly Contest 246 Q1
tags:
    - Greedy
    - Math
    - String
---

<!-- problem:start -->

# [1903. Largest Odd Number in String](https://leetcode.com/problems/largest-odd-number-in-string)

[中文文档](/solution/1900-1999/1903.Largest%20Odd%20Number%20in%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>num</code>, biểu diễn một số nguyên lớn. Hãy trả về <em>số nguyên <strong>lẻ có giá trị lớn nhất</strong> (dưới dạng chuỗi) là một <strong>substring không rỗng</strong> của </em><code>num</code><em>, hoặc chuỗi rỗng </em><code>&quot;&quot;</code><em> nếu không tồn tại số nguyên lẻ nào</em>.</p>

<p><strong>Substring</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;52&quot;
<strong>Đầu ra:</strong> &quot;5&quot;
<strong>Giải thích:</strong> Các substring không rỗng duy nhất là &quot;5&quot;, &quot;2&quot; và &quot;52&quot;. &quot;5&quot; là số lẻ duy nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;4206&quot;
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong> Không có số lẻ nào trong &quot;4206&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;35427&quot;
<strong>Đầu ra:</strong> &quot;35427&quot;
<strong>Giải thích:</strong> &quot;35427&quot; vốn đã là một số lẻ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 10<sup>5</sup></code></li>
	<li><code>num</code> chỉ gồm các chữ số và không chứa số 0 ở đầu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt ngược

<!-- thinking:start -->

> **Tư duy**
>
> Số lẻ lớn nhất dưới dạng substring phải là một tiền tố của $num$ kết thúc bằng chữ số lẻ. Không cần chuyển từng tiền tố thành số nguyên, và $n\le 10^5$ khiến việc tính toán số lớn trở nên không phù hợp.
>
> Duyệt từ phải sang trái sẽ tìm được chữ số lẻ đầu tiên; tiền tố kết thúc tại đó vừa là số lẻ vừa dài nhất, nên cũng là số lớn nhất.
>
> Nếu không có chữ số lẻ nào, đáp án là chuỗi rỗng. Chỉ cần một lượt duyệt và không gian phụ hằng số.

<!-- thinking:end -->

Ta có thể duyệt chuỗi từ cuối về đầu, tìm số lẻ đầu tiên rồi trả về substring từ đầu chuỗi đến số lẻ đó. Nếu không có số lẻ, trả về chuỗi rỗng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $num$. Không tính phần không gian dùng cho chuỗi kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestOddNumber(self, num: str) -> str:
        for i in range(len(num) - 1, -1, -1):
            if (int(num[i]) & 1) == 1:
                return num[: i + 1]
        return ''
```

#### Java

```java
class Solution {
    public String largestOddNumber(String num) {
        for (int i = num.length() - 1; i >= 0; --i) {
            int c = num.charAt(i) - '0';
            if ((c & 1) == 1) {
                return num.substring(0, i + 1);
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
    string largestOddNumber(string num) {
        for (int i = num.size() - 1; i >= 0; --i) {
            int c = num[i] - '0';
            if ((c & 1) == 1) {
                return num.substr(0, i + 1);
            }
        }
        return "";
    }
};
```

#### Go

```go
func largestOddNumber(num string) string {
	for i := len(num) - 1; i >= 0; i-- {
		c := num[i] - '0'
		if (c & 1) == 1 {
			return num[:i+1]
		}
	}
	return ""
}
```

#### TypeScript

```ts
function largestOddNumber(num: string): string {
    for (let i = num.length - 1; ~i; --i) {
        if (Number(num[i]) & 1) {
            return num.slice(0, i + 1);
        }
    }
    return '';
}
```

#### JavaScript

```js
/**
 * @param {string} num
 * @return {string}
 */
var largestOddNumber = function (num) {
    for (let i = num.length - 1; ~i; --i) {
        if (Number(num[i]) & 1) {
            return num.slice(0, i + 1);
        }
    }
    return '';
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
