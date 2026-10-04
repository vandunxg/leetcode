---
comments: true
difficulty: Medium
rating: 2030
source: Weekly Contest 369 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2919. Minimum Increment Operations to Make Array Beautiful](https://leetcode.com/problems/minimum-increment-operations-to-make-array-beautiful)

[中文文档](/solution/2900-2999/2919.Minimum%20Increment%20Operations%20to%20Make%20Array%20Beautiful/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>nums</code> có độ dài <code>n</code>, và một số nguyên <code>k</code>.</p>

<p>Bạn có thể thực hiện thao tác <strong>tăng</strong> sau đây <strong>bất kỳ</strong> số lần nào (<strong>kể cả 0</strong>):</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> trong khoảng <code>[0, n - 1]</code>, và tăng <code>nums[i]</code> thêm <code>1</code>.</li>
</ul>

<p>Một mảng được xem là <strong>đẹp</strong> nếu, với mọi <strong>mảng con</strong> có kích thước <code>3</code> hoặc <strong>lớn hơn</strong>, phần tử <strong>lớn nhất</strong> của nó <strong>lớn hơn hoặc bằng</strong> <code>k</code>.</p>

<p>Hãy trả về <em>một số nguyên biểu thị số thao tác tăng <strong>nhỏ nhất</strong> cần thực hiện để biến </em><code>nums</code><em> thành một mảng <strong>đẹp</strong>.</em></p>

<p>Mảng con là một dãy phần tử liên tiếp <strong>không rỗng</strong> trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,0,0,2], k = 4
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác tăng sau để biến nums thành một mảng đẹp:
Chọn chỉ số i = 1 và tăng nums[1] thêm 1 -&gt; [2,4,0,0,2].
Chọn chỉ số i = 4 và tăng nums[4] thêm 1 -&gt; [2,4,0,0,3].
Chọn chỉ số i = 4 và tăng nums[4] thêm 1 -&gt; [2,4,0,0,4].
Các mảng con có kích thước từ 3 trở lên là: [2,4,0], [4,0,0], [0,0,4], [2,4,0,0], [4,0,0,4], [2,4,0,0,4].
Trong tất cả các mảng con, phần tử lớn nhất đều bằng k = 4, nên nums hiện là một mảng đẹp.
Có thể chứng minh rằng không thể biến nums thành một mảng đẹp với ít hơn 3 thao tác tăng.
Vì vậy, đáp án là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,3,3], k = 5
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác tăng sau để biến nums thành một mảng đẹp:
Chọn chỉ số i = 2 và tăng nums[2] thêm 1 -&gt; [0,1,4,3].
Chọn chỉ số i = 2 và tăng nums[2] thêm 1 -&gt; [0,1,5,3].
Các mảng con có kích thước từ 3 trở lên là: [0,1,5], [1,5,3], [0,1,5,3].
Trong tất cả các mảng con, phần tử lớn nhất đều bằng k = 5, nên nums hiện là một mảng đẹp.
Có thể chứng minh rằng không thể biến nums thành một mảng đẹp với ít hơn 2 thao tác tăng.
Vì vậy, đáp án là 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,2], k = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mảng con duy nhất có kích thước từ 3 trở lên trong ví dụ này là [1,1,2].
Phần tử lớn nhất là 2, vốn đã lớn hơn k = 1, nên ta không cần thực hiện thao tác tăng nào.
Vì vậy, đáp án là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Mọi cửa sổ có độ dài $3$ phải có giá trị lớn nhất ít nhất bằng $k$. Vì $n \le 10^5$, ta không thể tính chi phí riêng cho từng cửa sổ. Tăng chỉ số $i$ thêm $\max(k-nums[i],0)$ sẽ bao phủ mọi cửa sổ chứa $i$, và mỗi cửa sổ chỉ cần một phần tử được bao phủ như vậy.
>
> Do đó, trạng thái cần theo dõi cách rẻ nhất để bao phủ bằng một trong ba vị trí cuối. Ba giá trị cuộn $f,g,h$ lưu ba phương án tối ưu đó; khi đọc $x$, ta cập nhật chúng bằng $\min(f,g,h)+\max(k-x,0)$. Đáp án là giá trị nhỏ nhất trong ba giá trị này.

<!-- thinking:end -->

Ta định nghĩa $f$, $g$ và $h$ là số thao tác tăng nhỏ nhất cần thực hiện để giá trị lớn nhất của ba phần tử cuối cùng trong $i$ phần tử đầu tiên đạt yêu cầu, ban đầu $f = 0$, $g = 0$, $h = 0$.

Tiếp theo, ta duyệt mảng $nums$. Với mỗi $x$, ta cần cập nhật các giá trị của $f$, $g$ và $h$ để thỏa mãn yêu cầu của bài toán, cụ thể:

$$
\begin{aligned}
f' &= g \\
g' &= h \\
h' &= \min(f, g, h) + \max(k - x, 0)
\end{aligned}
$$

Cuối cùng, ta chỉ cần trả về giá trị nhỏ nhất trong $f$, $g$ và $h$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minIncrementOperations(self, nums: List[int], k: int) -> int:
        f = g = h = 0
        for x in nums:
            f, g, h = g, h, min(f, g, h) + max(k - x, 0)
        return min(f, g, h)
```

#### Java

```java
class Solution {
    public long minIncrementOperations(int[] nums, int k) {
        long f = 0, g = 0, h = 0;
        for (int x : nums) {
            long hh = Math.min(Math.min(f, g), h) + Math.max(k - x, 0);
            f = g;
            g = h;
            h = hh;
        }
        return Math.min(Math.min(f, g), h);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minIncrementOperations(vector<int>& nums, int k) {
        long long f = 0, g = 0, h = 0;
        for (int x : nums) {
            long long hh = min({f, g, h}) + max(k - x, 0);
            f = g;
            g = h;
            h = hh;
        }
        return min({f, g, h});
    }
};
```

#### Go

```go
func minIncrementOperations(nums []int, k int) int64 {
	var f, g, h int
	for _, x := range nums {
		f, g, h = g, h, min(f, g, h)+max(k-x, 0)
	}
	return int64(min(f, g, h))
}
```

#### TypeScript

```ts
function minIncrementOperations(nums: number[], k: number): number {
    let [f, g, h] = [0, 0, 0];
    for (const x of nums) {
        [f, g, h] = [g, h, Math.min(f, g, h) + Math.max(k - x, 0)];
    }
    return Math.min(f, g, h);
}
```

#### C#

```cs
public class Solution {
    public long MinIncrementOperations(int[] nums, int k) {
        long f = 0, g = 0, h = 0;
        foreach (int x in nums) {
            long hh = Math.Min(Math.Min(f, g), h) + Math.Max(k - x, 0);
            f = g;
            g = h;
            h = hh;
        }
        return Math.Min(Math.Min(f, g), h);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
