---
comments: true
difficulty: Medium
rating: 1653
source: Weekly Contest 149 Q2
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [1155. Number of Dice Rolls With Target Sum](https://leetcode.com/problems/number-of-dice-rolls-with-target-sum)

[中文文档](/solution/1100-1199/1155.Number%20of%20Dice%20Rolls%20With%20Target%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>n</code> viên xúc xắc, mỗi viên có <code>k</code> mặt được đánh số từ <code>1</code> đến <code>k</code>.</p>

<p>Cho ba số nguyên <code>n</code>, <code>k</code> và <code>target</code>, hãy trả về <em>số cách có thể (trong tổng số </em><code>k<sup>n</sup></code><em> cách) </em><em>gieo xúc xắc sao cho tổng các mặt xuất hiện bằng </em><code>target</code>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, k = 6, target = 3
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Bạn gieo một viên xúc xắc có 6 mặt.
Chỉ có một cách để được tổng bằng 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, k = 6, target = 7
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Bạn gieo hai viên xúc xắc, mỗi viên có 6 mặt.
Có 6 cách để được tổng bằng 7: 1+6, 2+5, 3+4, 4+3, 5+2, 6+1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 30, k = 30, target = 500
<strong>Đầu ra:</strong> 222616187
<strong>Giải thích:</strong> Đáp án cần được trả về modulo 10<sup>9</sup> + 7.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, k &lt;= 30</code></li>
	<li><code>1 &lt;= target &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Nếu thử mọi cách gieo $n$ viên xúc xắc có $k$ mặt để đạt tổng $target$, số trường hợp là $k^n$. Gọi $f[i][j]$ là số cách để $i$ viên xúc xắc có tổng bằng $j$; nếu viên cuối ra mặt $h$, số cách tương ứng là $f[i-1][j-h]$. Có một cách dùng 0 viên xúc xắc để tạo tổng 0. Lấy kết quả modulo $10^9+7$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số cách đạt tổng $j$ khi gieo $i$ viên xúc xắc. Khi đó, công thức chuyển trạng thái là:

$$
f[i][j] = \sum_{h=1}^{\min(j, k)} f[i-1][j-h]
$$

Trong đó, $h$ là số chấm xuất hiện trên viên xúc xắc thứ $i$.

Ban đầu, ta có $f[0][0] = 1$; đáp án cuối cùng là $f[n][target]$.

Độ phức tạp thời gian là $O(n \times k \times target)$, độ phức tạp không gian là $O(n \times target)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numRollsToTarget(self, n: int, k: int, target: int) -> int:
        f = [[0] * (target + 1) for _ in range(n + 1)]
        f[0][0] = 1
        mod = 10**9 + 7
        for i in range(1, n + 1):
            for j in range(1, min(i * k, target) + 1):
                for h in range(1, min(j, k) + 1):
                    f[i][j] = (f[i][j] + f[i - 1][j - h]) % mod
        return f[n][target]
```

#### Java

```java
class Solution {
    public int numRollsToTarget(int n, int k, int target) {
        final int mod = (int) 1e9 + 7;
        int[][] f = new int[n + 1][target + 1];
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= Math.min(target, i * k); ++j) {
                for (int h = 1; h <= Math.min(j, k); ++h) {
                    f[i][j] = (f[i][j] + f[i - 1][j - h]) % mod;
                }
            }
        }
        return f[n][target];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numRollsToTarget(int n, int k, int target) {
        const int mod = 1e9 + 7;
        int f[n + 1][target + 1];
        memset(f, 0, sizeof f);
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= min(target, i * k); ++j) {
                for (int h = 1; h <= min(j, k); ++h) {
                    f[i][j] = (f[i][j] + f[i - 1][j - h]) % mod;
                }
            }
        }
        return f[n][target];
    }
};
```

#### Go

```go
func numRollsToTarget(n int, k int, target int) int {
	const mod int = 1e9 + 7
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, target+1)
	}
	f[0][0] = 1
	for i := 1; i <= n; i++ {
		for j := 1; j <= min(target, i*k); j++ {
			for h := 1; h <= min(j, k); h++ {
				f[i][j] = (f[i][j] + f[i-1][j-h]) % mod
			}
		}
	}
	return f[n][target]
}
```

#### TypeScript

```ts
function numRollsToTarget(n: number, k: number, target: number): number {
    const f = Array.from({ length: n + 1 }, () => Array(target + 1).fill(0));
    f[0][0] = 1;
    const mod = 1e9 + 7;
    for (let i = 1; i <= n; ++i) {
        for (let j = 1; j <= Math.min(i * k, target); ++j) {
            for (let h = 1; h <= Math.min(j, k); ++h) {
                f[i][j] = (f[i][j] + f[i - 1][j - h]) % mod;
            }
        }
    }
    return f[n][target];
}
```

#### Rust

```rust
impl Solution {
    pub fn num_rolls_to_target(n: i32, k: i32, target: i32) -> i32 {
        let _mod = 1_000_000_007;
        let n = n as usize;
        let k = k as usize;
        let target = target as usize;
        let mut f = vec![vec![0; target + 1]; n + 1];
        f[0][0] = 1;

        for i in 1..=n {
            for j in 1..=target.min(i * k) {
                for h in 1..=j.min(k) {
                    f[i][j] = (f[i][j] + f[i - 1][j - h]) % _mod;
                }
            }
        }

        f[n][target]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động (Rolling Array)

<!-- thinking:start -->

> **Tư duy**
>
> Hàng $i$ của lời giải 1 chỉ phụ thuộc vào hàng $i-1$. Dùng hai rolling array giúp giảm độ phức tạp không gian từ $O(n\times target)$ xuống $O(target)$ mà vẫn giữ nguyên công thức chuyển trạng thái.

<!-- thinking:end -->

$f[i][j]$ chỉ phụ thuộc vào hàng trước đó, nên chỉ cần hai mảng $f$ và $g$ có độ dài $target+1$. Độ phức tạp không gian giảm còn $O(target)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numRollsToTarget(self, n: int, k: int, target: int) -> int:
        f = [1] + [0] * target
        mod = 10**9 + 7
        for i in range(1, n + 1):
            g = [0] * (target + 1)
            for j in range(1, min(i * k, target) + 1):
                for h in range(1, min(j, k) + 1):
                    g[j] = (g[j] + f[j - h]) % mod
            f = g
        return f[target]
```

#### Java

```java
class Solution {
    public int numRollsToTarget(int n, int k, int target) {
        final int mod = (int) 1e9 + 7;
        int[] f = new int[target + 1];
        f[0] = 1;
        for (int i = 1; i <= n; ++i) {
            int[] g = new int[target + 1];
            for (int j = 1; j <= Math.min(target, i * k); ++j) {
                for (int h = 1; h <= Math.min(j, k); ++h) {
                    g[j] = (g[j] + f[j - h]) % mod;
                }
            }
            f = g;
        }
        return f[target];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numRollsToTarget(int n, int k, int target) {
        const int mod = 1e9 + 7;
        vector<int> f(target + 1);
        f[0] = 1;
        for (int i = 1; i <= n; ++i) {
            vector<int> g(target + 1);
            for (int j = 1; j <= min(target, i * k); ++j) {
                for (int h = 1; h <= min(j, k); ++h) {
                    g[j] = (g[j] + f[j - h]) % mod;
                }
            }
            f = move(g);
        }
        return f[target];
    }
};
```

#### Go

```go
func numRollsToTarget(n int, k int, target int) int {
	const mod int = 1e9 + 7
	f := make([]int, target+1)
	f[0] = 1
	for i := 1; i <= n; i++ {
		g := make([]int, target+1)
		for j := 1; j <= min(target, i*k); j++ {
			for h := 1; h <= min(j, k); h++ {
				g[j] = (g[j] + f[j-h]) % mod
			}
		}
		f = g
	}
	return f[target]
}
```

#### TypeScript

```ts
function numRollsToTarget(n: number, k: number, target: number): number {
    const f = Array(target + 1).fill(0);
    f[0] = 1;
    const mod = 1e9 + 7;
    for (let i = 1; i <= n; ++i) {
        const g = Array(target + 1).fill(0);
        for (let j = 1; j <= Math.min(i * k, target); ++j) {
            for (let h = 1; h <= Math.min(j, k); ++h) {
                g[j] = (g[j] + f[j - h]) % mod;
            }
        }
        f.splice(0, target + 1, ...g);
    }
    return f[target];
}
```

#### Rust

```rust
impl Solution {
    pub fn num_rolls_to_target(n: i32, k: i32, target: i32) -> i32 {
        let _mod = 1_000_000_007;
        let n = n as usize;
        let k = k as usize;
        let target = target as usize;
        let mut f = vec![0; target + 1];
        f[0] = 1;

        for i in 1..=n {
            let mut g = vec![0; target + 1];
            for j in 1..=target {
                for h in 1..=j.min(k) {
                    g[j] = (g[j] + f[j - h]) % _mod;
                }
            }
            f = g;
        }

        f[target]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
