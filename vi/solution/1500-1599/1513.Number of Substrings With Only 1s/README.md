---
comments: true
difficulty: Medium
rating: 1351
source: Weekly Contest 197 Q2
tags:
    - Math
    - String
---

<!-- problem:start -->

# [1513. Number of Substrings With Only 1s](https://leetcode.com/problems/number-of-substrings-with-only-1s)

[中文文档](/solution/1500-1599/1513.Number%20of%20Substrings%20With%20Only%201s/README.md)

## Mô tả

<!-- description:start -->

<p>Với chuỗi nhị phân <code>s</code>, hãy trả về <em>số lượng chuỗi con mà mọi ký tự đều là</em> <code>1</code><em></em>. Vì đáp án có thể rất lớn, hãy trả về phần dư của đáp án khi chia cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;0110111&quot;
<strong>Output:</strong> 9
<strong>Explanation:</strong> Có tổng cộng 9 chuỗi con chỉ chứa các ký tự 1.
&quot;1&quot; -&gt; 5 times.
&quot;11&quot; -&gt; 3 times.
&quot;111&quot; -&gt; 1 time.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;101&quot;
<strong>Output:</strong> 2
<strong>Explanation:</strong> Chuỗi con &quot;1&quot; xuất hiện 2 lần trong s.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;111111&quot;
<strong>Output:</strong> 21
<strong>Explanation:</strong> Mỗi chuỗi con chỉ chứa các ký tự 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> is either <code>&#39;0&#39;</code> or <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt và đếm

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các chuỗi con chỉ chứa số 1. Vì $n\le 10^5$, ta không thể liệt kê tất cả $O(n^2)$ chuỗi con. Các đoạn số 1 được ngăn cách bởi số 0 là độc lập.
>
> Một đoạn có độ dài $k$ đóng góp $k(k+1)/2$ chuỗi con. Điều này tương đương với việc duy trì độ dài đoạn hiện tại $cur$: mỗi số $1$ mới tăng $cur$ và cộng $cur$ vào đáp án; số $0$ đặt lại giá trị này. Không cần tách đoạn một cách tường minh.

<!-- thinking:end -->

Ta duyệt chuỗi $s$, dùng biến $\textit{cur}$ để ghi nhận số lượng số 1 liên tiếp hiện tại và biến $\textit{ans}$ để ghi nhận đáp án. Khi duyệt đến ký tự $s[i]$, nếu $s[i] = 0$ thì đặt $\textit{cur}$ về 0; ngược lại, tăng $\textit{cur}$ lên 1, cộng $\textit{cur}$ vào $\textit{ans}$ rồi lấy modulo $10^9 + 7$.

Sau khi duyệt xong, trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

Các bài tương tự:

- [413. Arithmetic Slices](https://github.com/doocs/leetcode/blob/main/solution/0400-0499/0413.Arithmetic%20Slices/README_EN.md)
- [2348. Number of Zero-Filled Subarrays](https://github.com/doocs/leetcode/blob/main/solution/2300-2399/2348.Number%20of%20Zero-Filled%20Subarrays/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numSub(self, s: str) -> int:
        mod = 10**9 + 7
        ans = cur = 0
        for c in s:
            if c == "0":
                cur = 0
            else:
                cur += 1
                ans = (ans + cur) % mod
        return ans
```

#### Java

```java
class Solution {
    public int numSub(String s) {
        final int mod = 1_000_000_007;
        int ans = 0, cur = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) == '0') {
                cur = 0;
            } else {
                cur++;
                ans = (ans + cur) % mod;
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
    int numSub(string s) {
        const int mod = 1e9 + 7;
        int ans = 0, cur = 0;
        for (char c : s) {
            if (c == '0') {
                cur = 0;
            } else {
                cur++;
                ans = (ans + cur) % mod;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numSub(s string) (ans int) {
	const mod int = 1e9 + 7
	cur := 0
	for _, c := range s {
		if c == '0' {
			cur = 0
		} else {
			cur++
			ans = (ans + cur) % mod
		}
	}
	return
}
```

#### TypeScript

```ts
function numSub(s: string): number {
    const mod = 1_000_000_007;
    let [ans, cur] = [0, 0];
    for (const c of s) {
        if (c === '0') {
            cur = 0;
        } else {
            cur++;
            ans = (ans + cur) % mod;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn num_sub(s: String) -> i32 {
        const MOD: i32 = 1_000_000_007;
        let mut ans: i32 = 0;
        let mut cur: i32 = 0;
        for c in s.chars() {
            if c == '0' {
                cur = 0;
            } else {
                cur += 1;
                ans = (ans + cur) % MOD;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
