---
comments: true
difficulty: Hard
rating: 2005
source: Weekly Contest 316 Q3
tags:
    - Greedy
    - Array
    - Binary Search
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [2448. Minimum Cost to Make Array Equal](https://leetcode.com/problems/minimum-cost-to-make-array-equal)

[中文文档](/solution/2400-2499/2448.Minimum%20Cost%20to%20Make%20Array%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng được đánh chỉ số từ <strong>0</strong> <code>nums</code> và <code>cost</code>, mỗi mảng gồm <code>n</code> số nguyên <strong>dương</strong>.</p>

<p>Bạn có thể thực hiện thao tác sau <strong>bất kỳ</strong> số lần nào:</p>

<ul>
	<li>Tăng hoặc giảm <strong>bất kỳ</strong> phần tử nào của mảng <code>nums</code> đi <code>1</code>.</li>
</ul>

<p>Chi phí thực hiện một thao tác trên phần tử thứ <code>i<sup>th</sup></code> là <code>cost[i]</code>.</p>

<p>Trả về <em>tổng chi phí <strong>nhỏ nhất</strong> sao cho tất cả phần tử của mảng </em><code>nums</code><em> trở nên <strong>bằng nhau</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,5,2], cost = [2,3,1,14]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Ta có thể đưa tất cả phần tử về 2 như sau:
- Tăng phần tử thứ 0<sup>th</sup> một lần. Chi phí là 2.
- Giảm phần tử thứ 1<sup><span style="font-size: 10.8333px;">st</span></sup> một lần. Chi phí là 3.
- Giảm phần tử thứ 2<sup>nd</sup> ba lần. Chi phí là 1 + 1 + 1 = 3.
Tổng chi phí là 2 + 3 + 3 = 8.
Có thể chứng minh rằng không thể làm cho mảng bằng nhau với chi phí nhỏ hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,2,2,2], cost = [4,2,8,1,3]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Tất cả phần tử đã bằng nhau, nên không cần thực hiện thao tác nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length == cost.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i], cost[i] &lt;= 10<sup>6</sup></code></li>
	<li>Các test được tạo sao cho kết quả không vượt quá&nbsp;2<sup>53</sup>-1</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố + Sắp xếp + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Chi phí đưa mọi giá trị về $x$ là $\sum |a_i-x|b_i$. Với $n\le 10^5$, ta không thể thử mọi $x$. Sau khi sắp xếp theo $a$, chi phí được tách thành tổng có trọng số ở bên trái và bên phải; các tổng này có thể tính bằng tổng tiền tố của $a_ib_i$ và $b_i$ khi $x$ là một $a_i$.
>
> Giá trị tối ưu nằm tại một $a_i$ (hàm từng đoạn tuyến tính), nên ta chỉ cần đánh giá mọi vị trí sau khi sắp xếp.

<!-- thinking:end -->

Ta ký hiệu các phần tử của mảng `nums` là $a_1, a_2, \cdots, a_n$ và các phần tử của mảng `cost` là $b_1, b_2, \cdots, b_n$. Ta có thể giả sử $a_1 \leq a_2 \leq \cdots \leq a_n$, tức là mảng `nums` đã được sắp xếp tăng dần.

Giả sử ta thay đổi tất cả phần tử trong mảng `nums` thành $x$, khi đó tổng chi phí cần trả là:

$$
\begin{aligned}
\sum_{i=1}^{n} \left | a_i-x \right | b_i  &= \sum_{i=1}^{k} (x-a_i)b_i + \sum_{i=k+1}^{n} (a_i-x)b_i \\
&= x\sum_{i=1}^{k} b_i - \sum_{i=1}^{k} a_ib_i + \sum_{i=k+1}^{n}a_ib_i - x\sum_{i=k+1}^{n}b_i
\end{aligned}
$$

trong đó $k$ là số phần tử trong $a_1, a_2, \cdots, a_n$ nhỏ hơn hoặc bằng $x$.

Ta có thể dùng phương pháp tổng tiền tố để tính $\sum_{i=1}^{k} b_i$ và $\sum_{i=1}^{k} a_ib_i$, cũng như $\sum_{i=k+1}^{n}a_ib_i$ và $\sum_{i=k+1}^{n}b_i$.

