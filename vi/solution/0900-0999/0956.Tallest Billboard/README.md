---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
    - Knapsack
    - 0-1 Knapsack
    - Meet in the Middle
---

<!-- problem:start -->

# [956. Tallest Billboard](https://leetcode.com/problems/tallest-billboard)

[中文文档](/solution/0900-0999/0956.Tallest%20Billboard/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang dựng một biển quảng cáo và muốn biển cao nhất có thể. Biển được đỡ bởi hai trụ thép ở hai bên, và hai trụ phải có cùng chiều cao.</p>

<p>Cho một tập các thanh <code>rods</code> có thể hàn với nhau. Ví dụ, nếu có các thanh dài <code>1</code>, <code>2</code> và <code>3</code>, bạn có thể hàn chúng lại để tạo thành trụ dài <code>6</code>.</p>

<p>Trả về <em>chiều cao lớn nhất có thể của biển quảng cáo</em>. Nếu không thể dựng trụ đỡ biển, trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> rods = [1,2,3,6]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Ta có hai tập con rời nhau {1,2,3} và {6}, với tổng bằng nhau = 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> rods = [1,2,3,4,5,6]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Ta có hai tập con rời nhau {2,3,5} và {4,6}, với tổng bằng nhau = 10.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> rods = [1,2]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không thể dựng trụ đỡ biển quảng cáo nên ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= rods.length &lt;= 20</code></li>
	<li><code>1 &lt;= rods[i] &lt;= 1000</code></li>
	<li><code>sum(rods[i]) &lt;= 5000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thanh được đặt về bên trái, bên phải hoặc bỏ đi; hai bên cần cao bằng nhau và đạt chiều cao lớn nhất. Có quá nhiều cách gán để thử hết $3^n$. Trạng thái hữu ích là chiều cao chung lớn nhất sau khi xét thanh $i$ với độ chênh hiện tại $j$. Ba chuyển trạng thái tương ứng với ba lựa chọn; độ chênh bằng $0$ cho biết ta đã đạt được chiều cao hợp lệ. Memoization giúp tránh tính lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def tallestBillboard(self, rods: List[int]) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if i >= len(rods):
                return 0 if j == 0 else -inf
            ans = max(dfs(i + 1, j), dfs(i + 1, j + rods[i]))
            ans = max(ans, dfs(i + 1, abs(rods[i] - j)) + min(j, rods[i]))
            return ans

        return dfs(0, 0)
```

#### Java

```java
class Solution {
    private Integer[][] f;
    private int[] rods;
    private int n;

    public int tallestBillboard(int[] rods) {
        int s = 0;
        for (int x : rods) {
            s += x;
        }
        n = rods.length;
        this.rods = rods;
        f = new Integer[n][s + 1];
        return dfs(0, 0);
    }

    private int dfs(int i, int j) {
        if (i >= n) {
            return j == 0 ? 0 : -(1 << 30);
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        int ans = Math.max(dfs(i + 1, j), dfs(i + 1, j + rods[i]));
        ans = Math.max(ans, dfs(i + 1, Math.abs(rods[i] - j)) + Math.min(j, rods[i]));
        return f[i][j] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int tallestBillboard(vector<int>& rods) {
        int s = accumulate(rods.begin(), rods.end(), 0);
        int n = rods.size();
        int f[n][s + 1];
        memset(f, -1, sizeof(f));
        function<int(int, int)> dfs = [&](int i, int j) -> int {
            if (i >= n) {
                return j == 0 ? 0 : -(1 << 30);
            }
            if (f[i][j] != -1) {
                return f[i][j];
            }
            int ans = max(dfs(i + 1, j), dfs(i + 1, j + rods[i]));
            ans = max(ans, dfs(i + 1, abs(j - rods[i])) + min(j, rods[i]));
            return f[i][j] = ans;
        };
        return dfs(0, 0);
    }
};
```

#### Go

```go
func tallestBillboard(rods []int) int {
	s := 0
	for _, x := range rods {
		s += x
	}
	n := len(rods)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, s+1)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i >= n {
			if j == 0 {
				return 0
			}
			return -(1 << 30)
		}
		if f[i][j] != -1 {
			return f[i][j]
		}
		ans := max(dfs(i+1, j), dfs(i+1, j+rods[i]))
		ans = max(ans, dfs(i+1, abs(j-rods[i]))+min(j, rods[i]))
		f[i][j] = ans
		return ans
	}
	return dfs(0, 0)
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function tallestBillboard(rods: number[]): number {
    const s = rods.reduce((a, b) => a + b, 0);
    const n = rods.length;
    const f = new Array(n).fill(0).map(() => new Array(s + 1).fill(-1));
    const dfs = (i: number, j: number): number => {
        if (i >= n) {
            return j === 0 ? 0 : -(1 << 30);
        }
        if (f[i][j] !== -1) {
            return f[i][j];
        }
        let ans = Math.max(dfs(i + 1, j), dfs(i + 1, j + rods[i]));
        ans = Math.max(ans, dfs(i + 1, Math.abs(j - rods[i])) + Math.min(j, rods[i]));
        return (f[i][j] = ans);
    };
    return dfs(0, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Ta chuyển sang quy hoạch động bottom-up: $f[i][j]$ là chiều cao chung lớn nhất khi dùng $i$ thanh đầu tiên và có độ chênh $j$. Các chuyển trạng thái giống cách đệ quy, nhưng không tốn chi phí gọi hàm. Đáp án là $f[n][0]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def tallestBillboard(self, rods: List[int]) -> int:
        n = len(rods)
        s = sum(rods)
        f = [[-inf] * (s + 1) for _ in range(n + 1)]
        f[0][0] = 0
        t = 0
        for i, x in enumerate(rods, 1):
            t += x
            for j in range(t + 1):
                f[i][j] = f[i - 1][j]
                if j >= x:
                    f[i][j] = max(f[i][j], f[i - 1][j - x])
                if j + x <= t:
                    f[i][j] = max(f[i][j], f[i - 1][j + x] + x)
                if j < x:
                    f[i][j] = max(f[i][j], f[i - 1][x - j] + x - j)
        return f[n][0]
```

#### Java

```java
class Solution {
    public int tallestBillboard(int[] rods) {
        int n = rods.length;
        int s = 0;
        for (int x : rods) {
            s += x;
        }
        int[][] f = new int[n + 1][s + 1];
        for (var e : f) {
            Arrays.fill(e, -(1 << 30));
        }
        f[0][0] = 0;
        for (int i = 1, t = 0; i <= n; ++i) {
            int x = rods[i - 1];
            t += x;
            for (int j = 0; j <= t; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= x) {
                    f[i][j] = Math.max(f[i][j], f[i - 1][j - x]);
                }
                if (j + x <= t) {
                    f[i][j] = Math.max(f[i][j], f[i - 1][j + x] + x);
                }
                if (j < x) {
                    f[i][j] = Math.max(f[i][j], f[i - 1][x - j] + x - j);
                }
            }
        }
        return f[n][0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int tallestBillboard(vector<int>& rods) {
        int n = rods.size();
        int s = accumulate(rods.begin(), rods.end(), 0);
        int f[n + 1][s + 1];
        memset(f, -0x3f, sizeof(f));
        f[0][0] = 0;
        for (int i = 1, t = 0; i <= n; ++i) {
            int x = rods[i - 1];
            t += x;
            for (int j = 0; j <= t; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= x) {
                    f[i][j] = max(f[i][j], f[i - 1][j - x]);
                }
                if (j + x <= t) {
                    f[i][j] = max(f[i][j], f[i - 1][j + x] + x);
                }
                if (j < x) {
                    f[i][j] = max(f[i][j], f[i - 1][x - j] + x - j);
                }
            }
        }
        return f[n][0];
    }
};
```

#### Go

```go
func tallestBillboard(rods []int) int {
	n := len(rods)
	s := 0
	for _, x := range rods {
		s += x
	}
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, s+1)
		for j := range f[i] {
			f[i][j] = -(1 << 30)
		}
	}
	f[0][0] = 0
	for i, t := 1, 0; i <= n; i++ {
		x := rods[i-1]
		t += x
		for j := 0; j <= t; j++ {
			f[i][j] = f[i-1][j]
			if j >= x {
				f[i][j] = max(f[i][j], f[i-1][j-x])
			}
			if j+x <= t {
				f[i][j] = max(f[i][j], f[i-1][j+x]+x)
			}
			if j < x {
				f[i][j] = max(f[i][j], f[i-1][x-j]+x-j)
			}
		}
	}
	return f[n][0]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
