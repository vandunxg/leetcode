---
comments: true
difficulty: Medium
rating: 1483
source: Weekly Contest 411 Q2
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3259. Maximum Energy Boost From Two Drinks](https://leetcode.com/problems/maximum-energy-boost-from-two-drinks)

[中文文档](/solution/3200-3299/3259.Maximum%20Energy%20Boost%20From%20Two%20Drinks/README.md)

## Mô tả

<!-- description:start -->

<p>Một nhà khoa học thể thao đến từ tương lai cho bạn hai mảng số nguyên <code>energyDrinkA</code> và <code>energyDrinkB</code> có cùng độ dài <code>n</code>. Hai mảng này lần lượt biểu thị mức năng lượng được cung cấp mỗi giờ bởi hai loại nước tăng lực khác nhau, A và B.</p>

<p>Bạn muốn <em>tối đa hóa</em> tổng năng lượng nhận được bằng cách uống một loại nước tăng lực <em>mỗi giờ</em>. Tuy nhiên, nếu muốn chuyển từ loại nước này sang loại nước kia, bạn cần chờ <em>một giờ</em> để thanh lọc cơ thể (nghĩa là trong giờ đó bạn không nhận được năng lượng nào).</p>

<p>Hãy trả về tổng năng lượng <strong>lớn nhất</strong> bạn có thể nhận được trong <code>n</code> giờ tiếp theo.</p>

<p><strong>Lưu ý</strong> rằng bạn có thể bắt đầu uống <em>một trong hai</em> loại nước tăng lực.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> energyDrinkA<span class="example-io"> = [1,3,1], </span>energyDrinkB<span class="example-io"> = [3,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Để nhận được 5 đơn vị năng lượng, hãy chỉ uống nước tăng lực A (hoặc chỉ uống B).</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> energyDrinkA<span class="example-io"> = [4,1,1], </span>energyDrinkB<span class="example-io"> = [1,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Để nhận được 7 đơn vị năng lượng:</p>

<ul>
	<li>Uống nước tăng lực A trong giờ đầu tiên.</li>
	<li>Chuyển sang nước tăng lực B và mất năng lượng của giờ thứ hai.</li>
	<li>Nhận năng lượng từ nước tăng lực B trong giờ thứ ba.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == energyDrinkA.length == energyDrinkB.length</code></li>
	<li><code>3 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= energyDrinkA[i], energyDrinkB[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi giờ, ta uống A hoặc B, và việc chuyển đổi giữa hai loại nước cần một giờ thanh lọc. Vì $n\le 10^5$, không thể liệt kê các thời điểm chuyển đổi. Kết quả tối ưu tại giờ $i$ chỉ phụ thuộc vào loại nước được chọn gần nhất.
>
> $f[i][0]$ / $f[i][1]$ lần lượt là điểm số tốt nhất khi kết thúc với A / B: tiếp tục uống cùng loại và cộng giá trị hôm nay, hoặc chuyển từ loại kia sang và bỏ qua giờ đó. Đáp án là giá trị lớn hơn trong hai phần tử của hàng cuối.

<!-- thinking:end -->

Ta định nghĩa $f[i][0]$ là mức năng lượng tối đa nhận được khi chọn nước tăng lực A ở giờ thứ $i$, và $f[i][1]$ là mức năng lượng tối đa nhận được khi chọn nước tăng lực B ở giờ thứ $i$. Ban đầu, $f[0][0] = \textit{energyDrinkA}[0]$, $f[0][1] = \textit{energyDrinkB}[0]$. Đáp án là $\max(f[n - 1][0], f[n - 1][1])$.

Với $i > 0$, ta có các phương trình chuyển trạng thái sau:

$$
\begin{aligned}
f[i][0] & = \max(f[i - 1][0] + \textit{energyDrinkA}[i], f[i - 1][1]) \\
f[i][1] & = \max(f[i - 1][1] + \textit{energyDrinkB}[i], f[i - 1][0])
\end{aligned}
$$

Cuối cùng, trả về $\max(f[n - 1][0], f[n - 1][1])$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxEnergyBoost(self, energyDrinkA: List[int], energyDrinkB: List[int]) -> int:
        n = len(energyDrinkA)
        f = [[0] * 2 for _ in range(n)]
        f[0][0] = energyDrinkA[0]
        f[0][1] = energyDrinkB[0]
        for i in range(1, n):
            f[i][0] = max(f[i - 1][0] + energyDrinkA[i], f[i - 1][1])
            f[i][1] = max(f[i - 1][1] + energyDrinkB[i], f[i - 1][0])
        return max(f[n - 1])
```

#### Java

```java
class Solution {
    public long maxEnergyBoost(int[] energyDrinkA, int[] energyDrinkB) {
        int n = energyDrinkA.length;
        long[][] f = new long[n][2];
        f[0][0] = energyDrinkA[0];
        f[0][1] = energyDrinkB[0];
        for (int i = 1; i < n; ++i) {
            f[i][0] = Math.max(f[i - 1][0] + energyDrinkA[i], f[i - 1][1]);
            f[i][1] = Math.max(f[i - 1][1] + energyDrinkB[i], f[i - 1][0]);
        }
        return Math.max(f[n - 1][0], f[n - 1][1]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxEnergyBoost(vector<int>& energyDrinkA, vector<int>& energyDrinkB) {
        int n = energyDrinkA.size();
        vector<vector<long long>> f(n, vector<long long>(2));
        f[0][0] = energyDrinkA[0];
        f[0][1] = energyDrinkB[0];
        for (int i = 1; i < n; ++i) {
            f[i][0] = max(f[i - 1][0] + energyDrinkA[i], f[i - 1][1]);
            f[i][1] = max(f[i - 1][1] + energyDrinkB[i], f[i - 1][0]);
        }
        return max(f[n - 1][0], f[n - 1][1]);
    }
};
```

#### Go

```go
func maxEnergyBoost(energyDrinkA []int, energyDrinkB []int) int64 {
	n := len(energyDrinkA)
	f := make([][2]int64, n)
	f[0][0] = int64(energyDrinkA[0])
	f[0][1] = int64(energyDrinkB[0])
	for i := 1; i < n; i++ {
		f[i][0] = max(f[i-1][0]+int64(energyDrinkA[i]), f[i-1][1])
		f[i][1] = max(f[i-1][1]+int64(energyDrinkB[i]), f[i-1][0])
	}
	return max(f[n-1][0], f[n-1][1])
}
```

#### TypeScript

```ts
function maxEnergyBoost(energyDrinkA: number[], energyDrinkB: number[]): number {
    const n = energyDrinkA.length;
    const f: number[][] = Array.from({ length: n }, () => [0, 0]);
    f[0][0] = energyDrinkA[0];
    f[0][1] = energyDrinkB[0];
    for (let i = 1; i < n; i++) {
        f[i][0] = Math.max(f[i - 1][0] + energyDrinkA[i], f[i - 1][1]);
        f[i][1] = Math.max(f[i - 1][1] + energyDrinkB[i], f[i - 1][0]);
    }
    return Math.max(...f[n - 1]!);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động (Tối ưu hóa không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 chỉ đọc $f[i-1]$, nên có thể rút gọn bảng thành hai biến. Hai biến luân phiên $f,g$ lưu điểm số tốt nhất cho A và B, đồng thời giảm không gian xuống $O(1)$.

<!-- thinking:end -->

Ta nhận thấy trạng thái $f[i]$ chỉ liên quan đến $f[i - 1]$ chứ không liên quan đến $f[i - 2]$. Vì vậy, ta chỉ cần dùng hai biến $f$ và $g$ để duy trì trạng thái, từ đó tối ưu độ phức tạp không gian xuống $O(1)$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxEnergyBoost(self, energyDrinkA: List[int], energyDrinkB: List[int]) -> int:
        f, g = energyDrinkA[0], energyDrinkB[0]
        for a, b in zip(energyDrinkA[1:], energyDrinkB[1:]):
            f, g = max(f + a, g), max(g + b, f)
        return max(f, g)
```

#### Java

```java
class Solution {
    public long maxEnergyBoost(int[] energyDrinkA, int[] energyDrinkB) {
        int n = energyDrinkA.length;
        long f = energyDrinkA[0], g = energyDrinkB[0];
        for (int i = 1; i < n; ++i) {
            long ff = Math.max(f + energyDrinkA[i], g);
            g = Math.max(g + energyDrinkB[i], f);
            f = ff;
        }
        return Math.max(f, g);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxEnergyBoost(vector<int>& energyDrinkA, vector<int>& energyDrinkB) {
        int n = energyDrinkA.size();
        long long f = energyDrinkA[0], g = energyDrinkB[0];
        for (int i = 1; i < n; ++i) {
            long long ff = max(f + energyDrinkA[i], g);
            g = max(g + energyDrinkB[i], f);
            f = ff;
        }
        return max(f, g);
    }
};
```

#### Go

```go
func maxEnergyBoost(energyDrinkA []int, energyDrinkB []int) int64 {
	n := len(energyDrinkA)
	f, g := energyDrinkA[0], energyDrinkB[0]
	for i := 1; i < n; i++ {
		f, g = max(f+energyDrinkA[i], g), max(g+energyDrinkB[i], f)
	}
	return int64(max(f, g))
}
```

#### TypeScript

```ts
function maxEnergyBoost(energyDrinkA: number[], energyDrinkB: number[]): number {
    const n = energyDrinkA.length;
    let [f, g] = [energyDrinkA[0], energyDrinkB[0]];
    for (let i = 1; i < n; ++i) {
        [f, g] = [Math.max(f + energyDrinkA[i], g), Math.max(g + energyDrinkB[i], f)];
    }
    return Math.max(f, g);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
