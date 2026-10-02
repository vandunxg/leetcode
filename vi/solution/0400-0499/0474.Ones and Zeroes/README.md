---
comments: true
difficulty: Medium
tags:
    - Array
    - String
    - Dynamic Programming
    - Knapsack
    - 0-1 Knapsack
---

<!-- problem:start -->

# [474. Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes)

[中文文档](/solution/0400-0499/0474.Ones%20and%20Zeroes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng các chuỗi nhị phân <code>strs</code> và hai số nguyên <code>m</code>, <code>n</code>.</p>

<p>Hãy trả về <em>kích thước của tập con lớn nhất trong <code>strs</code> sao cho tập con đó có nhiều nhất </em><code>m</code><em> chữ số </em><code>0</code><em> và </em><code>n</code><em> chữ số </em><code>1</code>.</p>

<p>Tập hợp <code>x</code> là <strong>tập con</strong> của tập hợp <code>y</code> nếu mọi phần tử của <code>x</code> cũng là phần tử của <code>y</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;10&quot;,&quot;0001&quot;,&quot;111001&quot;,&quot;1&quot;,&quot;0&quot;], m = 5, n = 3
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Tập con lớn nhất có nhiều nhất 5 chữ số 0 và 3 chữ số 1 là {&quot;10&quot;, &quot;0001&quot;, &quot;1&quot;, &quot;0&quot;}, nên đáp án là 4.
Một số tập con hợp lệ nhưng nhỏ hơn gồm {&quot;0001&quot;, &quot;1&quot;} và {&quot;10&quot;, &quot;1&quot;, &quot;0&quot;}.
{&quot;111001&quot;} không phải tập con hợp lệ vì chứa 4 chữ số 1, vượt quá giới hạn 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;10&quot;,&quot;0&quot;,&quot;1&quot;], m = 1, n = 1
<strong>Đầu ra:</strong> 2
<b>Giải thích:</b> Tập con lớn nhất là {&quot;0&quot;, &quot;1&quot;}, nên đáp án là 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= strs.length &lt;= 600</code></li>
	<li><code>1 &lt;= strs[i].length &lt;= 100</code></li>
	<li><code>strs[i]</code> chỉ gồm các chữ số <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>.</li>
	<li><code>1 &lt;= m, n &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Chọn được nhiều chuỗi nhất với giới hạn số chữ số 0 và 1 là bài toán knapsack $0$-$1$ với hai loại trọng lượng. Duyệt mọi tập con sẽ quá tốn kém.
>
> $f[i][j][k]$ là số chuỗi lớn nhất có thể chọn trong $i$ chuỗi đầu tiên, với nhiều nhất $j$ chữ số 0 và $k$ chữ số 1. Bỏ qua chuỗi hiện tại thì lấy giá trị từ hàng trước; chọn chuỗi thì cộng $1$ nếu không vượt quá giới hạn.
>
> Đếm số chữ số $0$ và $1$ trong chuỗi hiện tại trước khi điền hàng DP để phép chuyển trạng thái dùng đúng chi phí của chuỗi đó.

<!-- thinking:end -->

Định nghĩa $f[i][j][k]$ là số chuỗi lớn nhất có thể chọn trong $i$ chuỗi đầu tiên, sử dụng $j$ chữ số 0 và $k$ chữ số 1. Ban đầu, $f[i][j][k]=0$; đáp án là $f[sz][m][n]$, trong đó $sz$ là độ dài mảng $strs$.

Với $f[i][j][k]$, có hai lựa chọn:

- Không chọn chuỗi thứ $i$, khi đó $f[i][j][k]=f[i-1][j][k]$;
- Chọn chuỗi thứ $i$, khi đó $f[i][j][k]=f[i-1][j-a][k-b]+1$, trong đó $a$ và $b$ lần lượt là số chữ số 0 và 1 trong chuỗi thứ $i$.

Lấy giá trị lớn hơn trong hai lựa chọn này để tính $f[i][j][k]$.

Đáp án cuối cùng là $f[sz][m][n]$.

