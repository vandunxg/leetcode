---
comments: true
difficulty: Easy
rating: 1580
source: Biweekly Contest 108 Q1
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [2765. Longest Alternating Subarray](https://leetcode.com/problems/longest-alternating-subarray)

[中文文档](/solution/2700-2799/2765.Longest%20Alternating%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>. Một mảng con <code>s</code> có độ dài <code>m</code> được gọi là <strong>xen kẽ</strong> nếu:</p>

<ul>
	<li><code>m</code> lớn hơn <code>1</code>.</li>
	<li><code>s<sub>1</sub> = s<sub>0</sub> + 1</code>.</li>
	<li>Mảng con <code>s</code> được đánh chỉ số từ <strong>0</strong> có dạng <code>[s<sub>0</sub>, s<sub>1</sub>, s<sub>0</sub>, s<sub>1</sub>,...,s<sub>(m-1) % 2</sub>]</code>. Nói cách khác, <code>s<sub>1</sub> - s<sub>0</sub> = 1</code>, <code>s<sub>2</sub> - s<sub>1</sub> = -1</code>, <code>s<sub>3</sub> - s<sub>2</sub> = 1</code>, <code>s<sub>4</sub> - s<sub>3</sub> = -1</code>, và tiếp tục như vậy cho đến <code>s[m - 1] - s[m - 2] = (-1)<sup>m</sup></code>.</li>
</ul>

<p>Trả về <em>độ dài lớn nhất của mọi mảng con <strong>xen kẽ</strong> xuất hiện trong </em><code>nums</code> <em>hoặc </em><code>-1</code><em> nếu không tồn tại mảng con như vậy</em><em>.</em></p>

<p>Mảng con là một dãy phần tử <strong>liền kề, không rỗng</strong> trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,4,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con xen kẽ là <code>[2, 3]</code>, <code>[3,4]</code>, <code>[3,4,3]</code> và <code>[3,4,3,4]</code>. Mảng con dài nhất là <code>[3,4,3,4]</code>, có độ dài 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,5,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>[4,5]</code> và <code>[5,6]</code> là hai mảng con xen kẽ duy nhất. Cả hai đều có độ dài 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con xen kẽ bắt đầu bằng hiệu $1$, sau đó luân phiên giữa $1$ và $-1$; ta cần tìm độ dài lớn nhất, ít nhất là $2$. Vì $n\le 100$, chỉ cần mở rộng từ mỗi vị trí bắt đầu.
>
> Từ mỗi vị trí bắt đầu $i$, ta đi sang phải với hiệu cần tìm $k=1$ và đổi dấu $k$ sau mỗi lần khớp. Cập nhật đáp án khi độ dài lớn hơn $1$; nếu không thì giữ nguyên $-1$.

<!-- thinking:end -->

Ta có thể liệt kê đầu trái $i$ của mảng con. Với mỗi $i$, ta cần tìm mảng con dài nhất thỏa mãn điều kiện. Ta bắt đầu duyệt sang phải từ $i$; mỗi khi gặp hai phần tử liền kề có hiệu không thỏa mãn điều kiện xen kẽ, ta đã tìm được một mảng con thỏa mãn điều kiện. Ta có thể dùng biến $k$ để ghi nhận hiệu của phần tử hiện tại phải là $1$ hay $-1$. Nếu hiệu của phần tử hiện tại bằng $-k$, ta đổi dấu $k$. Khi tìm được mảng con $nums[i..j]$ thỏa mãn điều kiện, ta cập nhật đáp án thành $\max(ans, j - i + 1)$.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là độ dài mảng. Ta cần liệt kê đầu trái $i$ của mảng con, và với mỗi $i$, cần $O(n)$ thời gian để tìm mảng con dài nhất thỏa mãn điều kiện. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def alternatingSubarray(self, nums: List[int]) -> int:
        ans, n = -1, len(nums)
        for i in range(n):
            k = 1
            j = i
            while j + 1 < n and nums[j + 1] - nums[j] == k:
                j += 1
                k *= -1
            if j - i + 1 > 1:
                ans = max(ans, j - i + 1)
        return ans
```

#### Java

```java
class Solution {
    public int alternatingSubarray(int[] nums) {
        int ans = -1, n = nums.length;
        for (int i = 0; i < n; ++i) {
            int k = 1;
            int j = i;
            for (; j + 1 < n && nums[j + 1] - nums[j] == k; ++j) {
                k *= -1;
            }
            if (j - i + 1 > 1) {
                ans = Math.max(ans, j - i + 1);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int alternatingSubarray(vector<int>& nums) {
        int ans = -1, n = nums.size();
        for (int i = 0; i < n; ++i) {
            int k = 1;
            int j = i;
            for (; j + 1 < n && nums[j + 1] - nums[j] == k; ++j) {
                k *= -1;
            }
            if (j - i + 1 > 1) {
                ans = max(ans, j - i + 1);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func alternatingSubarray(nums []int) int {
	ans, n := -1, len(nums)
	for i := range nums {
		k := 1
		j := i
		for ; j+1 < n && nums[j+1]-nums[j] == k; j++ {
			k *= -1
		}
		if t := j - i + 1; t > 1 && ans < t {
			ans = t
		}
	}
	return ans
}
```

#### TypeScript

```ts
function alternatingSubarray(nums: number[]): number {
    let ans = -1;
    const n = nums.length;
    for (let i = 0; i < n; ++i) {
        let k = 1;
        let j = i;
        for (; j + 1 < n && nums[j + 1] - nums[j] === k; ++j) {
            k *= -1;
        }
        if (j - i + 1 > 1) {
            ans = Math.max(ans, j - i + 1);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
