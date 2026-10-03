---
comments: true
difficulty: Medium
rating: 1490
source: Weekly Contest 228 Q2
tags:
    - Math
    - String
---

<!-- problem:start -->

# [1759. Count Number of Homogenous Substrings](https://leetcode.com/problems/count-number-of-homogenous-substrings)

[中文文档](/solution/1700-1799/1759.Count%20Number%20of%20Homogenous%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, trả về <em>số chuỗi con <strong>đồng nhất</strong> của </em><code>s</code><em>.</em> Vì đáp án có thể rất lớn, hãy trả về <strong>phần dư</strong> khi chia cho <code>10<sup>9</sup> + 7</code>.</p>

<p>Chuỗi được gọi là <strong>đồng nhất</strong> nếu mọi ký tự trong chuỗi đều giống nhau.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abbcccaa&quot;
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Các chuỗi con đồng nhất là:
&quot;a&quot;   appears 3 times.
&quot;aa&quot;  appears 1 time.
&quot;b&quot;   appears 2 times.
&quot;bb&quot;  appears 1 time.
&quot;c&quot;   appears 3 times.
&quot;cc&quot;  appears 2 times.
&quot;ccc&quot; appears 1 time.
3 + 1 + 2 + 1 + 3 + 2 + 1 = 13.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;xy&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các chuỗi con đồng nhất là &quot;x&quot; và &quot;y&quot;.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;zzzzz&quot;
<strong>Đầu ra:</strong> 15
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> consists of lowercase letters.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi con đồng nhất nằm trong một đoạn liên tiếp gồm các ký tự giống nhau. Một đoạn có độ dài $cnt$ đóng góp $cnt(cnt+1)/2$. Vì $n\le 10^5$, ta duyệt theo từng đoạn.
>
> Hai con trỏ xác định hai đầu mỗi đoạn, cộng số tam giác tương ứng rồi lấy phần dư modulo $10^9+7$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countHomogenous(self, s: str) -> int:
        mod = 10**9 + 7
        i, n = 0, len(s)
        ans = 0
        while i < n:
            j = i
            while j < n and s[j] == s[i]:
                j += 1
            cnt = j - i
            ans += (1 + cnt) * cnt // 2
            ans %= mod
            i = j
        return ans
```

#### Java

```java
class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int countHomogenous(String s) {
        int n = s.length();
        long ans = 0;
        for (int i = 0, j = 0; i < n; i = j) {
            j = i;
            while (j < n && s.charAt(j) == s.charAt(i)) {
                ++j;
            }
            int cnt = j - i;
            ans += (long) (1 + cnt) * cnt / 2;
            ans %= MOD;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    const int mod = 1e9 + 7;

    int countHomogenous(string s) {
        int n = s.size();
        long ans = 0;
        for (int i = 0, j = 0; i < n; i = j) {
            j = i;
            while (j < n && s[j] == s[i]) ++j;
            int cnt = j - i;
            ans += 1ll * (1 + cnt) * cnt / 2;
            ans %= mod;
        }
        return ans;
    }
};
```

#### Go

```go
func countHomogenous(s string) (ans int) {
	n := len(s)
	const mod int = 1e9 + 7
	for i, j := 0, 0; i < n; i = j {
		j = i
		for j < n && s[j] == s[i] {
			j++
		}
		cnt := j - i
		ans += (1 + cnt) * cnt / 2
		ans %= mod
	}
	return
}
```

#### TypeScript

```ts
function countHomogenous(s: string): number {
    const mod = 1e9 + 7;
    const n = s.length;
    let ans = 0;
    for (let i = 0, j = 0; j < n; j++) {
        if (s[i] !== s[j]) {
            i = j;
        }
        ans = (ans + j - i + 1) % mod;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_homogenous(s: String) -> i32 {
        const MOD: usize = (1e9 as usize) + 7;
        let s = s.as_bytes();
        let n = s.len();
        let mut ans = 0;
        let mut i = 0;
        for j in 0..n {
            if s[i] != s[j] {
                i = j;
            }
            ans = (ans + j - i + 1) % MOD;
        }
        ans as i32
    }
}
```

#### C#

```cs
public class Solution {
    public int CountHomogenous(string s) {
        long MOD = 1000000007;
        long ans = 0;
        for (int i = 0, j = 0; i < s.Length; i = j) {
            j = i;
            while (j < s.Length && s[j] == s[i]) {
                ++j;
            }
            int cnt = j - i;
            ans += (long) (1 + cnt) * cnt / 2;
            ans %= MOD;
        }
        return (int) ans;
    }
}
```

#### C

```c
int countHomogenous(char* s) {
    int MOD = 1e9 + 7;
    int ans = 0;
    for (int i = 0, j = 0; s[j]; j++) {
        if (s[i] != s[j]) {
            i = j;
        }
        ans = (ans + j - i + 1) % MOD;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 cộng cả đoạn trong một lần. Khi duyệt tuyến tính, ta có thể tăng độ dài đoạn khi hai ký tự kề nhau giống nhau, ngược lại đặt lại thành $1$, rồi cộng độ dài hiện tại ở mỗi bước.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countHomogenous(self, s: str) -> int:
        mod = 10**9 + 7
        ans = cnt = 1
        for a, b in pairwise(s):
            cnt = cnt + 1 if a == b else 1
            ans = (ans + cnt) % mod
        return ans
```

#### Java

```java
class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int countHomogenous(String s) {
        int n = s.length();
        int ans = 1, cnt = 1;
        for (int i = 1; i < n; ++i) {
            cnt = s.charAt(i) == s.charAt(i - 1) ? cnt + 1 : 1;
            ans = (ans + cnt) % MOD;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    const int mod = 1e9 + 7;

    int countHomogenous(string s) {
        int n = s.size();
        int ans = 1, cnt = 1;
        for (int i = 1; i < n; ++i) {
            cnt = s[i] == s[i - 1] ? cnt + 1 : 1;
            ans = (ans + cnt) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func countHomogenous(s string) int {
	n := len(s)
	const mod int = 1e9 + 7
	ans, cnt := 1, 1
	for i := 1; i < n; i++ {
		if s[i] == s[i-1] {
			cnt++
		} else {
			cnt = 1
		}
		ans = (ans + cnt) % mod
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
