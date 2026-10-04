---
comments: true
difficulty: Medium
rating: 1763
source: Weekly Contest 454 Q3
tags:
    - Array
    - Two Pointers
---

<!-- problem:start -->

# [3584. Maximum Product of First and Last Elements of a Subsequence](https://leetcode.com/problems/maximum-product-of-first-and-last-elements-of-a-subsequence)

[中文文档](/solution/3500-3599/3584.Maximum%20Product%20of%20First%20and%20Last%20Elements%20of%20a%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>m</code>.</p>

<p>Hãy trả về tích <strong>lớn nhất</strong> của phần tử đầu tiên và phần tử cuối cùng của bất kỳ <strong><span data-keyword="subsequence-array">dãy con</span></strong> nào của <code>nums</code> có kích thước <code>m</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-1,-9,2,3,-2,-3,1], m = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">81</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con <code>[-9]</code> có tích của phần tử đầu tiên và phần tử cuối cùng lớn nhất: <code>-9 * -9 = 81</code>. Do đó, đáp án là 81.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,-5,5,6,-4], m = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">20</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con <code>[-5, 6, -4]</code> có tích của phần tử đầu tiên và phần tử cuối cùng lớn nhất.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,-1,2,-6,5,2,-5,7], m = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">35</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con <code>[5, 7]</code> có tích của phần tử đầu tiên và phần tử cuối cùng lớn nhất.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + duy trì các cực trị của tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Một dãy con độ dài $m$ có tích bằng tích của phần tử đầu tiên và phần tử cuối cùng. Cố định phần tử cuối $i$; phần tử đầu tiên nằm tại hoặc trước $i-m+1$, nên các tích cực trị sử dụng giá trị nhỏ nhất và lớn nhất trong tiền tố.
>
> Khi duyệt, ta đưa $nums[i-m+1]$ vào hai cực trị đó rồi nhân $x$ hiện tại với cả hai. Một giá trị âm có thể khiến một trong hai cực trị trở thành lựa chọn tối ưu.

<!-- thinking:end -->

Ta có thể liệt kê phần tử cuối của dãy con, giả sử đó là $\textit{nums}[i]$. Khi đó, phần tử đầu tiên của dãy con có thể là $\textit{nums}[j]$, với $j \leq i - m + 1$. Vì vậy, ta dùng hai biến $\textit{mi}$ và $\textit{mx}$ để lần lượt duy trì giá trị nhỏ nhất và lớn nhất của tiền tố. Khi duyệt đến $\textit{nums}[i]$, ta cập nhật $\textit{mi}$ và $\textit{mx}$, sau đó tính tích của $\textit{nums}[i]$ với $\textit{mi}$ và $\textit{mx}$, rồi lấy giá trị lớn nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumProduct(self, nums: List[int], m: int) -> int:
        ans = mx = -inf
        mi = inf
        for i in range(m - 1, len(nums)):
            x = nums[i]
            y = nums[i - m + 1]
            mi = min(mi, y)
            mx = max(mx, y)
            ans = max(ans, x * mi, x * mx)
        return ans
```

#### Java

```java
class Solution {
    public long maximumProduct(int[] nums, int m) {
        long ans = Long.MIN_VALUE;
        int mx = Integer.MIN_VALUE;
        int mi = Integer.MAX_VALUE;
        for (int i = m - 1; i < nums.length; ++i) {
            int x = nums[i];
            int y = nums[i - m + 1];
            mi = Math.min(mi, y);
            mx = Math.max(mx, y);
            ans = Math.max(ans, Math.max(1L * x * mi, 1L * x * mx));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumProduct(vector<int>& nums, int m) {
        long long ans = LLONG_MIN;
        int mx = INT_MIN;
        int mi = INT_MAX;
        for (int i = m - 1; i < nums.size(); ++i) {
            int x = nums[i];
            int y = nums[i - m + 1];
            mi = min(mi, y);
            mx = max(mx, y);
            ans = max(ans, max(1LL * x * mi, 1LL * x * mx));
        }
        return ans;
    }
};
```

#### Go

```go
func maximumProduct(nums []int, m int) int64 {
	ans := int64(math.MinInt64)
	mx := math.MinInt32
	mi := math.MaxInt32

	for i := m - 1; i < len(nums); i++ {
		x := nums[i]
		y := nums[i-m+1]
		mi = min(mi, y)
		mx = max(mx, y)
		ans = max(ans, max(int64(x)*int64(mi), int64(x)*int64(mx)))
	}

	return ans
}
```

#### TypeScript

```ts
function maximumProduct(nums: number[], m: number): number {
    let ans = Number.MIN_SAFE_INTEGER;
    let mx = Number.MIN_SAFE_INTEGER;
    let mi = Number.MAX_SAFE_INTEGER;

    for (let i = m - 1; i < nums.length; i++) {
        const x = nums[i];
        const y = nums[i - m + 1];
        mi = Math.min(mi, y);
        mx = Math.max(mx, y);
        ans = Math.max(ans, x * mi, x * mx);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
