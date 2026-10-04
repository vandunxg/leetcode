---
comments: true
difficulty: Medium
rating: 1943
source: Weekly Contest 427 Q3
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [3381. Maximum Subarray Sum With Length Divisible by K](https://leetcode.com/problems/maximum-subarray-sum-with-length-divisible-by-k)

[中文文档](/solution/3300-3399/3381.Maximum%20Subarray%20Sum%20With%20Length%20Divisible%20by%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Hãy trả về tổng <strong>lớn nhất</strong> của một <span data-keyword="subarray-nonempty">mảng con</span> của <code>nums</code>, sao cho độ dài của mảng con <strong>chia hết</strong> cho <code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con <code>[1, 2]</code> có tổng bằng 3 và độ dài bằng 2, chia hết cho 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-1,-2,-3,-4,-5], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con có tổng lớn nhất là <code>[-1, -2, -3, -4]</code>, có độ dài bằng 4 và chia hết cho 4.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-5,1,2,-3,4], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con có tổng lớn nhất là <code>[1, 2, -3, 4]</code>, có độ dài bằng 4 và chia hết cho 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Độ dài mảng con là bội số của $k$ khi và chỉ khi hai chỉ số prefix có cùng phần dư modulo $k$. Với $n \le 2 \times 10^5$, ta lưu tổng tiền tố nhỏ nhất của mỗi phần dư.
>
> $f[r]$ là tổng tiền tố nhỏ nhất có chỉ số đồng dư với $r$ modulo $k$. Tại $j$, ta cập nhật đáp án bằng $s-f[j \bmod k]$, sau đó ghi $s$ vào vị trí tương ứng.
>
> Giá trị khởi tạo $f[k-1]=0$ biểu diễn tổng tiền tố rỗng tại chỉ số $-1$.

<!-- thinking:end -->

Theo mô tả bài toán, để độ dài của một mảng con chia hết cho $k$, mảng con $\textit{nums}[i+1 \ldots j]$ cần thỏa mãn $i \bmod k = j \bmod k$.

Ta có thể liệt kê điểm cuối bên phải $j$ của mảng con và sử dụng một mảng $\textit{f}$ có độ dài $k$ để lưu tổng tiền tố nhỏ nhất tương ứng với mỗi phần dư modulo $k$. Ban đầu, $\textit{f}[k-1] = 0$, biểu thị tổng tiền tố tại chỉ số $-1$ bằng $0$.

Với điểm cuối bên phải hiện tại $j$ và tổng tiền tố $s$, ta có thể tính tổng lớn nhất của các mảng con kết thúc tại $j$ có độ dài chia hết cho $k$ bằng $s - \textit{f}[j \bmod k]$, rồi cập nhật đáp án tương ứng. Đồng thời, ta cần cập nhật $\textit{f}[j \bmod k]$ thành giá trị nhỏ hơn giữa tổng tiền tố hiện tại $s$ và $\textit{f}[j \bmod k]$.

Sau khi hoàn tất việc liệt kê, trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(k)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSubarraySum(self, nums: List[int], k: int) -> int:
        f = [inf] * k
        ans = -inf
        s = f[-1] = 0
        for i, x in enumerate(nums):
            s += x
            ans = max(ans, s - f[i % k])
            f[i % k] = min(f[i % k], s)
        return ans
```

#### Java

```java
class Solution {
    public long maxSubarraySum(int[] nums, int k) {
        long[] f = new long[k];
        final long inf = 1L << 62;
        Arrays.fill(f, inf);
        f[k - 1] = 0;
        long s = 0;
        long ans = -inf;
        for (int i = 0; i < nums.length; ++i) {
            s += nums[i];
            ans = Math.max(ans, s - f[i % k]);
            f[i % k] = Math.min(f[i % k], s);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxSubarraySum(vector<int>& nums, int k) {
        using ll = long long;
        ll inf = 1e18;
        vector<ll> f(k, inf);
        ll ans = -inf;
        ll s = 0;
        f[k - 1] = 0;
        for (int i = 0; i < nums.size(); ++i) {
            s += nums[i];
            ans = max(ans, s - f[i % k]);
            f[i % k] = min(f[i % k], s);
        }
        return ans;
    }
};
```

#### Go

```go
func maxSubarraySum(nums []int, k int) int64 {
	inf := int64(1) << 62
	f := make([]int64, k)
	for i := range f {
		f[i] = inf
	}
	f[k-1] = 0

	var s, ans int64
	ans = -inf
	for i := 0; i < len(nums); i++ {
		s += int64(nums[i])
		ans = max(ans, s-f[i%k])
		f[i%k] = min(f[i%k], s)
	}

	return ans
}
```

#### TypeScript

```ts
function maxSubarraySum(nums: number[], k: number): number {
    const f: number[] = Array(k).fill(Infinity);
    f[k - 1] = 0;
    let ans = -Infinity;
    let s = 0;
    for (let i = 0; i < nums.length; ++i) {
        s += nums[i];
        ans = Math.max(ans, s - f[i % k]);
        f[i % k] = Math.min(f[i % k], s);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_subarray_sum(nums: Vec<i32>, k: i32) -> i64 {
        let k = k as usize;
        let inf = 1i64 << 62;
        let mut f = vec![inf; k];
        f[k - 1] = 0;
        let mut s = 0i64;
        let mut ans = -inf;
        for (i, &x) in nums.iter().enumerate() {
            s += x as i64;
            ans = ans.max(s - f[i % k]);
            f[i % k] = f[i % k].min(s);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
