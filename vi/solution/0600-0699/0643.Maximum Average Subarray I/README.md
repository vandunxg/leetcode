---
comments: true
difficulty: Easy
tags:
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i)

[中文文档](/solution/0600-0699/0643.Maximum%20Average%20Subarray%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> gồm <code>n</code> phần tử và một số nguyên <code>k</code>.</p>

<p>Hãy tìm mảng con liên tiếp có <strong>độ dài bằng</strong> <code>k</code> và giá trị trung bình lớn nhất, rồi trả về <em>giá trị đó</em>. Chấp nhận mọi đáp án có sai số tính toán nhỏ hơn <code>10<sup>-5</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,12,-5,-6,50,3], k = 4
<strong>Đầu ra:</strong> 12.75000
<strong>Giải thích:</strong> Giá trị trung bình lớn nhất là (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5], k = 1
<strong>Đầu ra:</strong> 5.00000
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= k &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Giá trị trung bình lớn nhất của cửa sổ độ dài $k$ tương ứng với tổng lớn nhất. Tính lại tổng cho từng cửa sổ sẽ tốn $O(nk)$ khi $n\le 10^5$.
>
> Trượt cửa sổ tổng độ dài $k$: cộng giá trị mới đi vào, trừ giá trị rời khỏi cửa sổ, rồi lấy tổng lớn nhất chia cho $k$.

<!-- thinking:end -->

Ta duy trì một sliding window có độ dài $k$ và tính tổng $s$ các số trong mỗi cửa sổ. Tổng lớn nhất $s$ chính là giá trị cần tìm.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        ans = s = sum(nums[:k])
        for i in range(k, len(nums)):
            s += nums[i] - nums[i - k]
            ans = max(ans, s)
        return ans / k
```

#### Java

```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        int s = 0;
        for (int i = 0; i < k; ++i) {
            s += nums[i];
        }
        int ans = s;
        for (int i = k; i < nums.length; ++i) {
            s += (nums[i] - nums[i - k]);
            ans = Math.max(ans, s);
        }
        return ans * 1.0 / k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        int s = accumulate(nums.begin(), nums.begin() + k, 0);
        int ans = s;
        for (int i = k; i < nums.size(); ++i) {
            s += nums[i] - nums[i - k];
            ans = max(ans, s);
        }
        return static_cast<double>(ans) / k;
    }
};
```

#### Go

```go
func findMaxAverage(nums []int, k int) float64 {
	s := 0
	for _, x := range nums[:k] {
		s += x
	}
	ans := s
	for i := k; i < len(nums); i++ {
		s += nums[i] - nums[i-k]
		ans = max(ans, s)
	}
	return float64(ans) / float64(k)
}
```

#### TypeScript

```ts
function findMaxAverage(nums: number[], k: number): number {
    let s = 0;
    for (let i = 0; i < k; ++i) {
        s += nums[i];
    }
    let ans = s;
    for (let i = k; i < nums.length; ++i) {
        s += nums[i] - nums[i - k];
        ans = Math.max(ans, s);
    }
    return ans / k;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_max_average(nums: Vec<i32>, k: i32) -> f64 {
        let k = k as usize;
        let n = nums.len();
        let mut s = nums.iter().take(k).sum::<i32>();
        let mut ans = s;
        for i in k..n {
            s += nums[i] - nums[i - k];
            ans = ans.max(s);
        }
        f64::from(ans) / f64::from(k as i32)
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums
     * @param Integer $k
     * @return Float
     */
    function findMaxAverage($nums, $k) {
        $s = 0;
        for ($i = 0; $i < $k; $i++) {
            $s += $nums[$i];
        }
        $ans = $s;
        for ($j = $k; $j < count($nums); $j++) {
            $s = $s - $nums[$j - $k] + $nums[$j];
            $ans = max($ans, $s);
        }
        return $ans / $k;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
