---
comments: true
difficulty: Hard
rating: 2414
source: Weekly Contest 325 Q4
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2518. Number of Great Partitions](https://leetcode.com/problems/number-of-great-partitions)

[中文文档](/solution/2500-2599/2518.Number%20of%20Great%20Partitions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm các số nguyên <strong>dương</strong> và một số nguyên <code>k</code>.</p>

<p><strong>Phân hoạch</strong> mảng thành hai <strong>nhóm</strong> có thứ tự sao cho mỗi phần tử thuộc chính xác <strong>một</strong> nhóm. Một phân hoạch được gọi là tốt nếu <strong>tổng</strong> các phần tử của mỗi nhóm lớn hơn hoặc bằng <code>k</code>.</p>

<p>Trả về <em>số lượng phân hoạch tốt <strong>phân biệt</strong></em>. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>lấy modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Hai phân hoạch được xem là phân biệt nếu có phần tử <code>nums[i]</code> thuộc các nhóm khác nhau trong hai phân hoạch.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4], k = 4
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Các phân hoạch tốt là: ([1,2,3], [4]), ([1,3], [2,4]), ([1,4], [2,3]), ([2,3], [1,4]), ([2,4], [1,3]) và ([4], [1,2,3]).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,3,3], k = 4
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có phân hoạch tốt nào cho mảng này.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [6,6], k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể đưa nums[0] vào phân hoạch thứ nhất hoặc phân hoạch thứ hai.
Các phân hoạch tốt sẽ là ([6], [6]) và ([6], [6]).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length, k &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi phần tử được đưa vào chính xác một trong hai nhóm, và tổng của cả hai nhóm đều phải ít nhất là $k$. Có $2^n$ cách phân chia; với $n$ và $k$ tối đa $10^3$, không thể liệt kê tất cả, nhưng một nhóm có tổng nhỏ hơn $k$ chính là trường hợp không hợp lệ.
>
> Nếu tổng tất cả phần tử nhỏ hơn $2k$, cả hai phía không thể đồng thời đạt yêu cầu và đáp án là $0$. Vì vậy, ta trừ các phân hoạch không hợp lệ khỏi $2^n$. Một phân hoạch không hợp lệ tương ứng với một tập con có tổng $<k$ được dùng làm một phía, tức là bài toán knapsack $0$-$1$ với sức chứa $k-1$. Vì một trong hai phía đều có thể là nhóm nhỏ, số lượng phân hoạch không hợp lệ bằng hai lần số tập con như vậy.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPartitions(self, nums: List[int], k: int) -> int:
        if sum(nums) < k * 2:
            return 0
        mod = 10**9 + 7
        n = len(nums)
        f = [[0] * k for _ in range(n + 1)]
        f[0][0] = 1
        ans = 1
        for i in range(1, n + 1):
            ans = ans * 2 % mod
            for j in range(k):
                f[i][j] = f[i - 1][j]
                if j >= nums[i - 1]:
                    f[i][j] = (f[i][j] + f[i - 1][j - nums[i - 1]]) % mod
        return (ans - sum(f[-1]) * 2 + mod) % mod
```

#### Java

```java
class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int countPartitions(int[] nums, int k) {
        long s = 0;
        for (int v : nums) {
            s += v;
        }
        if (s < k * 2) {
            return 0;
        }
        int n = nums.length;
        long[][] f = new long[n + 1][k];
        f[0][0] = 1;
        long ans = 1;
        for (int i = 1; i <= n; ++i) {
            int v = nums[i - 1];
            ans = ans * 2 % MOD;
            for (int j = 0; j < k; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= v) {
                    f[i][j] = (f[i][j] + f[i - 1][j - v]) % MOD;
                }
            }
        }
        for (int j = 0; j < k; ++j) {
            ans = (ans - f[n][j] * 2 % MOD + MOD) % MOD;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    const int mod = 1e9 + 7;

    int countPartitions(vector<int>& nums, int k) {
        long s = accumulate(nums.begin(), nums.end(), 0l);
        if (s < k * 2) return 0;
        int n = nums.size();
        long f[n + 1][k];
        int ans = 1;
        memset(f, 0, sizeof f);
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            int v = nums[i - 1];
            ans = ans * 2 % mod;
            for (int j = 0; j < k; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= v) {
                    f[i][j] = (f[i][j] + f[i - 1][j - v]) % mod;
                }
            }
        }
        for (int j = 0; j < k; ++j) {
            ans = (ans - f[n][j] * 2 % mod + mod) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func countPartitions(nums []int, k int) int {
	s := 0
	for _, v := range nums {
		s += v
	}
	if s < k*2 {
		return 0
	}
	const mod int = 1e9 + 7
	n := len(nums)
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, k)
	}
	f[0][0] = 1
	ans := 1
	for i := 1; i <= n; i++ {
		v := nums[i-1]
		ans = ans * 2 % mod
		for j := 0; j < k; j++ {
			f[i][j] = f[i-1][j]
			if j >= v {
				f[i][j] = (f[i][j] + f[i-1][j-v]) % mod
			}
		}
	}
	for j := 0; j < k; j++ {
		ans = (ans - f[n][j]*2%mod + mod) % mod
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
