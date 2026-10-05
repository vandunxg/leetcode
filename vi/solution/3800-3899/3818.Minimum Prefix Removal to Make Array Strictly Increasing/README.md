---
comments: true
difficulty: Medium
rating: 1206
source: Weekly Contest 486 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3818. Minimum Prefix Removal to Make Array Strictly Increasing](https://leetcode.com/problems/minimum-prefix-removal-to-make-array-strictly-increasing)

[中文文档](/solution/3800-3899/3818.Minimum%20Prefix%20Removal%20to%20Make%20Array%20Strictly%20Increasing/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Bạn cần xóa <strong>đúng</strong> một tiền tố (có thể rỗng) khỏi nums.</p>

<p>Trả về một số nguyên biểu thị độ dài <strong>nhỏ nhất</strong> của <span data-keyword="array-prefix">tiền tố</span> được xóa sao cho mảng còn lại <strong><span data-keyword="strictly-increasing-array">tăng nghiêm ngặt</span></strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,-1,2,3,3,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xóa <code>prefix = [1, -1, 2, 3]</code> sẽ để lại mảng <code>[3, 4, 5]</code>, là mảng tăng nghiêm ngặt.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,3,-2,-5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xóa <code>prefix = [4, 3, -2]</code> sẽ để lại mảng <code>[-5]</code>, là mảng tăng nghiêm ngặt.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng <code>nums = [1, 2, 3, 4]</code> đã tăng nghiêm ngặt, nên chỉ cần xóa một tiền tố rỗng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup>​​​​​​​</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt ngược

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải xóa một tiền tố (có thể rỗng) để phần còn lại tăng nghiêm ngặt, đồng thời tiền tố cần xóa phải ngắn nhất. Với $n \le 10^5$, không thể thử mọi tiền tố.
>
> Phần còn lại là một hậu tố tự nó tăng nghiêm ngặt. Tiền tố ngắn nhất chính là phần bù của hậu tố dài nhất thỏa mãn điều kiện này.
>
> Khi duyệt từ phải sang trái, điểm giảm đầu tiên $nums[i-1] \ge nums[i]$ sẽ chặn hậu tố; đáp án là $i$.
>
> Nếu không có điểm giảm nào, toàn bộ mảng tăng nghiêm ngặt và đáp án là $0$.

<!-- thinking:end -->

Ta có thể duyệt mảng từ cuối về đầu để tìm vị trí đầu tiên $i$ không thỏa điều kiện tăng nghiêm ngặt, tức là $nums[i-1] \geq nums[i]$. Tại vị trí này, độ dài nhỏ nhất của tiền tố cần xóa là $i$.

Nếu toàn bộ mảng tăng nghiêm ngặt, ta không cần xóa tiền tố nào và trả về $0$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumPrefixLength(self, nums: List[int]) -> int:
        for i in range(len(nums) - 1, 0, -1):
            if nums[i - 1] >= nums[i]:
                return i
        return 0
```

#### Java

```java
class Solution {
    public int minimumPrefixLength(int[] nums) {
        for (int i = nums.length - 1; i > 0; --i) {
            if (nums[i - 1] >= nums[i]) {
                return i;
            }
        }
        return 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumPrefixLength(vector<int>& nums) {
        for (int i = nums.size() - 1; i; --i) {
            if (nums[i - 1] >= nums[i]) {
                return i;
            }
        }
        return 0;
    }
};
```

#### Go

```go
func minimumPrefixLength(nums []int) int {
	for i := len(nums) - 1; i > 0; i-- {
		if nums[i-1] >= nums[i] {
			return i
		}
	}
	return 0
}
```

#### TypeScript

```ts
function minimumPrefixLength(nums: number[]): number {
    for (let i = nums.length - 1; i; --i) {
        if (nums[i - 1] >= nums[i]) {
            return i;
        }
    }
    return 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
