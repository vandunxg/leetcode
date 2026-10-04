---
comments: true
difficulty: Easy
rating: 1270
source: Weekly Contest 365 Q1
tags:
    - Array
---

<!-- problem:start -->

# [2873. Maximum Value of an Ordered Triplet I](https://leetcode.com/problems/maximum-value-of-an-ordered-triplet-i)

[中文文档](/solution/2800-2899/2873.Maximum%20Value%20of%20an%20Ordered%20Triplet%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> <strong>được đánh chỉ số từ 0</strong>.</p>

<p>Hãy trả về <em><strong>giá trị lớn nhất trong tất cả các bộ ba chỉ số</strong></em> <code>(i, j, k)</code> <em>sao cho</em> <code>i &lt; j &lt; k</code>. Nếu mọi bộ ba như vậy đều có giá trị âm, hãy trả về <code>0</code>.</p>

<p><strong>Giá trị của bộ ba chỉ số</strong> <code>(i, j, k)</code> bằng <code>(nums[i] - nums[j]) * nums[k]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [12,6,1,2,7]
<strong>Đầu ra:</strong> 77
<strong>Giải thích:</strong> Giá trị của bộ ba (0, 2, 4) là (nums[0] - nums[2]) * nums[4] = 77.
Có thể chứng minh rằng không có bộ ba chỉ số theo thứ tự nào có giá trị lớn hơn 77.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,10,3,4,19]
<strong>Đầu ra:</strong> 133
<strong>Giải thích:</strong> Giá trị của bộ ba (1, 2, 4) là (nums[1] - nums[2]) * nums[4] = 133.
Có thể chứng minh rằng không có bộ ba chỉ số theo thứ tự nào có giá trị lớn hơn 133.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Bộ ba chỉ số theo thứ tự duy nhất (0, 1, 2) có giá trị âm là (nums[0] - nums[1]) * nums[2] = -3. Vì vậy, đáp án là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duy trì Giá trị lớn nhất của tiền tố và Hiệu lớn nhất

<!-- thinking:start -->

> **Tư duy**
>
> Giá trị có dạng $(nums[i]-nums[j])\times nums[k]$ với $i<j<k$. Mặc dù $n$ nhỏ, ta chỉ cần duyệt một lần: khi coi phần tử hiện tại là $k$, duy trì giá trị lớn nhất của tiền tố $mx$ và hiệu tốt nhất $mx-nums[j]$, sau đó nhân hiệu này với $nums[k]$.

<!-- thinking:end -->

Ta sử dụng hai biến $\textit{mx}$ và $\textit{mxDiff}$ lần lượt để duy trì giá trị lớn nhất của tiền tố và hiệu lớn nhất, cùng với biến $\textit{ans}$ để duy trì đáp án. Ban đầu, tất cả các biến này đều bằng $0$.

Tiếp theo, ta duyệt qua từng phần tử $x$ trong mảng, coi nó là $\textit{nums}[k]$. Trước hết, ta cập nhật đáp án $\textit{ans} = \max(\textit{ans}, \textit{mxDiff} \times x)$. Sau đó, ta cập nhật hiệu lớn nhất $\textit{mxDiff} = \max(\textit{mxDiff}, \textit{mx} - x)$. Cuối cùng, ta cập nhật giá trị lớn nhất của tiền tố $\textit{mx} = \max(\textit{mx}, x)$.

Sau khi duyệt qua tất cả các phần tử, ta trả về đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumTripletValue(self, nums: List[int]) -> int:
        ans = mx = mx_diff = 0
        for x in nums:
            ans = max(ans, mx_diff * x)
            mx_diff = max(mx_diff, mx - x)
            mx = max(mx, x)
        return ans
```

#### Java

```java
class Solution {
    public long maximumTripletValue(int[] nums) {
        long ans = 0, mxDiff = 0;
        int mx = 0;
        for (int x : nums) {
            ans = Math.max(ans, mxDiff * x);
            mxDiff = Math.max(mxDiff, mx - x);
            mx = Math.max(mx, x);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumTripletValue(vector<int>& nums) {
        long long ans = 0, mxDiff = 0;
        int mx = 0;
        for (int x : nums) {
            ans = max(ans, mxDiff * x);
            mxDiff = max(mxDiff, 1LL * mx - x);
            mx = max(mx, x);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumTripletValue(nums []int) int64 {
	ans, mx, mxDiff := 0, 0, 0
	for _, x := range nums {
		ans = max(ans, mxDiff*x)
		mxDiff = max(mxDiff, mx-x)
		mx = max(mx, x)
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function maximumTripletValue(nums: number[]): number {
    let [ans, mx, mxDiff] = [0, 0, 0];
    for (const x of nums) {
        ans = Math.max(ans, mxDiff * x);
        mxDiff = Math.max(mxDiff, mx - x);
        mx = Math.max(mx, x);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_triplet_value(nums: Vec<i32>) -> i64 {
        let mut ans: i64 = 0;
        let mut mx: i32 = 0;
        let mut mx_diff: i32 = 0;

        for &x in &nums {
            ans = ans.max(mx_diff as i64 * x as i64);
            mx_diff = mx_diff.max(mx - x);
            mx = mx.max(x);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
