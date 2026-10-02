---
comments: true
difficulty: Hard
tags:
    - String
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [903. Valid Permutations for DI Sequence](https://leetcode.com/problems/valid-permutations-for-di-sequence)

[中文文档](/solution/0900-0999/0903.Valid%20Permutations%20for%20DI%20Sequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> có độ dài <code>n</code>, trong đó <code>s[i]</code> thuộc một trong hai giá trị sau:</p>

<ul>
	<li><code>&#39;D&#39;</code> biểu thị thứ tự giảm dần,</li>
	<li><code>&#39;I&#39;</code> biểu thị thứ tự tăng dần.</li>
</ul>

<p>Một hoán vị <code>perm</code> gồm đủ <code>n + 1</code> số nguyên trong khoảng <code>[0, n]</code> được gọi là <strong>hoán vị hợp lệ</strong> nếu với mọi <code>i</code> hợp lệ:</p>

<ul>
	<li>Nếu <code>s[i] == &#39;D&#39;</code>, thì <code>perm[i] &gt; perm[i + 1]</code>, và</li>
	<li>Nếu <code>s[i] == &#39;I&#39;</code>, thì <code>perm[i] &lt; perm[i + 1]</code>.</li>
</ul>

<p>Trả về <em>số lượng <strong>hoán vị hợp lệ</strong> </em><code>perm</code>. Vì kết quả có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;DID&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Có 5 hoán vị hợp lệ của (0, 1, 2, 3):
(1, 0, 3, 2)
(2, 0, 3, 1)
(2, 1, 3, 0)
(3, 0, 2, 1)
(3, 1, 2, 0)
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;D&quot;
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == s.length</code></li>
	<li><code>1 &lt;= n &lt;= 200</code></li>
	<li><code>s[i]</code> chỉ có thể là <code>&#39;I&#39;</code> hoặc <code>&#39;D&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Với chuỗi DI dài $n$, ta cần tìm các hoán vị của $0..n$. Vì $n$ có thể bằng $200$, không thể liệt kê tất cả hoán vị. Sau khi đặt $i$ số, thứ hạng $j$ của số cuối trong các giá trị còn lại là yếu tố duy nhất cần xét để xác định bước tăng hay giảm tiếp theo.
>
> Gọi $f[i][j]$ là số cách thỏa mãn $i$ ký tự đầu tiên, với số cuối có thứ hạng $j$. Với `'D'`, ta cộng các trạng thái có thứ hạng lớn hơn; với `'I'`, ta cộng các trạng thái có thứ hạng nhỏ hơn, sau khi chuẩn hóa thứ hạng về đoạn $[0,i]$. Công thức truy hồi thu được có độ phức tạp bậc ba.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số hoán vị thỏa mãn yêu cầu của đề bài với $i$ ký tự đầu tiên của chuỗi, trong đó số cuối có thứ hạng $j$. Ban đầu, $f[0][0]=1$, còn các giá trị $f[0][j]$ khác bằng $0$. Đáp án là $\sum_{j=0}^n f[n][j]$.

Xét $f[i][j]$, với $j \in [0, i]$.

Nếu ký tự thứ $i$, tức $s[i-1]$, là `'D'`, thì $f[i][j]$ được chuyển từ $f[i-1][k]$ với $k \in [j+1, i]$. Vì thứ hạng $k-1$ tối đa là $i-1$, ta dịch $k$ sang trái một vị trí, nên $k \in [j, i-1]$. Do đó, $f[i][j] = \sum_{k=j}^{i-1} f[i-1][k]$.

Nếu ký tự thứ $i$, tức $s[i-1]$, là `'I'`, thì $f[i][j]$ được chuyển từ $f[i-1][k]$ với $k \in [0, j-1]$. Do đó, $f[i][j] = \sum_{k=0}^{j-1} f[i-1][k]$.

Đáp án cuối cùng là $\sum_{j=0}^n f[n][j]$.

Độ phức tạp thời gian là $O(n^3)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numPermsDISequence(self, s: str) -> int:
        mod = 10**9 + 7
        n = len(s)
        f = [[0] * (n + 1) for _ in range(n + 1)]
        f[0][0] = 1
        for i, c in enumerate(s, 1):
            if c == "D":
                for j in range(i + 1):
                    for k in range(j, i):
                        f[i][j] = (f[i][j] + f[i - 1][k]) % mod
            else:
                for j in range(i + 1):
                    for k in range(j):
                        f[i][j] = (f[i][j] + f[i - 1][k]) % mod
        return sum(f[n][j] for j in range(n + 1)) % mod
```

#### Java

```java
class Solution {
    public int numPermsDISequence(String s) {
        final int mod = (int) 1e9 + 7;
        int n = s.length();
        int[][] f = new int[n + 1][n + 1];
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            if (s.charAt(i - 1) == 'D') {
                for (int j = 0; j <= i; ++j) {
                    for (int k = j; k < i; ++k) {
                        f[i][j] = (f[i][j] + f[i - 1][k]) % mod;
                    }
                }
            } else {
                for (int j = 0; j <= i; ++j) {
                    for (int k = 0; k < j; ++k) {
                        f[i][j] = (f[i][j] + f[i - 1][k]) % mod;
                    }
                }
            }
        }
        int ans = 0;
        for (int j = 0; j <= n; ++j) {
            ans = (ans + f[n][j]) % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numPermsDISequence(string s) {
        const int mod = 1e9 + 7;
        int n = s.size();
        int f[n + 1][n + 1];
        memset(f, 0, sizeof(f));
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            if (s[i - 1] == 'D') {
                for (int j = 0; j <= i; ++j) {
                    for (int k = j; k < i; ++k) {
                        f[i][j] = (f[i][j] + f[i - 1][k]) % mod;
                    }
                }
            } else {
                for (int j = 0; j <= i; ++j) {
                    for (int k = 0; k < j; ++k) {
                        f[i][j] = (f[i][j] + f[i - 1][k]) % mod;
                    }
                }
            }
        }
        int ans = 0;
        for (int j = 0; j <= n; ++j) {
            ans = (ans + f[n][j]) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func numPermsDISequence(s string) (ans int) {
	const mod = 1e9 + 7
	n := len(s)
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	f[0][0] = 1
	for i := 1; i <= n; i++ {
		if s[i-1] == 'D' {
			for j := 0; j <= i; j++ {
				for k := j; k < i; k++ {
					f[i][j] = (f[i][j] + f[i-1][k]) % mod
				}
			}
		} else {
			for j := 0; j <= i; j++ {
				for k := 0; k < j; k++ {
					f[i][j] = (f[i][j] + f[i-1][k]) % mod
				}
			}
		}
	}
	for j := 0; j <= n; j++ {
		ans = (ans + f[n][j]) % mod
	}
	return
}
```

#### TypeScript

```ts
function numPermsDISequence(s: string): number {
    const n = s.length;
    const f: number[][] = Array(n + 1)
        .fill(0)
        .map(() => Array(n + 1).fill(0));
    f[0][0] = 1;
    const mod = 10 ** 9 + 7;
    for (let i = 1; i <= n; ++i) {
        if (s[i - 1] === 'D') {
            for (let j = 0; j <= i; ++j) {
                for (let k = j; k < i; ++k) {
                    f[i][j] = (f[i][j] + f[i - 1][k]) % mod;
                }
            }
        } else {
            for (let j = 0; j <= i; ++j) {
                for (let k = 0; k < j; ++k) {
                    f[i][j] = (f[i][j] + f[i - 1][k]) % mod;
                }
            }
        }
    }
    let ans = 0;
    for (let j = 0; j <= n; ++j) {
        ans = (ans + f[n][j]) % mod;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tối ưu bằng prefix sum

<!-- thinking:start -->

> **Tư duy**
>
> Ở phương pháp 1, mỗi giá trị $j$ đều phải duyệt một đoạn $k$, trong khi các giá trị $j$ liên tiếp có tổng trên các đoạn gần như trùng nhau. Dùng prefix sum theo chiều phù hợp giúp tính $\sum f[i-1][k]$ trong thời gian hằng số, giảm độ phức tạp thời gian xuống bậc hai mà không đổi trạng thái.

<!-- thinking:end -->

Ta có thể tối ưu các chuyển trạng thái bằng prefix sum, giảm độ phức tạp thời gian xuống $O(n^2)$. Độ phức tạp không gian vẫn là $O(n^2)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numPermsDISequence(self, s: str) -> int:
        mod = 10**9 + 7
        n = len(s)
        f = [[0] * (n + 1) for _ in range(n + 1)]
        f[0][0] = 1
        for i, c in enumerate(s, 1):
            pre = 0
            if c == "D":
                for j in range(i, -1, -1):
                    pre = (pre + f[i - 1][j]) % mod
                    f[i][j] = pre
            else:
                for j in range(i + 1):
                    f[i][j] = pre
                    pre = (pre + f[i - 1][j]) % mod
        return sum(f[n][j] for j in range(n + 1)) % mod
```

#### Java

```java
class Solution {
    public int numPermsDISequence(String s) {
        final int mod = (int) 1e9 + 7;
        int n = s.length();
        int[][] f = new int[n + 1][n + 1];
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            int pre = 0;
            if (s.charAt(i - 1) == 'D') {
                for (int j = i; j >= 0; --j) {
                    pre = (pre + f[i - 1][j]) % mod;
                    f[i][j] = pre;
                }
            } else {
                for (int j = 0; j <= i; ++j) {
                    f[i][j] = pre;
                    pre = (pre + f[i - 1][j]) % mod;
                }
            }
        }
        int ans = 0;
        for (int j = 0; j <= n; ++j) {
            ans = (ans + f[n][j]) % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numPermsDISequence(string s) {
        const int mod = 1e9 + 7;
        int n = s.size();
        int f[n + 1][n + 1];
        memset(f, 0, sizeof(f));
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            int pre = 0;
            if (s[i - 1] == 'D') {
                for (int j = i; j >= 0; --j) {
                    pre = (pre + f[i - 1][j]) % mod;
                    f[i][j] = pre;
                }
            } else {
                for (int j = 0; j <= i; ++j) {
                    f[i][j] = pre;
                    pre = (pre + f[i - 1][j]) % mod;
                }
            }
        }
        int ans = 0;
        for (int j = 0; j <= n; ++j) {
            ans = (ans + f[n][j]) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func numPermsDISequence(s string) (ans int) {
	const mod = 1e9 + 7
	n := len(s)
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	f[0][0] = 1
	for i := 1; i <= n; i++ {
		pre := 0
		if s[i-1] == 'D' {
			for j := i; j >= 0; j-- {
				pre = (pre + f[i-1][j]) % mod
				f[i][j] = pre
			}
		} else {
			for j := 0; j <= i; j++ {
				f[i][j] = pre
				pre = (pre + f[i-1][j]) % mod
			}
		}
	}
	for j := 0; j <= n; j++ {
		ans = (ans + f[n][j]) % mod
	}
	return
}
```

#### TypeScript

```ts
function numPermsDISequence(s: string): number {
    const n = s.length;
    const f: number[][] = Array(n + 1)
        .fill(0)
        .map(() => Array(n + 1).fill(0));
    f[0][0] = 1;
    const mod = 10 ** 9 + 7;
    for (let i = 1; i <= n; ++i) {
        let pre = 0;
        if (s[i - 1] === 'D') {
            for (let j = i; j >= 0; --j) {
                pre = (pre + f[i - 1][j]) % mod;
                f[i][j] = pre;
            }
        } else {
            for (let j = 0; j <= i; ++j) {
                f[i][j] = pre;
                pre = (pre + f[i - 1][j]) % mod;
            }
        }
    }
    let ans = 0;
    for (let j = 0; j <= n; ++j) {
        ans = (ans + f[n][j]) % mod;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Tối ưu bằng mảng cuộn

<!-- thinking:start -->

> **Tư duy**
>
> $f[i]$ chỉ phụ thuộc vào $f[i-1]$, nên có thể bỏ chiều thứ hai của bảng. Dùng một mảng một chiều cuộn qua các bước, tính lớp kế tiếp bằng tổng tích lũy theo chiều `'D'` hoặc `'I'`, chỉ cần thêm không gian tuyến tính.

<!-- thinking:end -->

Dựa trên Lời giải 2, ta dùng mảng cuộn để giảm độ phức tạp không gian xuống $O(n)$. Độ phức tạp thời gian vẫn là $O(n^2)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numPermsDISequence(self, s: str) -> int:
        mod = 10**9 + 7
        n = len(s)
        f = [1] + [0] * n
        for i, c in enumerate(s, 1):
            pre = 0
            g = [0] * (n + 1)
            if c == "D":
                for j in range(i, -1, -1):
                    pre = (pre + f[j]) % mod
                    g[j] = pre
            else:
                for j in range(i + 1):
                    g[j] = pre
                    pre = (pre + f[j]) % mod
            f = g
        return sum(f) % mod
```

#### Java

```java
class Solution {
    public int numPermsDISequence(String s) {
        final int mod = (int) 1e9 + 7;
        int n = s.length();
        int[] f = new int[n + 1];
        f[0] = 1;
        for (int i = 1; i <= n; ++i) {
            int pre = 0;
            int[] g = new int[n + 1];
            if (s.charAt(i - 1) == 'D') {
                for (int j = i; j >= 0; --j) {
                    pre = (pre + f[j]) % mod;
                    g[j] = pre;
                }
            } else {
                for (int j = 0; j <= i; ++j) {
                    g[j] = pre;
                    pre = (pre + f[j]) % mod;
                }
            }
            f = g;
        }
        int ans = 0;
        for (int j = 0; j <= n; ++j) {
            ans = (ans + f[j]) % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numPermsDISequence(string s) {
        const int mod = 1e9 + 7;
        int n = s.size();
        vector<int> f(n + 1);
        f[0] = 1;
        for (int i = 1; i <= n; ++i) {
            int pre = 0;
            vector<int> g(n + 1);
            if (s[i - 1] == 'D') {
                for (int j = i; j >= 0; --j) {
                    pre = (pre + f[j]) % mod;
                    g[j] = pre;
                }
            } else {
                for (int j = 0; j <= i; ++j) {
                    g[j] = pre;
                    pre = (pre + f[j]) % mod;
                }
            }
            f = move(g);
        }
        int ans = 0;
        for (int j = 0; j <= n; ++j) {
            ans = (ans + f[j]) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func numPermsDISequence(s string) (ans int) {
	const mod = 1e9 + 7
	n := len(s)
	f := make([]int, n+1)
	f[0] = 1
	for i := 1; i <= n; i++ {
		pre := 0
		g := make([]int, n+1)
		if s[i-1] == 'D' {
			for j := i; j >= 0; j-- {
				pre = (pre + f[j]) % mod
				g[j] = pre
			}
		} else {
			for j := 0; j <= i; j++ {
				g[j] = pre
				pre = (pre + f[j]) % mod
			}
		}
		f = g
	}
	for j := 0; j <= n; j++ {
		ans = (ans + f[j]) % mod
	}
	return
}
```

#### TypeScript

```ts
function numPermsDISequence(s: string): number {
    const n = s.length;
    let f: number[] = Array(n + 1).fill(0);
    f[0] = 1;
    const mod = 10 ** 9 + 7;
    for (let i = 1; i <= n; ++i) {
        let pre = 0;
        const g: number[] = Array(n + 1).fill(0);
        if (s[i - 1] === 'D') {
            for (let j = i; j >= 0; --j) {
                pre = (pre + f[j]) % mod;
                g[j] = pre;
            }
        } else {
            for (let j = 0; j <= i; ++j) {
                g[j] = pre;
                pre = (pre + f[j]) % mod;
            }
        }
        f = g;
    }
    let ans = 0;
    for (let j = 0; j <= n; ++j) {
        ans = (ans + f[j]) % mod;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