Sau đó, ta liệt kê các giá trị $x$, tính bốn tổng tiền tố trên, thu được tổng chi phí tương ứng rồi chọn giá trị nhỏ nhất.

Độ phức tạp thời gian là $O(n\times \log n)$, trong đó $n$ là độ dài của mảng `nums`. Phần lớn độ phức tạp thời gian đến từ việc sắp xếp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, nums: List[int], cost: List[int]) -> int:
        arr = sorted(zip(nums, cost))
        n = len(arr)
        f = [0] * (n + 1)
        g = [0] * (n + 1)
        for i in range(1, n + 1):
            a, b = arr[i - 1]
            f[i] = f[i - 1] + a * b
            g[i] = g[i - 1] + b
        ans = inf
        for i in range(1, n + 1):
            a = arr[i - 1][0]
            l = a * g[i - 1] - f[i - 1]
            r = f[n] - f[i] - a * (g[n] - g[i])
            ans = min(ans, l + r)
        return ans
```

#### Java

```java
class Solution {
    public long minCost(int[] nums, int[] cost) {
        int n = nums.length;
        int[][] arr = new int[n][2];
        for (int i = 0; i < n; ++i) {
            arr[i] = new int[] {nums[i], cost[i]};
        }
        Arrays.sort(arr, (a, b) -> a[0] - b[0]);
        long[] f = new long[n + 1];
        long[] g = new long[n + 1];
        for (int i = 1; i <= n; ++i) {
            long a = arr[i - 1][0], b = arr[i - 1][1];
            f[i] = f[i - 1] + a * b;
            g[i] = g[i - 1] + b;
        }
        long ans = Long.MAX_VALUE;
        for (int i = 1; i <= n; ++i) {
            long a = arr[i - 1][0];
            long l = a * g[i - 1] - f[i - 1];
            long r = f[n] - f[i] - a * (g[n] - g[i]);
            ans = Math.min(ans, l + r);
        }
        return ans;
    }
}
```

#### C++

```cpp
using ll = long long;

