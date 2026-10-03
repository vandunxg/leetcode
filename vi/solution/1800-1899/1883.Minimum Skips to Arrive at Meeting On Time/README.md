---
comments: true
difficulty: Hard
rating: 2587
source: Weekly Contest 243 Q4
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1883. Minimum Skips to Arrive at Meeting On Time](https://leetcode.com/problems/minimum-skips-to-arrive-at-meeting-on-time)

[中文文档](/solution/1800-1899/1883.Minimum%20Skips%20to%20Arrive%20at%20Meeting%20On%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>hoursBefore</code>, là số giờ bạn có để đi đến cuộc họp. Để đến cuộc họp, bạn phải đi qua <code>n</code> con đường. Độ dài các con đường được cho bởi mảng số nguyên <code>dist</code> có độ dài <code>n</code>, trong đó <code>dist[i]</code> mô tả độ dài của con đường thứ <code>i<sup>th</sup></code> <strong>tính bằng ki-lô-mét</strong>. Ngoài ra, cho số nguyên <code>speed</code>, là tốc độ bạn di chuyển <strong>tính bằng km/h</strong>.</p>

<p>Sau khi đi qua con đường <code>i</code>, bạn phải nghỉ và chờ đến <strong>giờ nguyên tiếp theo</strong> trước khi bắt đầu đi trên con đường kế tiếp. Lưu ý rằng bạn không cần nghỉ sau khi đi qua con đường cuối cùng vì lúc đó bạn đã đến cuộc họp.</p>

<ul>
	<li>Ví dụ, nếu đi qua một con đường mất <code>1.4</code> giờ, bạn phải chờ đến mốc <code>2</code> giờ mới được đi trên con đường tiếp theo. Nếu đi qua một con đường mất đúng <code>2</code> giờ, bạn không cần chờ.</li>
</ul>

<p>Tuy nhiên, bạn được phép <strong>bỏ qua</strong> một số lần nghỉ để có thể đến đúng giờ, nghĩa là bạn không cần chờ đến giờ nguyên tiếp theo. Điều này có nghĩa là bạn có thể hoàn thành các con đường sau đó ở những mốc giờ khác nhau.</p>

<ul>
	<li>Ví dụ, giả sử đi qua con đường đầu tiên mất <code>1.4</code> giờ và con đường thứ hai mất <code>0.6</code> giờ. Bỏ qua lần nghỉ sau con đường đầu tiên sẽ khiến bạn hoàn thành con đường thứ hai đúng vào mốc <code>2</code> giờ, nhờ đó có thể bắt đầu ngay con đường thứ ba.</li>
</ul>

<p>Trả về <em><strong>số lần bỏ qua nhỏ nhất cần thiết</strong> để đến cuộc họp đúng giờ, hoặc </em><code>-1</code><em> nếu <strong>không thể</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> dist = [1,3,2], speed = 4, hoursBefore = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Nếu không bỏ qua lần nghỉ nào, bạn sẽ đến nơi sau (1/4 + 3/4) + (3/4 + 1/4) + (2/4) = 2.5 giờ.
Bạn có thể bỏ qua lần nghỉ đầu tiên để đến nơi sau ((1/4 + <u>0</u>) + (3/4 + 0)) + (2/4) = 1.5 giờ.
Lưu ý rằng lần nghỉ thứ hai được rút ngắn vì bạn hoàn thành con đường thứ hai vào một giờ nguyên do đã bỏ qua lần nghỉ đầu tiên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> dist = [7,3,5,5], speed = 2, hoursBefore = 10
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Nếu không bỏ qua lần nghỉ nào, bạn sẽ đến nơi sau (7/2 + 1/2) + (3/2 + 1/2) + (5/2 + 1/2) + (5/2) = 11.5 giờ.
Bạn có thể bỏ qua lần nghỉ thứ nhất và thứ ba để đến nơi sau ((7/2 + <u>0</u>) + (3/2 + 0)) + ((5/2 + <u>0</u>) + (5/2)) = 10 giờ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> dist = [7,3,5,5], speed = 1, hoursBefore = 10
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể đến cuộc họp đúng giờ ngay cả khi bỏ qua tất cả lần nghỉ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == dist.length</code></li>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= dist[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= speed &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= hoursBefore &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ngoại trừ con đường cuối cùng, nếu kết thúc vào một giờ không nguyên thì ta buộc phải chờ. Ta có thể bỏ qua một số lần chờ và muốn hoàn thành trước $hoursBefore$ với ít lần bỏ qua nhất. Việc chọn một tập con các con đường là hàm mũ.
>
> $f[i][j]$ là thời gian sớm nhất sau $i$ con đường và $j$ lần bỏ qua: nếu không bỏ qua, ta làm tròn lên $f[i-1][j]+d_i/s$; nếu bỏ qua, ta cộng thời gian thực. $eps$ dùng để tránh lỗi số thực khi làm tròn lên. Giá trị $j$ nhỏ nhất sao cho $f[n][j]$ không vượt giới hạn là đáp án.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là thời gian ngắn nhất khi xét $i$ con đường đầu tiên và bỏ qua đúng $j$ lần nghỉ. Ban đầu, $f[0][0]=0$, các giá trị còn lại $f[i][j]=\infty$.

Vì ta có thể chọn bỏ qua hoặc không bỏ qua thời gian nghỉ sau con đường thứ $i$, ta có phương trình chuyển trạng thái:

$$
f[i][j]=\min\left\{\begin{aligned} \lceil f[i-1][j]+\frac{d_i}{s}\rceil & \textit{Do not skip the rest time of the $i$-th road} \\ f[i-1][j-1]+\frac{d_i}{s} & \textit{Skip the rest time of the $i$-th road} \end{aligned}\right.
$$

Trong đó, $\lceil x\rceil$ biểu diễn phép làm tròn $x$ lên. Cần lưu ý rằng vì phải đảm bảo bỏ qua đúng $j$ lần nghỉ, ta có $j\le i$; hơn nữa, nếu $j=0$ thì không thể bỏ qua lần nghỉ nào.

Do các phép tính số thực và làm tròn lên có thể gây sai số, ta đưa vào hằng số $eps = 10^{-8}$ để biểu diễn một số thực dương rất nhỏ. Ta trừ $eps$ trước khi làm tròn số thực lên, và cuối cùng khi so sánh $f[n][j]$ với $hoursBefore$, ta cần cộng thêm $eps$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số con đường.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSkips(self, dist: List[int], speed: int, hoursBefore: int) -> int:
        n = len(dist)
        f = [[inf] * (n + 1) for _ in range(n + 1)]
        f[0][0] = 0
        eps = 1e-8
        for i, x in enumerate(dist, 1):
            for j in range(i + 1):
                if j < i:
                    f[i][j] = min(f[i][j], ceil(f[i - 1][j] + x / speed - eps))
                if j:
                    f[i][j] = min(f[i][j], f[i - 1][j - 1] + x / speed)
        for j in range(n + 1):
            if f[n][j] <= hoursBefore + eps:
                return j
        return -1
```

#### Java

```java
class Solution {
    public int minSkips(int[] dist, int speed, int hoursBefore) {
        int n = dist.length;
        double[][] f = new double[n + 1][n + 1];
        for (int i = 0; i <= n; i++) {
            Arrays.fill(f[i], 1e20);
        }
        f[0][0] = 0;
        double eps = 1e-8;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j <= i; ++j) {
                if (j < i) {
                    f[i][j] = Math.min(
                        f[i][j], Math.ceil(f[i - 1][j]) + 1.0 * dist[i - 1] / speed - eps);
                }
                if (j > 0) {
                    f[i][j] = Math.min(f[i][j], f[i - 1][j - 1] + 1.0 * dist[i - 1] / speed);
                }
            }
        }
        for (int j = 0; j <= n; ++j) {
            if (f[n][j] <= hoursBefore + eps) {
                return j;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSkips(vector<int>& dist, int speed, int hoursBefore) {
        int n = dist.size();
        vector<vector<double>> f(n + 1, vector<double>(n + 1, 1e20));
        f[0][0] = 0;
        double eps = 1e-8;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j <= i; ++j) {
                if (j < i) {
                    f[i][j] = min(f[i][j], ceil(f[i - 1][j] + dist[i - 1] * 1.0 / speed - eps));
                }
                if (j) {
                    f[i][j] = min(f[i][j], f[i - 1][j - 1] + dist[i - 1] * 1.0 / speed);
                }
            }
        }
        for (int j = 0; j <= n; ++j) {
            if (f[n][j] <= hoursBefore + eps) {
                return j;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func minSkips(dist []int, speed int, hoursBefore int) int {
	n := len(dist)
	f := make([][]float64, n+1)
	for i := range f {
		f[i] = make([]float64, n+1)
		for j := range f[i] {
			f[i][j] = 1e20
		}
	}
	f[0][0] = 0
	eps := 1e-8
	for i := 1; i <= n; i++ {
		for j := 0; j <= i; j++ {
			if j < i {
				f[i][j] = math.Min(f[i][j], math.Ceil(f[i-1][j]+float64(dist[i-1])/float64(speed)-eps))
			}
			if j > 0 {
				f[i][j] = math.Min(f[i][j], f[i-1][j-1]+float64(dist[i-1])/float64(speed))
			}
		}
	}
	for j := 0; j <= n; j++ {
		if f[n][j] <= float64(hoursBefore) {
			return j
		}
	}
	return -1
}
```

#### TypeScript

```ts
function minSkips(dist: number[], speed: number, hoursBefore: number): number {
    const n = dist.length;
    const f = Array.from({ length: n + 1 }, () => Array.from({ length: n + 1 }, () => Infinity));
    f[0][0] = 0;
    const eps = 1e-8;
    for (let i = 1; i <= n; ++i) {
        for (let j = 0; j <= i; ++j) {
            if (j < i) {
                f[i][j] = Math.min(f[i][j], Math.ceil(f[i - 1][j] + dist[i - 1] / speed - eps));
            }
            if (j) {
                f[i][j] = Math.min(f[i][j], f[i - 1][j - 1] + dist[i - 1] / speed);
            }
        }
    }
    for (let j = 0; j <= n; ++j) {
        if (f[n][j] <= hoursBefore + eps) {
            return j;
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
