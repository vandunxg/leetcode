---
comments: true
difficulty: Medium
tags:
    - Math
    - Dynamic Programming
    - Sliding Window
    - Probability and Statistics
---

<!-- problem:start -->

# [837. New 21 Game](https://leetcode.com/problems/new-21-game)

[中文文档](/solution/0800-0899/0837.New%2021%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Alice chơi trò chơi sau, lấy cảm hứng từ trò bài <strong>&quot;21&quot;</strong>.</p>

<p>Alice bắt đầu với <code>0</code> điểm và rút số khi có ít hơn <code>k</code> điểm. Mỗi lần rút, cô nhận ngẫu nhiên một số điểm nguyên trong phạm vi <code>[1, maxPts]</code>, với <code>maxPts</code> là số nguyên. Các lần rút độc lập và mỗi kết quả có xác suất như nhau.</p>

<p>Alice dừng rút khi đạt <code>k</code> điểm <strong>trở lên</strong>.</p>

<p>Trả về xác suất Alice có không quá <code>n</code> điểm.</p>

<p>Đáp án được chấp nhận nếu sai lệch không quá <code>10<sup>-5</sup></code> so với kết quả thực tế.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10, k = 1, maxPts = 10
<strong>Đầu ra:</strong> 1.00000
<strong>Giải thích:</strong> Alice rút một lá bài rồi dừng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6, k = 1, maxPts = 10
<strong>Đầu ra:</strong> 0.60000
<strong>Giải thích:</strong> Alice rút một lá bài rồi dừng.
Trong 10 khả năng, có 6 trường hợp cô đạt không quá 6 điểm.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 21, k = 17, maxPts = 10
<strong>Đầu ra:</strong> 0.73278
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= k &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= maxPts &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lượt ta cộng một số nguyên có phân phối đều trong $[1,\textit{maxPts}]$ cho đến khi tổng đạt $k$; cần tính xác suất tổng vẫn $\le n$. Không thể xét mọi chuỗi rút khi $k,n\le 10^4$, nên ghi nhớ kết quả theo tổng điểm hiện tại.
>
> $dfs(i)$ là xác suất đạt yêu cầu khi tổng điểm hiện tại là $i$. Với $i\ge k$, ta dừng và kiểm tra $i\le n$; trường hợp $i=k-1$ có công thức trực tiếp. Các trạng thái còn lại dùng hiệu giữa hai trạng thái lân cận để mỗi bước chuyển có độ phức tạp $O(1)$ thay vì $O(\textit{maxPts})$.

<!-- thinking:end -->

Ta định nghĩa hàm $dfs(i)$ biểu diễn xác suất điểm cuối cùng không vượt quá $n$ khi điểm hiện tại là $i$ và Alice dừng rút. Đáp án là $dfs(0)$.

Ta tính $dfs(i)$ như sau:

- Nếu $i \ge k$, Alice dừng rút. Nếu $i \le n$ thì trả về $1$, ngược lại trả về $0;
- Nếu chưa dừng, ta có thể rút số điểm tiếp theo $j$ trong phạm vi $[1,..\textit{maxPts}]$, khi đó $dfs(i) = \frac{1}{maxPts} \sum_{j=1}^{maxPts} dfs(i+j)$.

Có thể dùng tìm kiếm có ghi nhớ để tăng tốc phép tính.

Độ phức tạp thời gian của cách trên là $O(k \times \textit{maxPts})$, vượt quá giới hạn thời gian nên cần tối ưu.

Khi $i \lt k$, ta có phương trình sau:

$$
\begin{aligned}
dfs(i) &= (dfs(i + 1) + dfs(i + 2) + \cdots + dfs(i + \textit{maxPts})) / \textit{maxPts} & (1)
\end{aligned}
$$

Khi $i \lt k - 1$, ta có phương trình sau:

$$
\begin{aligned}
dfs(i+1) &= (dfs(i + 2) + dfs(i + 3) + \cdots + dfs(i + \textit{maxPts} + 1)) / \textit{maxPts} & (2)
\end{aligned}
$$

Vì vậy, khi $i \lt k-1$, lấy phương trình $(2)$ trừ khỏi phương trình $(1)$, ta được:

$$
\begin{aligned}
dfs(i) - dfs(i+1) &= (dfs(i + 1) - dfs(i + \textit{maxPts} + 1)) / \textit{maxPts}
\end{aligned}
$$

Tức là:

$$
\begin{aligned}
dfs(i) &= dfs(i + 1) + (dfs(i + 1) - dfs(i + \textit{maxPts} + 1)) / \textit{maxPts}
\end{aligned}
$$

Nếu $i=k-1$, ta có:

$$
\begin{aligned}
dfs(i) &= dfs(k - 1) = (dfs(k) + dfs(k + 1) + \cdots + dfs(k + \textit{maxPts} - 1)) / \textit{maxPts} & (3)
\end{aligned}
$$

Giả sử có $i$ số không vượt quá $n$, khi đó $k+i-1 \leq n$. Vì $i\leq \textit{maxPts}$ nên $i \leq \min(n-k+1, \textit{maxPts})$, do đó có thể viết lại phương trình $(3)$ thành:

$$
\begin{aligned}
dfs(k-1) &= \min(n-k+1, \textit{maxPts}) / \textit{maxPts}
\end{aligned}
$$

Tóm lại, phương trình chuyển trạng thái là:

$$
\begin{aligned}
dfs(i) &= \begin{cases}
1, & i \geq k, i \leq n \\
0, & i \geq k, i \gt n \\
\min(n-k+1, \textit{maxPts}) / \textit{maxPts}, & i = k - 1 \\
dfs(i + 1) + (dfs(i + 1) - dfs(i + \textit{maxPts} + 1)) / \textit{maxPts}, & i < k - 1
\end{cases}
\end{aligned}
$$

Độ phức tạp thời gian là $O(k + \textit{maxPts})$ và độ phức tạp không gian là $O(k + \textit{maxPts})$, trong đó $k$ là số điểm tối đa.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def new21Game(self, n: int, k: int, maxPts: int) -> float:
        @cache
        def dfs(i: int) -> float:
            if i >= k:
                return int(i <= n)
            if i == k - 1:
                return min(n - k + 1, maxPts) / maxPts
            return dfs(i + 1) + (dfs(i + 1) - dfs(i + maxPts + 1)) / maxPts

        return dfs(0)
```

#### Java

```java
class Solution {
    private double[] f;
    private int n, k, maxPts;

    public double new21Game(int n, int k, int maxPts) {
        f = new double[k];
        this.n = n;
        this.k = k;
        this.maxPts = maxPts;
        return dfs(0);
    }

    private double dfs(int i) {
        if (i >= k) {
            return i <= n ? 1 : 0;
        }
        if (i == k - 1) {
            return Math.min(n - k + 1, maxPts) * 1.0 / maxPts;
        }
        if (f[i] != 0) {
            return f[i];
        }
        return f[i] = dfs(i + 1) + (dfs(i + 1) - dfs(i + maxPts + 1)) / maxPts;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double new21Game(int n, int k, int maxPts) {
        vector<double> f(k);
        auto dfs = [&](this auto&& dfs, int i) -> double {
            if (i >= k) {
                return i <= n ? 1 : 0;
            }
            if (i == k - 1) {
                return min(n - k + 1, maxPts) * 1.0 / maxPts;
            }
            if (f[i]) {
                return f[i];
            }
            return f[i] = dfs(i + 1) + (dfs(i + 1) - dfs(i + maxPts + 1)) / maxPts;
        };
        return dfs(0);
    }
};
```

#### Go

```go
func new21Game(n int, k int, maxPts int) float64 {
	f := make([]float64, k)
	var dfs func(int) float64
	dfs = func(i int) float64 {
		if i >= k {
			if i <= n {
				return 1
			}
			return 0
		}
		if i == k-1 {
			return float64(min(n-k+1, maxPts)) / float64(maxPts)
		}
		if f[i] > 0 {
			return f[i]
		}
		f[i] = dfs(i+1) + (dfs(i+1)-dfs(i+maxPts+1))/float64(maxPts)
		return f[i]
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function new21Game(n: number, k: number, maxPts: number): number {
    const f: number[] = Array(k).fill(0);
    const dfs = (i: number): number => {
        if (i >= k) {
            return i <= n ? 1 : 0;
        }
        if (i === k - 1) {
            return Math.min(n - k + 1, maxPts) / maxPts;
        }
        if (f[i] !== 0) {
            return f[i];
        }
        return (f[i] = dfs(i + 1) + (dfs(i + 1) - dfs(i + maxPts + 1)) / maxPts);
    };
    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Memoization vẫn dùng đệ quy. Có thể tính cùng công thức theo chiều giảm dần từ $k-2$: $f[i]$ được suy ra từ $f[i+1]$ và phần tử ở cuối cửa sổ $f[i+\textit{maxPts}+1]$.
>
> Các trạng thái kết thúc trong $[k,\min(n,k+\textit{maxPts}))$ có giá trị $1$. Đáp án là $f[0]$, có thể tính trong thời gian tuyến tính.

<!-- thinking:end -->

Ta có thể chuyển cách tìm kiếm có ghi nhớ ở Lời giải 1 thành dynamic programming.

Định nghĩa $f[i]$ là xác suất điểm cuối cùng không vượt quá $n$ khi điểm hiện tại là $i$ và Alice dừng rút. Đáp án là $f[0]$.

Khi $k \leq i \leq \min(n, k + \textit{maxPts} - 1)$, ta có $f[i] = 1$.

Khi $i = k - 1$, ta có $f[i] = \min(n-k+1, \textit{maxPts}) / \textit{maxPts}$.

When $i \lt k - 1$, we have $f[i] = f[i + 1] + (f[i + 1] - f[i + \textit{maxPts} + 1]) / \textit{maxPts}$.

Độ phức tạp thời gian là $O(k + \textit{maxPts})$ và độ phức tạp không gian là $O(k + \textit{maxPts})$, trong đó $k$ là số điểm tối đa.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def new21Game(self, n: int, k: int, maxPts: int) -> float:
        f = [0] * (k + maxPts)
        for i in range(k, min(n + 1, k + maxPts)):
            f[i] = 1
        f[k - 1] = min(n - k + 1, maxPts) / maxPts
        for i in range(k - 2, -1, -1):
            f[i] = f[i + 1] + (f[i + 1] - f[i + maxPts + 1]) / maxPts
        return f[0]
```

#### Java

```java
class Solution {
    public double new21Game(int n, int k, int maxPts) {
        if (k == 0) {
            return 1.0;
        }
        double[] f = new double[k + maxPts];
        for (int i = k; i < Math.min(n + 1, k + maxPts); ++i) {
            f[i] = 1;
        }
        f[k - 1] = Math.min(n - k + 1, maxPts) * 1.0 / maxPts;
        for (int i = k - 2; i >= 0; --i) {
            f[i] = f[i + 1] + (f[i + 1] - f[i + maxPts + 1]) / maxPts;
        }
        return f[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    double new21Game(int n, int k, int maxPts) {
        if (k == 0) {
            return 1.0;
        }
        double f[k + maxPts];
        memset(f, 0, sizeof(f));
        for (int i = k; i < min(n + 1, k + maxPts); ++i) {
            f[i] = 1;
        }
        f[k - 1] = min(n - k + 1, maxPts) * 1.0 / maxPts;
        for (int i = k - 2; i >= 0; --i) {
            f[i] = f[i + 1] + (f[i + 1] - f[i + maxPts + 1]) / maxPts;
        }
        return f[0];
    }
};
```

#### Go

```go
func new21Game(n int, k int, maxPts int) float64 {
	if k == 0 {
		return 1
	}
	f := make([]float64, k+maxPts)
	for i := k; i < min(n+1, k+maxPts); i++ {
		f[i] = 1
	}
	f[k-1] = float64(min(n-k+1, maxPts)) / float64(maxPts)
	for i := k - 2; i >= 0; i-- {
		f[i] = f[i+1] + (f[i+1]-f[i+maxPts+1])/float64(maxPts)
	}
	return f[0]
}
```

#### TypeScript

```ts
function new21Game(n: number, k: number, maxPts: number): number {
    if (k === 0) {
        return 1;
    }
    const f: number[] = Array(k + maxPts).fill(0);
    for (let i = k; i < Math.min(n + 1, k + maxPts); ++i) {
        f[i] = 1;
    }
    f[k - 1] = Math.min(n - k + 1, maxPts) / maxPts;
    for (let i = k - 2; i >= 0; --i) {
        f[i] = f[i + 1] + (f[i + 1] - f[i + maxPts + 1]) / maxPts;
    }
    return f[0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
