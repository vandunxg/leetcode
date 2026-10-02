---
comments: true
difficulty: Hard
rating: 2008
source: Weekly Contest 158 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1223. Dice Roll Simulation](https://leetcode.com/problems/dice-roll-simulation)

[中文文档](/solution/1200-1299/1223.Dice%20Roll%20Simulation/README.md)

## Mô tả

<!-- description:start -->

<p>Mô phỏng gieo xúc xắc tạo ra một số ngẫu nhiên từ <code>1</code> đến <code>6</code> cho mỗi lần gieo. Bạn thêm một ràng buộc để số <code>i</code> không thể xuất hiện liên tiếp quá <code>rollMax[i]</code> lần (<strong>đánh chỉ số từ 1</strong>).</p>

<p>Cho mảng số nguyên <code>rollMax</code> và số nguyên <code>n</code>. Hãy trả về <em>số lượng dãy khác nhau có thể tạo được sau đúng </em><code>n</code><em> lần gieo</em>. Vì đáp án có thể rất lớn, hãy trả về kết quả theo <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Hai dãy được xem là khác nhau nếu có ít nhất một phần tử khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 2, rollMax = [1,1,2,2,2,3]
<strong>Output:</strong> 34
<strong>Giải thích:</strong> Xúc xắc được gieo 2 lần. Nếu không có ràng buộc, có 6 * 6 = 36 tổ hợp. Theo mảng rollMax, các số 1 và 2 chỉ được xuất hiện liên tiếp tối đa một lần, vì vậy không thể có các dãy (1,1) và (2,2). Do đó, đáp án cuối cùng là 36-2 = 34.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 2, rollMax = [1,1,1,1,1,1]
<strong>Output:</strong> 30
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> n = 3, rollMax = [1,1,1,2,2,3]
<strong>Output:</strong> 181
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 5000</code></li>
	<li><code>rollMax.length == 6</code></li>
	<li><code>1 &lt;= rollMax[i] &lt;= 15</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Một mặt xúc xắc chỉ được xuất hiện liên tiếp tối đa $rollMax$ lần. $n$ có thể lên đến $5000$, nên không thể liệt kê các dãy. Số cách chỉ phụ thuộc vào số lần gieo còn lại, mặt xúc xắc gần nhất và số lần mặt đó đã xuất hiện liên tiếp.
>
> $dfs(i,j,x)$ có ghi nhớ biểu diễn số cách bắt đầu từ lần gieo thứ $i$, sau khi mặt $j$ đã xuất hiện liên tiếp $x$ lần. Mặt tiếp theo $k$ sẽ bắt đầu streak mới nếu $k\ne j$, hoặc tiếp tục streak hiện tại nếu chưa vượt giới hạn. Trạng thái đầu $(0,0,0)$ nghĩa là chưa gieo lần nào.
>
> Có $O(n\times 6\times 15)$ trạng thái và mỗi trạng thái có $6$ chuyển tiếp; ghi nhớ giúp dùng chung kết quả các bài toán con.

<!-- thinking:end -->

Ta định nghĩa hàm $dfs(i, j, x)$ là số cách bắt đầu từ lần gieo thứ $i$, với mặt hiện tại là $j$ và mặt $j$ đã xuất hiện liên tiếp $x$ lần. $j$ nằm trong đoạn $[1, 6]$, còn $x$ nằm trong đoạn $[1, rollMax[j - 1]]$. Đáp án là $dfs(0, 0, 0)$.

Hàm $dfs(i, j, x)$ được tính như sau:

- Nếu $i \ge n$, nghĩa là đã gieo đủ $n$ lần, trả về $1$.
- Nếu không, duyệt số $k$ sẽ gieo tiếp theo. Nếu $k \ne j$, ta gieo $k$ và đặt số lần xuất hiện liên tiếp của mặt $k$ thành $1$, nên số cách là $dfs(i + 1, k, 1)$. Nếu $k = j$, cần kiểm tra $x$ có nhỏ hơn $rollMax[j - 1]$ hay không. Nếu nhỏ hơn, ta có thể tiếp tục gieo $j$ và tăng số lần xuất hiện liên tiếp thêm $1$, nên số cách là $dfs(i + 1, j, x + 1)$. Cuối cùng, cộng số cách của tất cả lựa chọn để tính $dfs(i, j, x)$. Vì đáp án có thể rất lớn, cần lấy modulo $10^9 + 7$.

Trong quá trình tính, có thể dùng ghi nhớ để tránh tính toán lặp lại.

Độ phức tạp thời gian là $O(n \times k^2 \times M)$ và độ phức tạp không gian là $O(n \times k \times M)$. Trong đó, $k$ là số mặt xúc xắc và $M$ là số lần tối đa một mặt có thể xuất hiện liên tiếp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def dieSimulator(self, n: int, rollMax: List[int]) -> int:
        @cache
        def dfs(i, j, x):
            if i >= n:
                return 1
            ans = 0
            for k in range(1, 7):
                if k != j:
                    ans += dfs(i + 1, k, 1)
                elif x < rollMax[j - 1]:
                    ans += dfs(i + 1, j, x + 1)
            return ans % (10**9 + 7)

        return dfs(0, 0, 0)
```

#### Java

```java
class Solution {
    private Integer[][][] f;
    private int[] rollMax;

    public int dieSimulator(int n, int[] rollMax) {
        f = new Integer[n][7][16];
        this.rollMax = rollMax;
        return dfs(0, 0, 0);
    }

    private int dfs(int i, int j, int x) {
        if (i >= f.length) {
            return 1;
        }
        if (f[i][j][x] != null) {
            return f[i][j][x];
        }
        long ans = 0;
        for (int k = 1; k <= 6; ++k) {
            if (k != j) {
                ans += dfs(i + 1, k, 1);
            } else if (x < rollMax[j - 1]) {
                ans += dfs(i + 1, j, x + 1);
            }
        }
        ans %= 1000000007;
        return f[i][j][x] = (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int dieSimulator(int n, vector<int>& rollMax) {
        int f[n][7][16];
        memset(f, 0, sizeof f);
        const int mod = 1e9 + 7;
        function<int(int, int, int)> dfs = [&](int i, int j, int x) -> int {
            if (i >= n) {
                return 1;
            }
            if (f[i][j][x]) {
                return f[i][j][x];
            }
            long ans = 0;
            for (int k = 1; k <= 6; ++k) {
                if (k != j) {
                    ans += dfs(i + 1, k, 1);
                } else if (x < rollMax[j - 1]) {
                    ans += dfs(i + 1, j, x + 1);
                }
            }
            ans %= mod;
            return f[i][j][x] = ans;
        };
        return dfs(0, 0, 0);
    }
};
```

#### Go

```go
func dieSimulator(n int, rollMax []int) int {
	f := make([][7][16]int, n)
	const mod = 1e9 + 7
	var dfs func(i, j, x int) int
	dfs = func(i, j, x int) int {
		if i >= n {
			return 1
		}
		if f[i][j][x] != 0 {
			return f[i][j][x]
		}
		ans := 0
		for k := 1; k <= 6; k++ {
			if k != j {
				ans += dfs(i+1, k, 1)
			} else if x < rollMax[j-1] {
				ans += dfs(i+1, j, x+1)
			}
		}
		f[i][j][x] = ans % mod
		return f[i][j][x]
	}
	return dfs(0, 0, 0)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ghi nhớ áp dụng các chuyển trạng thái theo thứ tự đệ quy. Với cách bottom-up, $f[i][j][x]$ là số cách sau $i$ lần gieo, kết thúc bằng mặt $j$ với streak dài $x$; ta tính từ lớp $i-1$ theo cùng quy tắc. Cách này không cần call stack và có thể tối ưu bằng rolling array.

<!-- thinking:end -->

Có thể chuyển cách tìm kiếm có ghi nhớ ở lời giải 1 thành quy hoạch động.

Định nghĩa $f[i][j][x]$ là số cách thực hiện $i$ lần gieo đầu tiên, trong đó lần gieo thứ $i$ ra mặt $j$ và mặt $j$ đã xuất hiện liên tiếp $x$ lần. Ban đầu, $f[1][j][1] = 1$, với $1 \leq j \leq 6$. Đáp án là:

$$
\sum_{j=1}^6 \sum_{x=1}^{rollMax[j-1]} f[n][j][x]
$$

Ta xét mặt của lần gieo trước là $j$ và số lần liên tiếp mặt $j$ xuất hiện là $x$. Mặt xúc xắc hiện tại có thể là $1, 2, \cdots, 6$. Gọi mặt hiện tại là $k$; có hai trường hợp:

- Nếu $k \neq j$, ta có thể gieo $k$ và streak mới sẽ có độ dài $1$. Vì vậy, cộng $f[i-1][j][x]$ vào số cách $f[i][k][1]$.
- Nếu $k = j$, cần kiểm tra $x+1$ có nhỏ hơn hoặc bằng $rollMax[j-1]$ hay không. Nếu có, ta tiếp tục gieo $j$ và tăng độ dài streak thêm $1$. Vì vậy, cộng $f[i-1][j][x]$ vào số cách $f[i][j][x+1]$.

Đáp án cuối cùng là tổng của mọi $f[n][j][x]$.

Độ phức tạp thời gian là $O(n \times k^2 \times M)$ và độ phức tạp không gian là $O(n \times k \times M)$. Trong đó, $k$ là số mặt xúc xắc và $M$ là số lần tối đa một mặt có thể xuất hiện liên tiếp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def dieSimulator(self, n: int, rollMax: List[int]) -> int:
        f = [[[0] * 16 for _ in range(7)] for _ in range(n + 1)]
        for j in range(1, 7):
            f[1][j][1] = 1
        for i in range(2, n + 1):
            for j in range(1, 7):
                for x in range(1, rollMax[j - 1] + 1):
                    for k in range(1, 7):
                        if k != j:
                            f[i][k][1] += f[i - 1][j][x]
                        elif x + 1 <= rollMax[j - 1]:
                            f[i][j][x + 1] += f[i - 1][j][x]
        mod = 10**9 + 7
        ans = 0
        for j in range(1, 7):
            for x in range(1, rollMax[j - 1] + 1):
                ans = (ans + f[n][j][x]) % mod
        return ans
```

#### Java

```java
class Solution {
    public int dieSimulator(int n, int[] rollMax) {
        int[][][] f = new int[n + 1][7][16];
        for (int j = 1; j <= 6; ++j) {
            f[1][j][1] = 1;
        }
        final int mod = (int) 1e9 + 7;
        for (int i = 2; i <= n; ++i) {
            for (int j = 1; j <= 6; ++j) {
                for (int x = 1; x <= rollMax[j - 1]; ++x) {
                    for (int k = 1; k <= 6; ++k) {
                        if (k != j) {
                            f[i][k][1] = (f[i][k][1] + f[i - 1][j][x]) % mod;
                        } else if (x + 1 <= rollMax[j - 1]) {
                            f[i][j][x + 1] = (f[i][j][x + 1] + f[i - 1][j][x]) % mod;
                        }
                    }
                }
            }
        }
        int ans = 0;
        for (int j = 1; j <= 6; ++j) {
            for (int x = 1; x <= rollMax[j - 1]; ++x) {
                ans = (ans + f[n][j][x]) % mod;
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
    int dieSimulator(int n, vector<int>& rollMax) {
        int f[n + 1][7][16];
        memset(f, 0, sizeof f);
        for (int j = 1; j <= 6; ++j) {
            f[1][j][1] = 1;
        }
        const int mod = 1e9 + 7;
        for (int i = 2; i <= n; ++i) {
            for (int j = 1; j <= 6; ++j) {
                for (int x = 1; x <= rollMax[j - 1]; ++x) {
                    for (int k = 1; k <= 6; ++k) {
                        if (k != j) {
                            f[i][k][1] = (f[i][k][1] + f[i - 1][j][x]) % mod;
                        } else if (x + 1 <= rollMax[j - 1]) {
                            f[i][j][x + 1] = (f[i][j][x + 1] + f[i - 1][j][x]) % mod;
                        }
                    }
                }
            }
        }
        int ans = 0;
        for (int j = 1; j <= 6; ++j) {
            for (int x = 1; x <= rollMax[j - 1]; ++x) {
                ans = (ans + f[n][j][x]) % mod;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func dieSimulator(n int, rollMax []int) (ans int) {
	f := make([][7][16]int, n+1)
	for j := 1; j <= 6; j++ {
		f[1][j][1] = 1
	}
	const mod = 1e9 + 7
	for i := 2; i <= n; i++ {
		for j := 1; j <= 6; j++ {
			for x := 1; x <= rollMax[j-1]; x++ {
				for k := 1; k <= 6; k++ {
					if k != j {
						f[i][k][1] = (f[i][k][1] + f[i-1][j][x]) % mod
					} else if x+1 <= rollMax[j-1] {
						f[i][j][x+1] = (f[i][j][x+1] + f[i-1][j][x]) % mod
					}
				}
			}
		}
	}
	for j := 1; j <= 6; j++ {
		for x := 1; x <= rollMax[j-1]; x++ {
			ans = (ans + f[n][j][x]) % mod
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
