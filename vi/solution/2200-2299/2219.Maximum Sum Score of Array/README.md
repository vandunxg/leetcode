---
comments: true
difficulty: Medium
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [2219. Maximum Sum Score of Array 🔒](https://leetcode.com/problems/maximum-sum-score-of-array)

[中文文档](/solution/2200-2299/2219.Maximum%20Sum%20Score%20of%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> <strong>được đánh chỉ số từ 0</strong>, có độ dài <code>n</code>.</p>

<p><strong>Điểm </strong><strong>tổng</strong> của <code>nums</code> tại một chỉ số <code>i</code> với <code>0 &lt;= i &lt; n</code> là <strong>giá trị lớn hơn</strong> trong hai giá trị sau:</p>

<ul>
	<li>Tổng của <code>i + 1</code> phần tử <strong>đầu tiên</strong> của <code>nums</code>.</li>
	<li>Tổng của <code>n - i</code> phần tử <strong>cuối cùng</strong> của <code>nums</code>.</li>
</ul>

<p>Trả về <em><strong>Điểm </strong><strong>tổng</strong> <strong>lớn nhất</strong> của </em><code>nums</code><em> tại bất kỳ chỉ số nào.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,3,-2,5]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong>
Điểm tổng tại chỉ số 0 là max(4, 4 + 3 + -2 + 5) = max(4, 10) = 10.
Điểm tổng tại chỉ số 1 là max(4 + 3, 3 + -2 + 5) = max(7, 6) = 7.
Điểm tổng tại chỉ số 2 là max(4 + 3 + -2, -2 + 5) = max(5, 3) = 5.
Điểm tổng tại chỉ số 3 là max(4 + 3 + -2 + 5, 5) = max(10, 5) = 10.
Điểm tổng lớn nhất của nums là 10.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-3,-5]
<strong>Đầu ra:</strong> -3
<strong>Giải thích:</strong>
Điểm tổng tại chỉ số 0 là max(-3, -3 + -5) = max(-3, -8) = -3.
Điểm tổng tại chỉ số 1 là max(-3 + -5, -5) = max(-8, -5) = -5.
Điểm tổng lớn nhất của nums là -3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Điểm tại $i$ là giá trị lớn hơn giữa tổng tiền tố đến $i$ và tổng hậu tố bắt đầu từ $i$; ta cần lấy giá trị lớn nhất trên mọi $i$. Vì $n \le 10^5$, không thể tính tổng lại từ đầu tại mỗi chỉ số.
>
> Có thể duy trì cả hai tổng trong khi duyệt: bắt đầu với tổng $r$, cộng $x$ vào tổng tiền tố $l$, cập nhật đáp án bằng $\max(l, r)$, rồi trừ $x$ khỏi $r$. Chỉ cần một lượt duyệt.

<!-- thinking:end -->

Ta có thể dùng hai biến $l$ và $r$ lần lượt biểu diễn tổng tiền tố và tổng hậu tố của mảng. Ban đầu, $l = 0$ và $r = \sum_{i=0}^{n-1} \textit{nums}[i]$.

Tiếp theo, ta duyệt mảng $\textit{nums}$. Với mỗi phần tử $x$, ta cộng $x$ vào $l$ và cập nhật đáp án $\textit{ans} = \max(\textit{ans}, l, r)$, sau đó trừ $x$ khỏi $r$.

Sau khi duyệt xong, trả về đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSumScore(self, nums: List[int]) -> int:
        l, r = 0, sum(nums)
        ans = -inf
        for x in nums:
            l += x
            ans = max(ans, l, r)
            r -= x
        return ans
```

#### Java

```java
class Solution {
    public long maximumSumScore(int[] nums) {
        long l = 0, r = 0;
        for (int x : nums) {
            r += x;
        }
        long ans = Long.MIN_VALUE;
        for (int x : nums) {
            l += x;
            ans = Math.max(ans, Math.max(l, r));
            r -= x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumSumScore(vector<int>& nums) {
        long long l = 0, r = accumulate(nums.begin(), nums.end(), 0LL);
        long long ans = -1e18;
        for (int x : nums) {
            l += x;
            ans = max({ans, l, r});
            r -= x;
        }
        return ans;
    }
};
```

#### Go

```go
func maximumSumScore(nums []int) int64 {
	l, r := 0, 0
	for _, x := range nums {
		r += x
	}
	ans := math.MinInt64
	for _, x := range nums {
		l += x
		ans = max(ans, max(l, r))
		r -= x
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function maximumSumScore(nums: number[]): number {
    let l = 0;
    let r = nums.reduce((a, b) => a + b, 0);
    let ans = -Infinity;
    for (const x of nums) {
        l += x;
        ans = Math.max(ans, l, r);
        r -= x;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_sum_score(nums: Vec<i32>) -> i64 {
        let mut l = 0;
        let mut r: i64 = nums.iter().map(|&x| x as i64).sum();
        let mut ans = std::i64::MIN;
        for &x in &nums {
            l += x as i64;
            ans = ans.max(l).max(r);
            r -= x as i64;
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var maximumSumScore = function (nums) {
    let l = 0;
    let r = nums.reduce((a, b) => a + b, 0);
    let ans = -Infinity;
    for (const x of nums) {
        l += x;
        ans = Math.max(ans, l, r);
        r -= x;
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
