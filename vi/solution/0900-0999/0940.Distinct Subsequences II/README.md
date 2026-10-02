---
comments: true
difficulty: Hard
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [940. Distinct Subsequences II](https://leetcode.com/problems/distinct-subsequences-ii)

[中文文档](/solution/0900-0999/0940.Distinct%20Subsequences%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi s, hãy trả về <em>số lượng <strong>dãy con không rỗng khác nhau</strong> của</em> <code>s</code>. Vì đáp án có thể rất lớn, hãy trả về <strong>phần dư khi chia cho</strong> <code>10<sup>9</sup> + 7</code>.</p>
<strong>Dãy con</strong> của một chuỗi là chuỗi mới được tạo từ chuỗi ban đầu bằng cách xóa một số ký tự (có thể không xóa ký tự nào) mà không làm thay đổi thứ tự tương đối của các ký tự còn lại. (Ví dụ, <code>&quot;ace&quot;</code> là dãy con của <code>&quot;<u>a</u>b<u>c</u>d<u>e</u>&quot;</code>, còn <code>&quot;aec&quot;</code> thì không.)
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abc&quot;
<strong>Output:</strong> 7
<strong>Giải thích:</strong> 7 dãy con khác nhau là &quot;a&quot;, &quot;b&quot;, &quot;c&quot;, &quot;ab&quot;, &quot;ac&quot;, &quot;bc&quot; và &quot;abc&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aba&quot;
<strong>Output:</strong> 6
<strong>Giải thích:</strong> 6 dãy con khác nhau là &quot;a&quot;, &quot;b&quot;, &quot;ab&quot;, &quot;aa&quot;, &quot;ba&quot; và &quot;aba&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aaa&quot;
<strong>Output:</strong> 3
<strong>Giải thích:</strong> 3 dãy con khác nhau là &quot;a&quot;, &quot;aa&quot; và &quot;aaa&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 2000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần đếm các dãy con không rỗng khác nhau. Vì $n\le 2000$, không thể liệt kê mọi tập con. Phân loại theo ký tự cuối: $f[c]$ là số dãy con khác nhau hiện kết thúc bằng $c$. Khi đọc được $c$, ký tự này có thể nối tiếp bất kỳ dãy con trước đó hoặc đứng riêng, nên $f[c]\leftarrow \sum f+1$; nếu $c$ đã xuất hiện, nhóm dãy con cũ kết thúc bằng $c$ sẽ được thay thế.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là số dãy con khác nhau kết thúc bằng chữ cái thường thứ $i$. Ban đầu, mọi phần tử trong $f$ đều bằng $0$.

Duyệt chuỗi $s$. Với ký tự hiện tại $c$, cập nhật $f[c]$ thành $\sum_{i=0}^{25} f[i] + 1$. Trong đó, $\sum_{i=0}^{25} f[i]$ là số dãy con khác nhau đã tạo được, còn $+1$ tính thêm trường hợp chỉ riêng ký tự $c$ tạo thành một dãy con.

Cuối cùng, đáp án là $\sum_{i=0}^{25} f[i]$ modulo $10^9 + 7$.

Độ phức tạp thời gian là $O(n \times C)$ và độ phức tạp không gian là $O(C)$, trong đó $n$ là độ dài của $s$, còn $C$ là kích thước bộ ký tự. Trong bài này, $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distinctSubseqII(self, s: str) -> int:
        mod = 10**9 + 7
        f = [0] * 26
        for c in s:
            f[ord(c) - ord("a")] = (sum(f) + 1) % mod
        return sum(f) % mod
```

#### Java

```java
class Solution {
    public int distinctSubseqII(String s) {
        final int mod = (int) 1e9 + 7;
        int[] f = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            int x = 1;
            for (int v : f) {
                x = (x + v) % mod;
            }
            f[s.charAt(i) - 'a'] = x;
        }
        int ans = 0;
        for (int v : f) {
            ans = (ans + v) % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int distinctSubseqII(string s) {
        const int mod = 1e9 + 7;
        int f[26]{};
        for (char& c : s) {
            int x = 1;
            for (int v : f) {
                x = (x + v) % mod;
            }
            f[c - 'a'] = x;
        }
        int ans = 0;
        for (int v : f) {
            ans = (ans + v) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func distinctSubseqII(s string) int {
	const mod int = 1e9 + 7
	f := [26]int{}
	for _, c := range s {
		x := 1
		for _, v := range f {
			x = (x + v) % mod
		}
		f[c-'a'] = x
	}
	ans := 0
	for _, v := range f {
		ans = (ans + v) % mod
	}
	return ans
}
```

#### TypeScript

```ts
function distinctSubseqII(s: string): number {
    const mod = 1e9 + 7;
    const f: number[] = Array(26).fill(0);
    for (const c of s) {
        f[c.charCodeAt(0) - 97] = f.reduce((acc, v) => (acc + v) % mod, 1);
    }
    return f.reduce((acc, v) => (acc + v) % mod);
}
```

#### Rust

```rust
impl Solution {
    pub fn distinct_subseq_ii(s: String) -> i32 {
        const MOD: i32 = 1_000_000_007;
        let mut f = [0; 26];
        for u in s.bytes() {
            let mut x = 1;
            for &v in &f {
                x = (x + v) % MOD;
            }
            f[(u - b'a') as usize] = x;
        }
        f.iter().fold(0, |acc, &v| (acc + v) % MOD)
    }
}
```

#### C

```c
int distinctSubseqII(char* s) {
    const int mod = 1e9 + 7;
    int f[26] = {0};
    for (int i = 0; s[i]; ++i) {
        int x = 1;
        for (int j = 0; j < 26; ++j) {
            x = (x + f[j]) % mod;
        }
        f[s[i] - 'a'] = x;
    }
    int ans = 0;
    for (int i = 0; i < 26; ++i) {
        ans = (ans + f[i]) % mod;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động tối ưu

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 duyệt lại $26$ phần tử mỗi lần cập nhật. Ta duy trì tổng hiện tại $\textit{ans}$; lượng tăng là $\textit{ans}-f[i]+1$, nhờ đó cập nhật cả $f[i]$ và $\textit{ans}$ trong $O(1)$.

<!-- thinking:end -->

Dựa trên Lời giải 1, ta duy trì biến $\textit{ans}$ làm tổng tất cả phần tử trong $f$. Mỗi lần cập nhật $f[i]$, số dãy con khác nhau mới được thêm vào là $\textit{ans} - f[i] + 1$. Sau đó, ta cập nhật cả $\textit{ans}$ và $f[i]$ tương ứng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(C)$.

Bài toán tương tự:

- [1987. Number of Unique Good Subsequences](https://github.com/doocs/leetcode/blob/main/solution/1900-1999/1987.Number%20of%20Unique%20Good%20Subsequences/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distinctSubseqII(self, s: str) -> int:
        mod = 10**9 + 7
        f = [0] * 26
        ans = 0
        for c in s:
            i = ord(c) - ord("a")
            add = (ans + 1 - f[i]) % mod
            ans = (ans + add) % mod
            f[i] = (f[i] + add) % mod
        return ans
```

#### Java

```java
class Solution {
    public int distinctSubseqII(String s) {
        final int mod = (int) 1e9 + 7;
        int[] f = new int[26];
        int ans = 0;
        for (int i = 0; i < s.length(); ++i) {
            int j = s.charAt(i) - 'a';
            int add = (ans + 1 + mod - f[j]) % mod;
            ans = (ans + add) % mod;
            f[j] = (f[j] + add) % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int distinctSubseqII(string s) {
        const int mod = 1e9 + 7;
        int f[26]{};
        int ans = 0;
        for (char& c : s) {
            int i = c - 'a';
            int add = (ans + 1 + mod - f[i]) % mod;
            ans = (ans + add) % mod;
            f[i] = (f[i] + add) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func distinctSubseqII(s string) int {
	const mod int = 1e9 + 7
	f := [26]int{}
	ans := 0
	for _, c := range s {
		i := c - 'a'
		add := (ans + 1 + mod - f[i]) % mod
		ans = (ans + add) % mod
		f[i] = (f[i] + add) % mod
	}
	return ans
}
```

#### TypeScript

```ts
function distinctSubseqII(s: string): number {
    const mod = 1e9 + 7;
    const f: number[] = Array(26).fill(0);
    let ans = 0;
    for (const c of s) {
        const i = c.charCodeAt(0) - 97;
        const add = (ans + 1 + mod - f[i]) % mod;
        ans = (ans + add) % mod;
        f[i] = (f[i] + add) % mod;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn distinct_subseq_ii(s: String) -> i32 {
        const MOD: i32 = 1_000_000_007;
        let mut f = [0; 26];
        let mut ans = 0;
        for u in s.bytes() {
            let i = (u - b'a') as usize;
            let add = (ans + 1 + MOD - f[i]) % MOD;
            ans = (ans + add) % MOD;
            f[i] = (f[i] + add) % MOD;
        }
        ans
    }
}
```

#### C

```c
int distinctSubseqII(char* s) {
    const int mod = 1e9 + 7;
    int f[26] = {0};
    int ans = 0;
    for (int i = 0; s[i]; ++i) {
        int j = s[i] - 'a';
        int add = (ans + 1LL + mod - f[j]) % mod;
        ans = (ans + add) % mod;
        f[j] = (f[j] + add) % mod;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
