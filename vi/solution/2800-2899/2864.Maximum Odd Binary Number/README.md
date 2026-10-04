---
comments: true
difficulty: Easy
rating: 1237
source: Weekly Contest 364 Q1
tags:
    - Greedy
    - Math
    - String
---

<!-- problem:start -->

# [2864. Maximum Odd Binary Number](https://leetcode.com/problems/maximum-odd-binary-number)

[中文文档](/solution/2800-2899/2864.Maximum%20Odd%20Binary%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <strong>nhị phân</strong> <code>s</code> chứa ít nhất một ký tự <code>&#39;1&#39;</code>.</p>

<p>Bạn phải <strong>sắp xếp lại</strong> các bit sao cho số nhị phân tạo thành là <strong>số nhị phân lẻ lớn nhất</strong> có thể tạo ra từ tổ hợp này.</p>

<p>Hãy trả về <em>một chuỗi biểu diễn số nhị phân lẻ lớn nhất có thể tạo ra từ tổ hợp đã cho.</em></p>

<p><strong>Lưu ý </strong>rằng chuỗi kết quả <strong>có thể</strong> chứa các số 0 ở đầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;010&quot;
<strong>Đầu ra:</strong> &quot;001&quot;
<strong>Giải thích:</strong> Vì chỉ có một ký tự &#39;1&#39;, nó phải nằm ở vị trí cuối cùng. Do đó, đáp án là &quot;001&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0101&quot;
<strong>Đầu ra:</strong> &quot;1001&quot;
<strong>Giải thích: </strong>Một trong các ký tự &#39;1&#39; phải nằm ở vị trí cuối cùng. Số lớn nhất có thể tạo ra từ các chữ số còn lại là &quot;100&quot;. Do đó, đáp án là &quot;1001&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>.</li>
	<li><code>s</code> chứa ít nhất một ký tự <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Một số nhị phân lẻ phải kết thúc bằng $1$, còn các bit 1 còn lại nên nằm ở những bit cao nhất. Sau khi đếm số lượng bit 1, ta xuất $cnt-1$ bit 1 ở đầu, tiếp theo là các số 0, rồi thêm một bit $1$ cuối cùng.

<!-- thinking:end -->

Trước hết, ta đếm số lượng ký tự '1' trong chuỗi $s$, ký hiệu là $cnt$. Sau đó, ta đặt $cnt - 1$ ký tự '1' ở các vị trí cao nhất, tiếp theo là $|s| - cnt$ ký tự '0' còn lại, và cuối cùng thêm một ký tự '1'.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumOddBinaryNumber(self, s: str) -> str:
        cnt = s.count("1")
        return "1" * (cnt - 1) + (len(s) - cnt) * "0" + "1"
```

#### Java

```java
class Solution {
    public String maximumOddBinaryNumber(String s) {
        int cnt = s.length() - s.replace("1", "").length();
        return "1".repeat(cnt - 1) + "0".repeat(s.length() - cnt) + "1";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string maximumOddBinaryNumber(string s) {
        int cnt = count(s.begin(), s.end(), '1');
        return string(cnt - 1, '1') + string(s.size() - cnt, '0') + '1';
    }
};
```

#### Go

```go
func maximumOddBinaryNumber(s string) string {
	cnt := strings.Count(s, "1")
	return strings.Repeat("1", cnt-1) + strings.Repeat("0", len(s)-cnt) + "1"
}
```

#### TypeScript

```ts
function maximumOddBinaryNumber(s: string): string {
    const cnt = s.length - s.replace(/1/g, '').length;
    return '1'.repeat(cnt - 1) + '0'.repeat(s.length - cnt) + '1';
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_odd_binary_number(s: String) -> String {
        let cnt = s.chars().filter(|&c| c == '1').count();
        "1".repeat(cnt - 1) + &"0".repeat(s.len() - cnt) + "1"
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
