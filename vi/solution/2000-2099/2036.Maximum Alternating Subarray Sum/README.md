---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2036. Maximum Alternating Subarray Sum 🔒](https://leetcode.com/problems/maximum-alternating-subarray-sum)

[中文文档](/solution/2000-2099/2036.Maximum%20Alternating%20Subarray%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Mảng con</strong> của một mảng số nguyên <strong>đánh chỉ số từ 0</strong> là một dãy phần tử <strong>liên tiếp và không rỗng</strong> trong mảng.</p>

<p><strong>Tổng xen kẽ của mảng con</strong> từ chỉ số <code>i</code> đến <code>j</code> (<strong>bao gồm cả hai đầu</strong>, <code>0 &lt;= i &lt;= j &lt; nums.length</code>) là <code>nums[i] - nums[i+1] + nums[i+2] - ... +/- nums[j]</code>.</p>

<p>Cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>nums</code>, hãy trả về <em><strong>tổng xen kẽ lớn nhất của mảng con</strong> bất kỳ của </em><code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,-1,1,2]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Mảng con [3,-1,1] có tổng xen kẽ lớn nhất.
Tổng xen kẽ của mảng con là 3 - (-1) + 1 = 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,2,2,2]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Các mảng con [2], [2,2,2] và [2,2,2,2,2] có tổng xen kẽ lớn nhất.
Tổng xen kẽ của [2] là 2.
Tổng xen kẽ của [2,2,2] là 2 - 2 + 2 = 2.
Tổng xen kẽ của [2,2,2,2,2] là 2 - 2 + 2 - 2 + 2 = 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Chỉ có một mảng con không rỗng là [1].
Tổng xen kẽ là 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm tổng xen kẽ lớn nhất của mảng con; $n \le 10^5$ nên không thể liệt kê các đoạn. Hãy phân loại theo dấu cuối cùng: trạng thái mới chỉ phụ thuộc vào trạng thái đối dấu ở chỉ số trước đó.
>
> $f$ kết thúc bằng $+nums[i]$, còn $g$ kết thúc bằng $-nums[i]$. $f$ mới có thể nối tiếp từ $g$ hoặc bắt đầu lại, còn $g$ nối tiếp từ $f$ vừa được cập nhật.
>
> Dùng hai biến luân phiên để đạt $O(1)$ bộ nhớ, đồng thời lấy giá trị lớn nhất trên toàn bộ mảng.

<!-- thinking:end -->

Ta định nghĩa $f$ là tổng lớn nhất của mảng con xen kẽ kết thúc bằng $nums[i]$, và $g$ là tổng lớn nhất của mảng con xen kẽ kết thúc bằng $-nums[i]$. Ban đầu, cả $f$ và $g$ đều bằng $-\infty$.

Tiếp theo, ta duyệt qua mảng $nums$. Với vị trí $i$, ta cần duy trì các giá trị của $f$ và $g$, tức là $f = \max(g, 0) + nums[i]$, còn $g = f - nums[i]$. Đáp án là giá trị lớn nhất trong tất cả các giá trị $f$ và $g$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumAlternatingSubarraySum(self, nums: List[int]) -> int:
        ans = f = g = -inf
        for x in nums:
            f, g = max(g, 0) + x, f - x
            ans = max(ans, f, g)
        return ans
```

#### Java

```java
class Solution {
    public long maximumAlternatingSubarraySum(int[] nums) {
        final long inf = 1L << 60;
        long ans = -inf, f = -inf, g = -inf;
        for (int x : nums) {
            long ff = Math.max(g, 0) + x;
            g = f - x;
            f = ff;
            ans = Math.max(ans, Math.max(f, g));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumAlternatingSubarraySum(vector<int>& nums) {
        using ll = long long;
        const ll inf = 1LL << 60;
        ll ans = -inf, f = -inf, g = -inf;
        for (int x : nums) {
            ll ff = max(g, 0LL) + x;
            g = f - x;
            f = ff;
            ans = max({ans, f, g});
        }
        return ans;
    }
};
```

#### Go

```go
func maximumAlternatingSubarraySum(nums []int) int64 {
	const inf = 1 << 60
	ans, f, g := -inf, -inf, -inf
	for _, x := range nums {
		f, g = max(g, 0)+x, f-x
		ans = max(ans, max(f, g))
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function maximumAlternatingSubarraySum(nums: number[]): number {
    let [ans, f, g] = [-Infinity, -Infinity, -Infinity];
    for (const x of nums) {
        [f, g] = [Math.max(g, 0) + x, f - x];
        ans = Math.max(ans, f, g);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
