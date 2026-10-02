---
comments: true
difficulty: Easy
rating: 1165
source: Biweekly Contest 26 Q1
tags:
    - String
---

<!-- problem:start -->

# [1446. Consecutive Characters](https://leetcode.com/problems/consecutive-characters)

[中文文档](/solution/1400-1499/1446.Consecutive%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Power</strong> của chuỗi là độ dài lớn nhất của một chuỗi con không rỗng chỉ chứa một ký tự duy nhất.</p>

<p>Cho một chuỗi <code>s</code>, hãy trả về <em><strong>power</strong> của</em> <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;leetcode&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Chuỗi con &quot;ee&quot; có độ dài 2 và chỉ chứa ký tự &#39;e&#39;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abbcccddddeeeeedcba&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Chuỗi con &quot;eeeee&quot; có độ dài 5 và chỉ chứa ký tự &#39;e&#39;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 500</code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt và đếm

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 500$. Duyệt một lần, tăng $t$ khi các ký tự liền kề giống nhau, đặt lại khi có thay đổi và lưu giá trị lớn nhất.

<!-- thinking:end -->

Ta định nghĩa biến $\textit{t}$ biểu diễn độ dài của dãy ký tự liên tiếp hiện tại, ban đầu $\textit{t}=1$.

Tiếp theo, ta duyệt chuỗi $s$ bắt đầu từ ký tự thứ hai. Nếu ký tự hiện tại giống ký tự trước đó, thì $\textit{t} = \textit{t} + 1$, đồng thời cập nhật đáp án $\textit{ans} = \max(\textit{ans}, \textit{t})$; ngược lại, đặt $\textit{t} = 1$.

Cuối cùng, trả về đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxPower(self, s: str) -> int:
        ans = t = 1
        for a, b in pairwise(s):
            if a == b:
                t += 1
                ans = max(ans, t)
            else:
                t = 1
        return ans
```

#### Java

```java
class Solution {
    public int maxPower(String s) {
        int ans = 1, t = 1;
        for (int i = 1; i < s.length(); ++i) {
            if (s.charAt(i) == s.charAt(i - 1)) {
                ans = Math.max(ans, ++t);
            } else {
                t = 1;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxPower(string s) {
        int ans = 1, t = 1;
        for (int i = 1; i < s.size(); ++i) {
            if (s[i] == s[i - 1]) {
                ans = max(ans, ++t);
            } else {
                t = 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxPower(s string) int {
	ans, t := 1, 1
	for i := 1; i < len(s); i++ {
		if s[i] == s[i-1] {
			t++
			ans = max(ans, t)
		} else {
			t = 1
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxPower(s: string): number {
    let ans = 1;
    let t = 1;
    for (let i = 1; i < s.length; ++i) {
        if (s[i] === s[i - 1]) {
            ans = Math.max(ans, ++t);
        } else {
            t = 1;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
