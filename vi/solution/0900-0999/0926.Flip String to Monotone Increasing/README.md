---
comments: true
difficulty: Medium
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [926. Flip String to Monotone Increasing](https://leetcode.com/problems/flip-string-to-monotone-increasing)

[中文文档](/solution/0900-0999/0926.Flip%20String%20to%20Monotone%20Increasing/README.md)

## Mô tả

<!-- description:start -->

<p>Chuỗi nhị phân được gọi là đơn điệu tăng nếu gồm một số chữ số <code>0</code> (có thể không có), theo sau là một số chữ số <code>1</code> (cũng có thể không có).</p>

<p>Cho chuỗi nhị phân <code>s</code>. Bạn có thể đảo <code>s[i]</code>, đổi nó từ <code>0</code> thành <code>1</code> hoặc từ <code>1</code> thành <code>0</code>.</p>

<p>Hãy trả về <em>số lần đảo ít nhất để biến </em><code>s</code><em> thành chuỗi đơn điệu tăng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;00110&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Đảo chữ số cuối cùng để được 00111.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;010110&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Đảo các chữ số để được 011111 hoặc 000111.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;00011000&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Đảo các chữ số để được 00000000.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi đơn điệu gồm các số 0 đứng trước các số 1, nên tồn tại một điểm chia sao cho bên trái toàn số 0 và bên phải toàn số 1. Vì $n\le 10^5$, quét lại toàn chuỗi ở mỗi điểm chia sẽ quá chậm. Gọi $\textit{tot}$ là tổng số 0. Tại điểm chia $i$, nếu tiền tố có $\textit{cur}$ số 0 thì số lần đảo là $i-\textit{cur}+\textit{tot}-\textit{cur}$; lấy giá trị nhỏ nhất.

<!-- thinking:end -->

Trước tiên, đếm số ký tự '0' trong chuỗi $s$, gọi là $tot$. Đặt biến đáp án $ans = tot$ ban đầu; đây là số lần đảo cần thiết để đổi tất cả '0' thành '1'.

Sau đó, ta xét từng vị trí $i$, đổi tất cả '1' ở bên trái vị trí $i$ (kể cả $i$) thành '0', đồng thời đổi tất cả '0' ở bên phải $i$ thành '1'. Số lần đảo trong trường hợp này là $i + 1 - cur + tot - cur$, trong đó $cur$ là số '0' ở bên trái vị trí $i$ (kể cả $i$). Cập nhật đáp án: $ans = \min(ans, i + 1 - cur + tot - cur)$.

Cuối cùng, trả về đáp án $ans$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minFlipsMonoIncr(self, s: str) -> int:
        tot = s.count("0")
        ans, cur = tot, 0
        for i, c in enumerate(s, 1):
            cur += int(c == "0")
            ans = min(ans, i - cur + tot - cur)
        return ans
```

#### Java

```java
class Solution {
    public int minFlipsMonoIncr(String s) {
        int n = s.length();
        int tot = 0;
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) == '0') {
                ++tot;
            }
        }
        int ans = tot, cur = 0;
        for (int i = 1; i <= n; ++i) {
            if (s.charAt(i - 1) == '0') {
                ++cur;
            }
            ans = Math.min(ans, i - cur + tot - cur);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minFlipsMonoIncr(string s) {
        int tot = count(s.begin(), s.end(), '0');
        int ans = tot, cur = 0;
        for (int i = 1; i <= s.size(); ++i) {
            cur += s[i - 1] == '0';
            ans = min(ans, i - cur + tot - cur);
        }
        return ans;
    }
};
```

#### Go

```go
func minFlipsMonoIncr(s string) int {
	tot := strings.Count(s, "0")
	ans, cur := tot, 0
	for i, c := range s {
		if c == '0' {
			cur++
		}
		ans = min(ans, i+1-cur+tot-cur)
	}
	return ans
}
```

#### TypeScript

```ts
function minFlipsMonoIncr(s: string): number {
    let tot = 0;
    for (const c of s) {
        tot += c === '0' ? 1 : 0;
    }
    let [ans, cur] = [tot, 0];
    for (let i = 1; i <= s.length; ++i) {
        cur += s[i - 1] === '0' ? 1 : 0;
        ans = Math.min(ans, i - cur + tot - cur);
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {number}
 */
var minFlipsMonoIncr = function (s) {
    let tot = 0;
    for (const c of s) {
        tot += c === '0' ? 1 : 0;
    }
    let [ans, cur] = [tot, 0];
    for (let i = 1; i <= s.length; ++i) {
        cur += s[i - 1] === '0' ? 1 : 0;
        ans = Math.min(ans, i - cur + tot - cur);
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
