---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2361. Minimum Costs Using the Train Line 🔒](https://leetcode.com/problems/minimum-costs-using-the-train-line)

[中文文档](/solution/2300-2399/2361.Minimum%20Costs%20Using%20the%20Train%20Line/README.md)

## Mô tả

<!-- description:start -->

<p>Một tuyến tàu đi qua thành phố có hai lộ trình: lộ trình thường và lộ trình express. Cả hai lộ trình đều đi qua cùng <strong>một</strong> <code>n + 1</code> trạm, được đánh số từ <code>0</code> đến <code>n</code>. Ban đầu, bạn ở lộ trình thường tại trạm <code>0</code>.</p>

<p>Bạn được cho hai mảng số nguyên <strong>1-indexed</strong> là <code>regular</code> và <code>express</code>, cả hai đều có độ dài <code>n</code>. <code>regular[i]</code> mô tả chi phí đi từ trạm <code>i - 1</code> đến trạm <code>i</code> bằng lộ trình thường, còn <code>express[i]</code> mô tả chi phí đi từ trạm <code>i - 1</code> đến trạm <code>i</code> bằng lộ trình express.</p>

<p>Bạn cũng được cho một số nguyên <code>expressCost</code>, biểu thị chi phí chuyển từ lộ trình thường sang lộ trình express.</p>

<p>Lưu ý rằng:</p>

<ul>
	<li>Không mất chi phí khi chuyển từ lộ trình express trở lại lộ trình thường.</li>
	<li>Bạn trả <code>expressCost</code> <strong>mỗi</strong> lần chuyển từ lộ trình thường sang lộ trình express.</li>
	<li>Không có chi phí bổ sung khi tiếp tục ở lộ trình express.</li>
</ul>

<p>Hãy trả về <em>một mảng <strong>1-indexed</strong> </em><code>costs</code><em> có độ dài </em><code>n</code><em>, trong đó </em><code>costs[i]</code><em> là chi phí <strong>nhỏ nhất</strong> để đi từ trạm </em><code>0</code><em> đến trạm </em><code>i</code>.</p>

<p>Lưu ý rằng một trạm có thể được xem là <strong>đã đến</strong> từ một trong hai lộ trình.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2361.Minimum%20Costs%20Using%20the%20Train%20Line/images/ex1drawio.png" style="width: 442px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> regular = [1,6,9,5], express = [5,2,3,10], expressCost = 8
<strong>Đầu ra:</strong> [1,7,14,19]
<strong>Giải thích:</strong> Sơ đồ trên minh họa cách đi từ trạm 0 đến trạm 4 với chi phí nhỏ nhất.
- Đi theo lộ trình thường từ trạm 0 đến trạm 1, tốn 1.
- Đi theo lộ trình express từ trạm 1 đến trạm 2, tốn 8 + 2 = 10.
- Đi theo lộ trình express từ trạm 2 đến trạm 3, tốn 3.
- Đi theo lộ trình thường từ trạm 3 đến trạm 4, tốn 5.
Tổng chi phí là 1 + 10 + 3 + 5 = 19.
Lưu ý rằng có thể chọn một lộ trình khác để đến các trạm còn lại với chi phí nhỏ nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2361.Minimum%20Costs%20Using%20the%20Train%20Line/images/ex2drawio.png" style="width: 346px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> regular = [11,5,13], express = [7,10,6], expressCost = 3
<strong>Đầu ra:</strong> [10,15,24]
<strong>Giải thích:</strong> Sơ đồ trên minh họa cách đi từ trạm 0 đến trạm 3 với chi phí nhỏ nhất.
- Đi theo lộ trình express từ trạm 0 đến trạm 1, tốn 3 + 7 = 10.
- Đi theo lộ trình thường từ trạm 1 đến trạm 2, tốn 5.
- Đi theo lộ trình express từ trạm 2 đến trạm 3, tốn 3 + 6 = 9.
Tổng chi phí là 10 + 5 + 9 = 24.
Lưu ý rằng phải trả thêm expressCost mỗi lần chuyển sang lộ trình express.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == regular.length == express.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= regular[i], express[i], expressCost &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Hai lộ trình chạy song song; lần đầu đi lộ trình express phải trả $expressCost$. Vì $n \le 10^5$, ta cần một công thức truy hồi tuyến tính. Chi phí tại trạm $i$ chỉ phụ thuộc vào lộ trình được sử dụng tại trạm $i-1$.
>
> Gọi $f[i]$ và $g[i]$ lần lượt là chi phí nhỏ nhất để đến trạm i bằng lộ trình thường hoặc express. Lộ trình thường cộng thêm $a_i$ từ cả hai lộ trình; lộ trình express cộng thêm $b_i$ nếu đang ở express, hoặc $expressCost+b_i$ nếu đang ở lộ trình thường. Đáp án tại $i$ là giá trị nhỏ hơn trong hai chi phí này.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là chi phí nhỏ nhất để đi từ trạm $0$ đến trạm $i$ và đến trạm $i$ bằng lộ trình thường, còn $g[i]$ là chi phí nhỏ nhất để đi từ trạm $0$ đến trạm $i$ và đến trạm $i$ bằng lộ trình express. Ban đầu, $f[0]=0, g[0]=\infty$.

Tiếp theo, ta xét cách chuyển trạng thái của $f[i]$ và $g[i]$.

Nếu đến trạm $i$ bằng lộ trình thường, ta có thể đi từ trạm $i-1$ bằng lộ trình thường hoặc chuyển từ lộ trình express tại trạm $i-1$ sang lộ trình thường. Do đó, ta có công thức chuyển trạng thái:

$$
f[i]=\min\{f[i-1]+a_i, g[i-1]+a_i\}
$$

trong đó $a_i$ biểu thị chi phí đi theo lộ trình thường từ trạm $i-1$ đến trạm $i$.

Nếu đến trạm $i$ bằng lộ trình express, ta có thể chuyển từ lộ trình thường tại trạm $i-1$ sang lộ trình express hoặc tiếp tục đi trên lộ trình express từ trạm $i-1$. Do đó, ta có công thức chuyển trạng thái:

$$
g[i]=\min\{f[i-1]+expressCost+b_i, g[i-1]+b_i\}
$$

trong đó $b_i$ biểu thị chi phí đi theo lộ trình express từ trạm $i-1$ đến trạm $i$.

Ta gọi mảng đáp án là $cost$, trong đó $cost[i]$ biểu thị chi phí nhỏ nhất để đi từ trạm $0$ đến trạm $i$. Vì ta có thể đến trạm $i$ bằng bất kỳ lộ trình nào, nên $cost[i]=\min\{f[i], g[i]\}$.

Cuối cùng, ta trả về $cost$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số trạm.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCosts(
        self, regular: List[int], express: List[int], expressCost: int
    ) -> List[int]:
        n = len(regular)
        f = [0] * (n + 1)
        g = [inf] * (n + 1)
        cost = [0] * n
        for i, (a, b) in enumerate(zip(regular, express), 1):
            f[i] = min(f[i - 1] + a, g[i - 1] + a)
            g[i] = min(f[i - 1] + expressCost + b, g[i - 1] + b)
            cost[i - 1] = min(f[i], g[i])
        return cost
```

#### Java

```java
class Solution {
    public long[] minimumCosts(int[] regular, int[] express, int expressCost) {
        int n = regular.length;
        long[] f = new long[n + 1];
        long[] g = new long[n + 1];
        g[0] = 1 << 30;
        long[] cost = new long[n];
        for (int i = 1; i <= n; ++i) {
            int a = regular[i - 1];
            int b = express[i - 1];
            f[i] = Math.min(f[i - 1] + a, g[i - 1] + a);
            g[i] = Math.min(f[i - 1] + expressCost + b, g[i - 1] + b);
            cost[i - 1] = Math.min(f[i], g[i]);
        }
        return cost;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> minimumCosts(vector<int>& regular, vector<int>& express, int expressCost) {
        int n = regular.size();
        long long f[n + 1];
        long long g[n + 1];
        f[0] = 0;
        g[0] = 1 << 30;
        vector<long long> cost(n);
        for (int i = 1; i <= n; ++i) {
            int a = regular[i - 1];
            int b = express[i - 1];
            f[i] = min(f[i - 1] + a, g[i - 1] + a);
            g[i] = min(f[i - 1] + expressCost + b, g[i - 1] + b);
            cost[i - 1] = min(f[i], g[i]);
        }
        return cost;
    }
};
```

#### Go

```go
func minimumCosts(regular []int, express []int, expressCost int) []int64 {
	n := len(regular)
	f := make([]int, n+1)
	g := make([]int, n+1)
	g[0] = 1 << 30
	cost := make([]int64, n)
	for i := 1; i <= n; i++ {
		a, b := regular[i-1], express[i-1]
		f[i] = min(f[i-1]+a, g[i-1]+a)
		g[i] = min(f[i-1]+expressCost+b, g[i-1]+b)
		cost[i-1] = int64(min(f[i], g[i]))
	}
	return cost
}
```

#### TypeScript

```ts
function minimumCosts(regular: number[], express: number[], expressCost: number): number[] {
    const n = regular.length;
    const f: number[] = new Array(n + 1).fill(0);
    const g: number[] = new Array(n + 1).fill(0);
    g[0] = 1 << 30;
    const cost: number[] = new Array(n).fill(0);
    for (let i = 1; i <= n; ++i) {
        const [a, b] = [regular[i - 1], express[i - 1]];
        f[i] = Math.min(f[i - 1] + a, g[i - 1] + a);
        g[i] = Math.min(f[i - 1] + expressCost + b, g[i - 1] + b);
        cost[i - 1] = Math.min(f[i], g[i]);
    }
    return cost;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động tối ưu

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 lưu toàn bộ các mảng. Vì công thức chuyển trạng thái chỉ cần cặp giá trị trước đó, ta chỉ cần hai biến vô hướng, nhờ đó không gian phụ giảm xuống hằng số (mảng đầu ra vẫn sử dụng $O(n)$).

<!-- thinking:end -->

$f[i]$ và $g[i]$ chỉ phụ thuộc vào $f[i-1]$ và $g[i-1]$, vì vậy ta có thể giữ hai biến luân phiên và giảm không gian phụ xuống $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCosts(
        self, regular: List[int], express: List[int], expressCost: int
    ) -> List[int]:
        n = len(regular)
        f, g = 0, inf
        cost = [0] * n
        for i, (a, b) in enumerate(zip(regular, express), 1):
            ff = min(f + a, g + a)
            gg = min(f + expressCost + b, g + b)
            f, g = ff, gg
            cost[i - 1] = min(f, g)
        return cost
```

#### Java

```java
class Solution {
    public long[] minimumCosts(int[] regular, int[] express, int expressCost) {
        int n = regular.length;
        long f = 0;
        long g = 1 << 30;
        long[] cost = new long[n];
        for (int i = 0; i < n; ++i) {
            int a = regular[i];
            int b = express[i];
            long ff = Math.min(f + a, g + a);
            long gg = Math.min(f + expressCost + b, g + b);
            f = ff;
            g = gg;
            cost[i] = Math.min(f, g);
        }
        return cost;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> minimumCosts(vector<int>& regular, vector<int>& express, int expressCost) {
        int n = regular.size();
        long long f = 0;
        long long g = 1 << 30;
        vector<long long> cost(n);
        for (int i = 0; i < n; ++i) {
            int a = regular[i];
            int b = express[i];
            long long ff = min(f + a, g + a);
            long long gg = min(f + expressCost + b, g + b);
            f = ff;
            g = gg;
            cost[i] = min(f, g);
        }
        return cost;
    }
};
```

#### Go

```go
func minimumCosts(regular []int, express []int, expressCost int) []int64 {
	f, g := 0, 1<<30
	cost := make([]int64, len(regular))
	for i, a := range regular {
		b := express[i]
		ff := min(f+a, g+a)
		gg := min(f+expressCost+b, g+b)
		f, g = ff, gg
		cost[i] = int64(min(f, g))
	}
	return cost
}
```

#### TypeScript

```ts
function minimumCosts(regular: number[], express: number[], expressCost: number): number[] {
    const n = regular.length;
    let f = 0;
    let g = 1 << 30;
    const cost: number[] = new Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        const [a, b] = [regular[i], express[i]];
        const ff = Math.min(f + a, g + a);
        const gg = Math.min(f + expressCost + b, g + b);
        [f, g] = [ff, gg];
        cost[i] = Math.min(f, g);
    }
    return cost;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
