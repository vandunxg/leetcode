---
comments: true
difficulty: Medium
rating: 1808
source: Biweekly Contest 11 Q3
tags:
    - Array
    - Math
    - Dynamic Programming
    - Probability and Statistics
---

<!-- problem:start -->

# [1230. Toss Strange Coins 🔒](https://leetcode.com/problems/toss-strange-coins)

[中文文档](/solution/1200-1299/1230.Toss%20Strange%20Coins/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một số đồng xu. Khi tung đồng xu thứ <code>i</code>, xác suất xuất hiện mặt ngửa là <code>prob[i]</code>.</p>

<p>Hãy trả về xác suất có đúng <code>target</code> đồng xu xuất hiện mặt ngửa khi mỗi đồng xu được tung đúng một lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> prob = [0.4], target = 1
<strong>Đầu ra:</strong> 0.40000
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> prob = [0.5,0.5,0.5,0.5,0.5], target = 0
<strong>Đầu ra:</strong> 0.03125
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= prob.length &lt;= 1000</code></li>
	<li><code>0 &lt;= prob[i] &lt;= 1</code></li>
	<li><code>0 &lt;= target&nbsp;</code><code>&lt;= prob.length</code></li>
	<li>Đáp án được chấp nhận nếu sai số so với kết quả đúng không vượt quá <code>10^-5</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi đồng xu có xác suất xuất hiện mặt ngửa khác nhau; ta cần đúng $target$ mặt ngửa. Vì $n \le 1000$, không thể liệt kê mọi tập con. Xác suất có $j$ mặt ngửa trong $i$ đồng xu đầu chỉ phụ thuộc vào $i-1$ đồng xu trước đó: mặt sấp giữ nguyên số mặt ngửa, còn mặt ngửa được tính từ trạng thái có $j-1$ mặt ngửa.
>
> Đặt $f[i][j]$ là xác suất đó, với $f[0][0]=1$. Tính lần lượt theo thứ tự các đồng xu sẽ cho $f[n][target]$. Bảng DP triển khai phép chập xác suất của các lần tung độc lập.

<!-- thinking:end -->

Ta đặt $f[i][j]$ là xác suất có $j$ đồng xu xuất hiện mặt ngửa trong $i$ đồng xu đầu, với $f[0][0]=1$. Đáp án là $f[n][target]$.

Xét $f[i][j]$, với $i \geq 1$. Nếu đồng xu hiện tại xuất hiện mặt sấp, ta có $f[i][j] = (1 - p) \times f[i - 1][j]$. Nếu đồng xu hiện tại xuất hiện mặt ngửa và $j \gt 0$, ta có $f[i][j] = p \times f[i - 1][j - 1]$. Do đó, công thức chuyển trạng thái là:

$$
f[i][j] = \begin{cases}
(1 - p) \times f[i - 1][j], & j = 0 \\
(1 - p) \times f[i - 1][j] + p \times f[i - 1][j - 1], & j \gt 0
\end{cases}
$$

trong đó $p$ là xác suất đồng xu thứ $i$ xuất hiện mặt ngửa.

Độ phức tạp thời gian là $O(n \times target)$ và độ phức tạp không gian là $O(n \times target)$, trong đó $n$ là số đồng xu.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def probabilityOfHeads(self, prob: List[float], target: int) -> float:
        n = len(prob)
        f = [[0] * (target + 1) for _ in range(n + 1)]
        f[0][0] = 1
        for i, p in enumerate(prob, 1):
            for j in range(min(i, target) + 1):
                f[i][j] = (1 - p) * f[i - 1][j]
                if j:
                    f[i][j] += p * f[i - 1][j - 1]
        return f[n][target]
```

#### Java

```java
class Solution {
    public double probabilityOfHeads(double[] prob, int target) {
        int n = prob.length;
        double[][] f = new double[n + 1][target + 1];
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j <= Math.min(i, target); ++j) {
                f[i][j] = (1 - prob[i - 1]) * f[i - 1][j];
                if (j > 0) {
                    f[i][j] += prob[i - 1] * f[i - 1][j - 1];
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
    double probabilityOfHeads(vector<double>& prob, int target) {
        int n = prob.size();
        double f[n + 1][target + 1];
        memset(f, 0, sizeof(f));
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j <= min(i, target); ++j) {
                f[i][j] = (1 - prob[i - 1]) * f[i - 1][j];
                if (j > 0) {
                    f[i][j] += prob[i - 1] * f[i - 1][j - 1];
                }
            }
        }
        return f[n][target];
    }
};
```

#### Go

```go
func probabilityOfHeads(prob []float64, target int) float64 {
	n := len(prob)
	f := make([][]float64, n+1)
	for i := range f {
		f[i] = make([]float64, target+1)
	}
	f[0][0] = 1
	for i := 1; i <= n; i++ {
		for j := 0; j <= i && j <= target; j++ {
			f[i][j] = (1 - prob[i-1]) * f[i-1][j]
			if j > 0 {
				f[i][j] += prob[i-1] * f[i-1][j-1]
			}
		}
	}
	return f[n][target]
}
```

#### TypeScript

```ts
function probabilityOfHeads(prob: number[], target: number): number {
    const n = prob.length;
    const f = new Array(n + 1).fill(0).map(() => new Array(target + 1).fill(0));
    f[0][0] = 1;
    for (let i = 1; i <= n; ++i) {
        for (let j = 0; j <= target; ++j) {
            f[i][j] = f[i - 1][j] * (1 - prob[i - 1]);
            if (j) {
                f[i][j] += f[i - 1][j - 1] * prob[i - 1];
            }
        }
    }
    return f[n][target];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Dynamic Programming (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Ở lời giải 1, hàng $i$ chỉ đọc hàng trước đó. Ta có thể dùng mảng một chiều và cập nhật $j$ từ lớn xuống nhỏ để không ghi đè $f[j-1]$ trước khi dùng giá trị này cho trường hợp đồng xu ra mặt ngửa. Độ phức tạp không gian giảm còn $O(target)$; công thức truy hồi không đổi.

<!-- thinking:end -->

$f[i][j]$ chỉ phụ thuộc vào hàng trước đó. Cập nhật $j$ từ lớn xuống nhỏ để giảm độ phức tạp không gian còn $O(target)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def probabilityOfHeads(self, prob: List[float], target: int) -> float:
        f = [0] * (target + 1)
        f[0] = 1
        for p in prob:
            for j in range(target, -1, -1):
                f[j] *= 1 - p
                if j:
                    f[j] += p * f[j - 1]
        return f[target]
```

#### Java

```java
class Solution {
    public double probabilityOfHeads(double[] prob, int target) {
        double[] f = new double[target + 1];
        f[0] = 1;
        for (double p : prob) {
            for (int j = target; j >= 0; --j) {
                f[j] *= (1 - p);
                if (j > 0) {
                    f[j] += p * f[j - 1];
                }
            }
        }
        return f[target];
    }
}
```

#### C++

```cpp
class Solution {
public:
    double probabilityOfHeads(vector<double>& prob, int target) {
        double f[target + 1];
        memset(f, 0, sizeof(f));
        f[0] = 1;
        for (double p : prob) {
            for (int j = target; j >= 0; --j) {
                f[j] *= (1 - p);
                if (j > 0) {
                    f[j] += p * f[j - 1];
                }
            }
        }
        return f[target];
    }
};
```

#### Go

```go
func probabilityOfHeads(prob []float64, target int) float64 {
	f := make([]float64, target+1)
	f[0] = 1
	for _, p := range prob {
		for j := target; j >= 0; j-- {
			f[j] *= (1 - p)
			if j > 0 {
				f[j] += p * f[j-1]
			}
		}
	}
	return f[target]
}
```

#### TypeScript

```ts
function probabilityOfHeads(prob: number[], target: number): number {
    const f = new Array(target + 1).fill(0);
    f[0] = 1;
    for (const p of prob) {
        for (let j = target; j >= 0; --j) {
            f[j] *= 1 - p;
            if (j > 0) {
                f[j] += f[j - 1] * p;
            }
        }
    }
    return f[target];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
