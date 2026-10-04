---
comments: true
difficulty: Medium
rating: 1583
source: Weekly Contest 365 Q2
tags:
    - Array
---

<!-- problem:start -->

# [2874. Maximum Value of an Ordered Triplet II](https://leetcode.com/problems/maximum-value-of-an-ordered-triplet-ii)

[中文文档](/solution/2800-2899/2874.Maximum%20Value%20of%20an%20Ordered%20Triplet%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>0-indexed</strong> <code>nums</code>.</p>

<p>Trả về <em><strong>giá trị lớn nhất trong tất cả các bộ ba chỉ số</strong></em> <code>(i, j, k)</code> <em>thỏa mãn</em> <code>i &lt; j &lt; k</code><em>. </em>Nếu tất cả các bộ ba này đều có giá trị âm, trả về <code>0</code>.</p>

<p><strong>Giá trị của một bộ ba chỉ số</strong> <code>(i, j, k)</code> bằng <code>(nums[i] - nums[j]) * nums[k]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [12,6,1,2,7]
<strong>Đầu ra:</strong> 77
<strong>Giải thích:</strong> Giá trị của bộ ba (0, 2, 4) là (nums[0] - nums[2]) * nums[4] = 77.
Có thể chứng minh rằng không có bộ ba chỉ số có thứ tự nào có giá trị lớn hơn 77.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,10,3,4,19]
<strong>Đầu ra:</strong> 133
<strong>Giải thích:</strong> Giá trị của bộ ba (1, 2, 4) là (nums[1] - nums[2]) * nums[4] = 133.
Có thể chứng minh rằng không có bộ ba chỉ số có thứ tự nào có giá trị lớn hơn 133.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Bộ ba chỉ số có thứ tự duy nhất (0, 1, 2) có giá trị âm là (nums[0] - nums[1]) * nums[2] = -3. Vì vậy, đáp án là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duy trì giá trị lớn nhất ở prefix và hiệu lớn nhất

<!-- thinking:start -->

> **Tư duy**
>
> Công thức giống với bài trước, nhưng $n$ lớn hơn nên vòng lặp ba tầng sẽ không chạy được. Trong một lượt duyệt, ta vẫn duy trì giá trị lớn nhất của prefix và hiệu lớn nhất, cập nhật đáp án với $k$ hiện tại trước khi cập nhật lại hai giá trị đó.

<!-- thinking:end -->

Ta sử dụng hai biến $\textit{mx}$ và $\textit{mxDiff}$ để lần lượt duy trì giá trị lớn nhất của prefix và hiệu lớn nhất, đồng thời dùng biến $\textit{ans}$ để duy trì đáp án. Ban đầu, tất cả các biến này đều bằng $0$.

Tiếp theo, ta duyệt qua từng phần tử $x$ trong mảng với vai trò là $\textit{nums}[k]$. Trước tiên, ta cập nhật đáp án $\textit{ans} = \max(\textit{ans}, \textit{mxDiff} \times x)$. Sau đó, ta cập nhật hiệu lớn nhất $\textit{mxDiff} = \max(\textit{mxDiff}, \textit{mx} - x)$. Cuối cùng, ta cập nhật giá trị lớn nhất của prefix $\textit{mx} = \max(\textit{mx}, x)$.

Sau khi duyệt qua toàn bộ mảng, ta trả về đáp án $\textit{ans}$.

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
