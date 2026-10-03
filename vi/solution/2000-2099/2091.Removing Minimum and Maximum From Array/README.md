---
comments: true
difficulty: Medium
rating: 1384
source: Weekly Contest 269 Q3
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [2091. Removing Minimum and Maximum From Array](https://leetcode.com/problems/removing-minimum-and-maximum-from-array)

[中文文档](/solution/2000-2099/2091.Removing%20Minimum%20and%20Maximum%20From%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>, trong đó các phần tử <strong>đôi một khác nhau</strong>.</p>

<p>Trong <code>nums</code> có một phần tử có giá trị <strong>nhỏ nhất</strong> và một phần tử có giá trị <strong>lớn nhất</strong>. Lần lượt gọi chúng là phần tử <strong>nhỏ nhất</strong> và <strong>lớn nhất</strong>. Mục tiêu của bạn là xóa <strong>cả hai</strong> phần tử này khỏi mảng.</p>

<p>Một phép <strong>xóa</strong> được định nghĩa là xóa một phần tử ở <strong>đầu</strong> hoặc <strong>cuối</strong> mảng.</p>

<p>Hãy trả về <em>số phép xóa <strong>ít nhất</strong> cần thực hiện để xóa <strong>cả hai</strong> phần tử nhỏ nhất và lớn nhất khỏi mảng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,<u><strong>10</strong></u>,7,5,4,<u><strong>1</strong></u>,8,6]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Phần tử nhỏ nhất trong mảng là nums[5], có giá trị 1.
Phần tử lớn nhất trong mảng là nums[1], có giá trị 10.
Ta có thể xóa cả phần tử nhỏ nhất và lớn nhất bằng cách xóa 2 phần tử ở đầu và 3 phần tử ở cuối.
Như vậy, tổng số phép xóa là 2 + 3 = 5, đây là số phép xóa ít nhất có thể.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,<u><strong>-4</strong></u>,<u><strong>19</strong></u>,1,8,-2,-3,5]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Phần tử nhỏ nhất trong mảng là nums[1], có giá trị -4.
Phần tử lớn nhất trong mảng là nums[2], có giá trị 19.
Ta có thể xóa cả phần tử nhỏ nhất và lớn nhất bằng cách xóa 3 phần tử ở đầu.
Như vậy, tổng số phép xóa chỉ là 3, đây là số phép xóa ít nhất có thể.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [<u><strong>101</strong></u>]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Mảng chỉ có một phần tử, nên phần tử này đồng thời là phần tử nhỏ nhất và lớn nhất.
Ta có thể xóa nó với 1 phép xóa.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li>Các số nguyên trong <code>nums</code> <strong>đôi một khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ được xóa ở hai đầu, nên phải loại bỏ cả hai chỉ số của phần tử nhỏ nhất và lớn nhất. Có ba phương án: xóa từ trái qua phần tử có chỉ số lớn hơn, xóa từ phải qua phần tử có chỉ số nhỏ hơn, hoặc xóa từ cả hai đầu.
>
> Xác định $mi,mx$, sắp xếp chúng, rồi lấy $\min(mx+1,\,n-mi,\,mi+1+n-mx)$. Chỉ cần một lần duyệt để tìm hai cực trị.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumDeletions(self, nums: List[int]) -> int:
        mi = mx = 0
        for i, num in enumerate(nums):
            if num < nums[mi]:
                mi = i
            if num > nums[mx]:
                mx = i
        if mi > mx:
            mi, mx = mx, mi
        return min(mx + 1, len(nums) - mi, mi + 1 + len(nums) - mx)
```

#### Java

```java
class Solution {
    public int minimumDeletions(int[] nums) {
        int mi = 0, mx = 0, n = nums.length;
        for (int i = 0; i < n; ++i) {
            if (nums[i] < nums[mi]) {
                mi = i;
            }
            if (nums[i] > nums[mx]) {
                mx = i;
            }
        }
        if (mi > mx) {
            int t = mx;
            mx = mi;
            mi = t;
        }
        return Math.min(Math.min(mx + 1, n - mi), mi + 1 + n - mx);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumDeletions(vector<int>& nums) {
        int mi = 0, mx = 0, n = nums.size();
        for (int i = 0; i < n; ++i) {
            if (nums[i] < nums[mi]) mi = i;
            if (nums[i] > nums[mx]) mx = i;
        }
        if (mi > mx) {
            int t = mi;
            mi = mx;
            mx = t;
        }
        return min(min(mx + 1, n - mi), mi + 1 + n - mx);
    }
};
```

#### Go

```go
func minimumDeletions(nums []int) int {
	mi, mx, n := 0, 0, len(nums)
	for i, num := range nums {
		if num < nums[mi] {
			mi = i
		}
		if num > nums[mx] {
			mx = i
		}
	}
	if mi > mx {
		mi, mx = mx, mi
	}
	return min(min(mx+1, n-mi), mi+1+n-mx)
}
```

#### TypeScript

```ts
function minimumDeletions(nums: number[]): number {
    const n = nums.length;
    if (n == 1) return 1;
    let i = nums.indexOf(Math.min(...nums));
    let j = nums.indexOf(Math.max(...nums));
    let left = Math.min(i, j);
    let right = Math.max(i, j);
    // 左右 left + 1 + n - right
    // 两个都是左边 left + 1 + right - left = right + 1
    // 都是右边 n - right + right - left = n - left
    return Math.min(left + 1 + n - right, right + 1, n - left);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