class Solution {
public:
    long long minCost(vector<int>& nums, vector<int>& cost) {
        int n = nums.size();
        vector<pair<int, int>> arr(n);
        for (int i = 0; i < n; ++i) arr[i] = {nums[i], cost[i]};
        sort(arr.begin(), arr.end());
        vector<ll> f(n + 1), g(n + 1);
        for (int i = 1; i <= n; ++i) {
            auto [a, b] = arr[i - 1];
            f[i] = f[i - 1] + 1ll * a * b;
            g[i] = g[i - 1] + b;
        }
        ll ans = 1e18;
        for (int i = 1; i <= n; ++i) {
            auto [a, _] = arr[i - 1];
            ll l = 1ll * a * g[i - 1] - f[i - 1];
            ll r = f[n] - f[i] - 1ll * a * (g[n] - g[i]);
            ans = min(ans, l + r);
        }
        return ans;
    }
};
```

#### Go

```go
func minCost(nums []int, cost []int) int64 {
	n := len(nums)
	type pair struct{ a, b int }
	arr := make([]pair, n)
	for i, a := range nums {
		b := cost[i]
		arr[i] = pair{a, b}
	}
	sort.Slice(arr, func(i, j int) bool { return arr[i].a < arr[j].a })
	f := make([]int, n+1)
	g := make([]int, n+1)
	for i := 1; i <= n; i++ {
		a, b := arr[i-1].a, arr[i-1].b
		f[i] = f[i-1] + a*b
		g[i] = g[i-1] + b
	}
	var ans int64 = 1e18
	for i := 1; i <= n; i++ {
		a := arr[i-1].a
		l := a*g[i-1] - f[i-1]
		r := f[n] - f[i] - a*(g[n]-g[i])
		ans = min(ans, int64(l+r))
	}
	return ans
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn min_cost(nums: Vec<i32>, cost: Vec<i32>) -> i64 {
        let mut zip_vec: Vec<_> = nums.into_iter().zip(cost.into_iter()).collect();

        // Sort the zip vector based on nums
        zip_vec.sort_by(|lhs, rhs| lhs.0.cmp(&rhs.0));

        let (nums, cost): (Vec<i32>, Vec<i32>) = zip_vec.into_iter().unzip();

        let mut sum: i64 = 0;
        for &c in &cost {
            sum += c as i64;
        }
        let middle_cost = (sum + 1) / 2;
        let mut cur_sum: i64 = 0;
        let mut i = 0;
        let n = nums.len();

        while i < n {
            if (cost[i] as i64) + cur_sum >= middle_cost {
                break;
            }
            cur_sum += cost[i] as i64;
            i += 1;
        }

        Self::compute_manhattan_dis(&nums, &cost, nums[i])
    }

    #[allow(dead_code)]
    fn compute_manhattan_dis(v: &Vec<i32>, c: &Vec<i32>, e: i32) -> i64 {
        let mut ret = 0;
        let n = v.len();

        for i in 0..n {
            if v[i] == e {
                continue;
            }
            ret += ((v[i] - e).abs() as i64) * (c[i] as i64);
        }

        ret
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Trung vị

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 thử mọi $a_i$. Nếu coi $b_i$ là số lần xuất hiện, trung vị có trọng số là giá trị tối ưu: ta duyệt cho đến khi tổng trọng số tiền tố vượt quá một nửa tổng $\sum b$, rồi chỉ cần tính chi phí một lần.

<!-- thinking:end -->

Ta cũng có thể coi $b_i$ là số lần xuất hiện của $a_i$, khi đó chỉ số của trung vị là $\frac{\sum_{i=1}^{n} b_i}{2}$. Việc thay đổi tất cả các số thành trung vị chắc chắn là tối ưu.

Độ phức tạp thời gian là $O(n\times \log n)$, trong đó $n$ là độ dài của mảng `nums`. Phần lớn độ phức tạp thời gian đến từ việc sắp xếp.

Bài toán tương tự:

- [296. Best Meeting Point](https://github.com/doocs/leetcode/blob/main/solution/0200-0299/0296.Best%20Meeting%20Point/README_EN.md)
- [462. Minimum Moves to Equal Array Elements II](https://github.com/doocs/leetcode/blob/main/solution/0400-0499/0462.Minimum%20Moves%20to%20Equal%20Array%20Elements%20II/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, nums: List[int], cost: List[int]) -> int:
        arr = sorted(zip(nums, cost))
        mid = sum(cost) // 2
        s = 0
        for x, c in arr:
            s += c
            if s > mid:
                return sum(abs(v - x) * c for v, c in arr)
```

#### Java

```java
class Solution {
    public long minCost(int[] nums, int[] cost) {
        int n = nums.length;
        int[][] arr = new int[n][2];
        for (int i = 0; i < n; ++i) {
            arr[i] = new int[] {nums[i], cost[i]};
        }
        Arrays.sort(arr, (a, b) -> a[0] - b[0]);
        long mid = sum(cost) / 2;
        long s = 0, ans = 0;
        for (var e : arr) {
            int x = e[0], c = e[1];
            s += c;
            if (s > mid) {
                for (var t : arr) {
                    ans += (long) Math.abs(t[0] - x) * t[1];
                }
                break;
            }
        }
        return ans;
    }

    private long sum(int[] arr) {
        long s = 0;
        for (int v : arr) {
            s += v;
        }
        return s;
    }
}
```

#### C++

```cpp
using ll = long long;

class Solution {
public:
    long long minCost(vector<int>& nums, vector<int>& cost) {
        int n = nums.size();
        vector<pair<int, int>> arr(n);
        for (int i = 0; i < n; ++i) arr[i] = {nums[i], cost[i]};
        sort(arr.begin(), arr.end());
        ll mid = accumulate(cost.begin(), cost.end(), 0ll) / 2;
        ll s = 0, ans = 0;
        for (auto [x, c] : arr) {
            s += c;
            if (s > mid) {
                for (auto [v, d] : arr) {
                    ans += 1ll * abs(v - x) * d;
                }
                break;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minCost(nums []int, cost []int) int64 {
	n := len(nums)
	type pair struct{ a, b int }
	arr := make([]pair, n)
	mid := 0
	for i, a := range nums {
		b := cost[i]
		mid += b
		arr[i] = pair{a, b}
	}
	mid /= 2
	sort.Slice(arr, func(i, j int) bool { return arr[i].a < arr[j].a })
	s, ans := 0, 0
	for _, e := range arr {
		x, c := e.a, e.b
		s += c
		if s > mid {
			for _, t := range arr {
				ans += abs(t.a-x) * t.b
			}
			break
		}
	}
	return int64(ans)

}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