Độ phức tạp thời gian và không gian đều là $O(sz \times m \times n)$, trong đó $sz$ là độ dài mảng $strs$, còn $m$ và $n$ lần lượt là giới hạn trên về số chữ số 0 và 1.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        sz = len(strs)
        f = [[[0] * (n + 1) for _ in range(m + 1)] for _ in range(sz + 1)]
        for i, s in enumerate(strs, 1):
            a, b = s.count("0"), s.count("1")
            for j in range(m + 1):
                for k in range(n + 1):
                    f[i][j][k] = f[i - 1][j][k]
                    if j >= a and k >= b:
                        f[i][j][k] = max(f[i][j][k], f[i - 1][j - a][k - b] + 1)
        return f[sz][m][n]
```

#### Java

```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int sz = strs.length;
        int[][][] f = new int[sz + 1][m + 1][n + 1];
        for (int i = 1; i <= sz; ++i) {
            int[] cnt = count(strs[i - 1]);
            for (int j = 0; j <= m; ++j) {
                for (int k = 0; k <= n; ++k) {
                    f[i][j][k] = f[i - 1][j][k];
                    if (j >= cnt[0] && k >= cnt[1]) {
                        f[i][j][k] = Math.max(f[i][j][k], f[i - 1][j - cnt[0]][k - cnt[1]] + 1);
                    }
                }
            }
        }
        return f[sz][m][n];
    }

    private int[] count(String s) {
        int[] cnt = new int[2];
        for (int i = 0; i < s.length(); ++i) {
            ++cnt[s.charAt(i) - '0'];
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        int sz = strs.size();
        int f[sz + 1][m + 1][n + 1];
        memset(f, 0, sizeof(f));
        for (int i = 1; i <= sz; ++i) {
            auto [a, b] = count(strs[i - 1]);
            for (int j = 0; j <= m; ++j) {
                for (int k = 0; k <= n; ++k) {
                    f[i][j][k] = f[i - 1][j][k];
                    if (j >= a && k >= b) {
                        f[i][j][k] = max(f[i][j][k], f[i - 1][j - a][k - b] + 1);
                    }
                }
            }
        }
        return f[sz][m][n];
    }

    pair<int, int> count(string& s) {
        int a = count_if(s.begin(), s.end(), [](char c) { return c == '0'; });
        return {a, s.size() - a};
    }
};
```

#### Go

```go
func findMaxForm(strs []string, m int, n int) int {
	sz := len(strs)
	f := make([][][]int, sz+1)
	for i := range f {
		f[i] = make([][]int, m+1)
		for j := range f[i] {
			f[i][j] = make([]int, n+1)
		}
	}
	for i := 1; i <= sz; i++ {
		a, b := count(strs[i-1])
		for j := 0; j <= m; j++ {
			for k := 0; k <= n; k++ {
				f[i][j][k] = f[i-1][j][k]
				if j >= a && k >= b {
					f[i][j][k] = max(f[i][j][k], f[i-1][j-a][k-b]+1)
				}
			}
		}
	}
	return f[sz][m][n]
}

