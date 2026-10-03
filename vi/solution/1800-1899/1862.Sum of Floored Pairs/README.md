---
comments: true
difficulty: Hard
rating: 2170
source: Biweekly Contest 52 Q4
tags:
    - Array
    - Math
    - Binary Search
    - Counting
    - Enumeration
    - Prefix Sum
---

<!-- problem:start -->

# [1862. Sum of Floored Pairs](https://leetcode.com/problems/sum-of-floored-pairs)

[中文文档](/solution/1800-1899/1862.Sum%20of%20Floored%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, hãy trả về tổng của <code>floor(nums[i] / nums[j])</code> với mọi cặp chỉ số <code>0 &lt;= i, j &lt; nums.length</code> trong mảng. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Hàm <code>floor()</code> trả về phần nguyên của phép chia.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,5,9]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong>
floor(2 / 5) = floor(2 / 9) = floor(5 / 9) = 0
floor(2 / 2) = floor(5 / 5) = floor(9 / 9) = 1
floor(5 / 2) = 2
floor(9 / 2) = 4
floor(9 / 5) = 1
Ta tính phần nguyên của phép chia cho mọi cặp chỉ số trong mảng rồi cộng chúng lại.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7,7,7,7,7,7,7]
<strong>Đầu ra:</strong> 49
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum trên miền giá trị + Liệt kê tối ưu

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tính $\sum_{i,j}\lfloor nums[i]/nums[j]\rfloor$. Liệt kê từng cặp có độ phức tạp $O(n^2)$, quá chậm khi $n\le 10^5$.
>
> Các giá trị không vượt quá $10^5$, nên ta xây dựng prefix sum của tần suất. Với mỗi mẫu số $y$ và thương $d$, số tử số trong đoạn $[dy,dy+y)$ được tính bằng hiệu prefix sum, rồi nhân với $cnt[y]\cdot d$. Cách liệt kê theo cấp số điều hòa có độ phức tạp $O(M\log M)$.

<!-- thinking:end -->

Trước hết, ta đếm số lần xuất hiện của mỗi phần tử trong mảng $nums$ và lưu vào mảng $cnt$. Sau đó, ta tính prefix sum của mảng $cnt$ và lưu vào mảng $s$, tức là $s[i]$ biểu diễn số phần tử nhỏ hơn hoặc bằng $i$.

Tiếp theo, ta liệt kê mẫu số $y$ và thương $d$. Dựa vào mảng prefix sum, ta có thể tính số tử số bằng $s[\min(mx, d \times y + y - 1)] - s[d \times y - 1]$, trong đó $mx$ là giá trị lớn nhất trong mảng $nums$. Sau đó, ta nhân số tử số với số mẫu số $cnt[y]$, rồi nhân tiếp với thương $d$. Đây là tổng giá trị của tất cả các phân số thỏa mãn điều kiện. Cộng các giá trị này lại, ta thu được đáp án.

Độ phức tạp thời gian là $O(M \times \log M)$, độ phức tạp không gian là $O(M)$. Ở đây, $M$ là giá trị lớn nhất trong mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfFlooredPairs(self, nums: List[int]) -> int:
        mod = 10**9 + 7
        cnt = Counter(nums)
        mx = max(nums)
        s = [0] * (mx + 1)
        for i in range(1, mx + 1):
            s[i] = s[i - 1] + cnt[i]
        ans = 0
        for y in range(1, mx + 1):
            if cnt[y]:
                d = 1
                while d * y <= mx:
                    ans += cnt[y] * d * (s[min(mx, d * y + y - 1)] - s[d * y - 1])
                    ans %= mod
                    d += 1
        return ans
```

#### Java

```java
class Solution {
    public int sumOfFlooredPairs(int[] nums) {
        final int mod = (int) 1e9 + 7;
        int mx = 0;
        for (int x : nums) {
            mx = Math.max(mx, x);
        }
        int[] cnt = new int[mx + 1];
        int[] s = new int[mx + 1];
        for (int x : nums) {
            ++cnt[x];
        }
        for (int i = 1; i <= mx; ++i) {
            s[i] = s[i - 1] + cnt[i];
        }
        long ans = 0;
        for (int y = 1; y <= mx; ++y) {
            if (cnt[y] > 0) {
                for (int d = 1; d * y <= mx; ++d) {
                    ans += 1L * cnt[y] * d * (s[Math.min(mx, d * y + y - 1)] - s[d * y - 1]);
                    ans %= mod;
                }
            }
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfFlooredPairs(vector<int>& nums) {
        const int mod = 1e9 + 7;
        int mx = *max_element(nums.begin(), nums.end());
        vector<int> cnt(mx + 1);
        vector<int> s(mx + 1);
        for (int x : nums) {
            ++cnt[x];
        }
        for (int i = 1; i <= mx; ++i) {
            s[i] = s[i - 1] + cnt[i];
        }
        long long ans = 0;
        for (int y = 1; y <= mx; ++y) {
            if (cnt[y]) {
                for (int d = 1; d * y <= mx; ++d) {
                    ans += 1LL * cnt[y] * d * (s[min(mx, d * y + y - 1)] - s[d * y - 1]);
                    ans %= mod;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func sumOfFlooredPairs(nums []int) (ans int) {
	mx := slices.Max(nums)
	cnt := make([]int, mx+1)
	s := make([]int, mx+1)
	for _, x := range nums {
		cnt[x]++
	}
	for i := 1; i <= mx; i++ {
		s[i] = s[i-1] + cnt[i]
	}
	const mod int = 1e9 + 7
	for y := 1; y <= mx; y++ {
		if cnt[y] > 0 {
			for d := 1; d*y <= mx; d++ {
				ans += d * cnt[y] * (s[min((d+1)*y-1, mx)] - s[d*y-1])
				ans %= mod
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function sumOfFlooredPairs(nums: number[]): number {
    const mx = Math.max(...nums);
    const cnt: number[] = Array(mx + 1).fill(0);
    const s: number[] = Array(mx + 1).fill(0);
    for (const x of nums) {
        ++cnt[x];
    }
    for (let i = 1; i <= mx; ++i) {
        s[i] = s[i - 1] + cnt[i];
    }
    let ans = 0;
    const mod = 1e9 + 7;
    for (let y = 1; y <= mx; ++y) {
        if (cnt[y]) {
            for (let d = 1; d * y <= mx; ++d) {
                ans += cnt[y] * d * (s[Math.min((d + 1) * y - 1, mx)] - s[d * y - 1]);
                ans %= mod;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_of_floored_pairs(nums: Vec<i32>) -> i32 {
        const MOD: i32 = 1_000_000_007;
        let mut mx = 0;
        for &x in nums.iter() {
            mx = mx.max(x);
        }

        let mut cnt = vec![0; (mx + 1) as usize];
        let mut s = vec![0; (mx + 1) as usize];

        for &x in nums.iter() {
            cnt[x as usize] += 1;
        }

        for i in 1..=mx as usize {
            s[i] = s[i - 1] + cnt[i];
        }

        let mut ans = 0;
        for y in 1..=mx as usize {
            if cnt[y] > 0 {
                let mut d = 1;
                while d * y <= (mx as usize) {
                    ans += ((cnt[y] as i64)
                        * (d as i64)
                        * (s[std::cmp::min(mx as usize, d * y + y - 1)] - s[d * y - 1]))
                        % (MOD as i64);
                    ans %= MOD as i64;
                    d += 1;
                }
            }
        }

        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