func count(s string) (int, int) {
	a := strings.Count(s, "0")
	return a, len(s) - a
}
```

#### TypeScript

```ts
function findMaxForm(strs: string[], m: number, n: number): number {
    const sz = strs.length;
    const f = Array.from({ length: sz + 1 }, () =>
        Array.from({ length: m + 1 }, () => Array.from({ length: n + 1 }, () => 0)),
    );
    const count = (s: string): [number, number] => {
        let a = 0;
        for (const c of s) {
            a += c === '0' ? 1 : 0;
        }
        return [a, s.length - a];
    };
    for (let i = 1; i <= sz; ++i) {
        const [a, b] = count(strs[i - 1]);
        for (let j = 0; j <= m; ++j) {
            for (let k = 0; k <= n; ++k) {
                f[i][j][k] = f[i - 1][j][k];
                if (j >= a && k >= b) {
                    f[i][j][k] = Math.max(f[i][j][k], f[i - 1][j - a][k - b] + 1);
                }
            }
        }
    }
    return f[sz][m][n];
}
```

#### Rust

```rust
impl Solution {
    pub fn find_max_form(strs: Vec<String>, m: i32, n: i32) -> i32 {
        let sz = strs.len();
        let m = m as usize;
        let n = n as usize;
        let mut f = vec![vec![vec![0; n + 1]; m + 1]; sz + 1];
        for i in 1..=sz {
            let a = strs[i - 1].chars().filter(|&c| c == '0').count();
            let b = strs[i - 1].len() - a;
            for j in 0..=m {
                for k in 0..=n {
                    f[i][j][k] = f[i - 1][j][k];
                    if j >= a && k >= b {
                        f[i][j][k] = f[i][j][k].max(f[i - 1][j - a][k - b] + 1);
                    }
                }
            }
        }
        f[sz][m][n] as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Dynamic Programming (Tối ưu bộ nhớ)

<!-- thinking:start -->

> **Tư duy**
>
> Hàng $i$ chỉ phụ thuộc vào hàng $i-1$. Duyệt $j$ và $k$ theo thứ tự giảm dần rồi bỏ chiều thứ nhất. Bộ nhớ giảm còn $O(mn)$, đồng thời đảm bảo mỗi chuỗi không bị chọn nhiều lần.

<!-- thinking:end -->

Ta nhận thấy việc tính $f[i][j][k]$ chỉ phụ thuộc vào $f[i-1][j][k]$ và $f[i-1][j-a][k-b]$. Vì vậy, có thể bỏ chiều thứ nhất và giảm độ phức tạp không gian xuống còn $O(m \times n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        f = [[0] * (n + 1) for _ in range(m + 1)]
        for s in strs:
            a, b = s.count("0"), s.count("1")
            for i in range(m, a - 1, -1):
                for j in range(n, b - 1, -1):
                    f[i][j] = max(f[i][j], f[i - a][j - b] + 1)
        return f[m][n]
```

#### Java

```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int[][] f = new int[m + 1][n + 1];
        for (String s : strs) {
            int[] cnt = count(s);
            for (int i = m; i >= cnt[0]; --i) {
                for (int j = n; j >= cnt[1]; --j) {
                    f[i][j] = Math.max(f[i][j], f[i - cnt[0]][j - cnt[1]] + 1);
                }
            }
        }
        return f[m][n];
    }

    private int[] count(String s) {
        int[] cnt = new int[2];
        for (int i = 0; i < s.length(); ++i) {
            ++cnt[s.charAt(i) - '0'];
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        int f[m + 1][n + 1];
        memset(f, 0, sizeof(f));
        for (auto& s : strs) {
            auto [a, b] = count(s);
            for (int i = m; i >= a; --i) {
                for (int j = n; j >= b; --j) {
                    f[i][j] = max(f[i][j], f[i - a][j - b] + 1);
                }
            }
        }
        return f[m][n];
    }

    pair<int, int> count(string& s) {
        int a = count_if(s.begin(), s.end(), [](char c) { return c == '0'; });
        return {a, s.size() - a};
    }
};
```

#### Go

```go
func findMaxForm(strs []string, m int, n int) int {
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	for _, s := range strs {
		a, b := count(s)
		for j := m; j >= a; j-- {
			for k := n; k >= b; k-- {
				f[j][k] = max(f[j][k], f[j-a][k-b]+1)
			}
		}
	}
	return f[m][n]
}

func count(s string) (int, int) {
	a := strings.Count(s, "0")
	return a, len(s) - a
}
```

#### TypeScript

```ts
function findMaxForm(strs: string[], m: number, n: number): number {
    const f = Array.from({ length: m + 1 }, () => Array.from({ length: n + 1 }, () => 0));
    const count = (s: string): [number, number] => {
        let a = 0;
        for (const c of s) {
            a += c === '0' ? 1 : 0;
        }
        return [a, s.length - a];
    };
    for (const s of strs) {
        const [a, b] = count(s);
        for (let i = m; i >= a; --i) {
            for (let j = n; j >= b; --j) {
                f[i][j] = Math.max(f[i][j], f[i - a][j - b] + 1);
            }
        }
    }
    return f[m][n];
}
```

#### Rust

```rust
impl Solution {
    pub fn find_max_form(strs: Vec<String>, m: i32, n: i32) -> i32 {
        let m = m as usize;
        let n = n as usize;
        let mut f = vec![vec![0; n + 1]; m + 1];

        for s in strs {
            let a = s.chars().filter(|&c| c == '0').count();
            let b = s.len() - a;
            for i in (a..=m).rev() {
                for j in (b..=n).rev() {
                    f[i][j] = f[i][j].max(f[i - a][j - b] + 1);
                }
            }
        }

        f[m][n]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
