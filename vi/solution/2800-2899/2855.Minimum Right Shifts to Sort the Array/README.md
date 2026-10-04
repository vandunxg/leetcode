---
comments: true
difficulty: Easy
rating: 1379
source: Biweekly Contest 113 Q1
tags:
    - Array
---

<!-- problem:start -->

# [2855. Minimum Right Shifts to Sort the Array](https://leetcode.com/problems/minimum-right-shifts-to-sort-the-array)

[中文文档](/solution/2800-2899/2855.Minimum%20Right%20Shifts%20to%20Sort%20the%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <strong>đánh chỉ số từ 0</strong> <code>nums</code> có độ dài <code>n</code>, chứa các số nguyên dương <strong>phân biệt</strong>. Hãy trả về <em><strong>số lượng nhỏ nhất</strong> các <strong>lần dịch phải</strong> cần thực hiện để sắp xếp</em> <code>nums</code><em>, hoặc </em><code>-1</code><em> nếu không thể.</em></p>

<p>Một <strong>lần dịch phải</strong> được định nghĩa là dịch phần tử tại chỉ số <code>i</code> đến chỉ số <code>(i + 1) % n</code>, với mọi chỉ số.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,5,1,2]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Sau lần dịch phải thứ nhất, nums = [2,3,4,5,1].
Sau lần dịch phải thứ hai, nums = [1,2,3,4,5].
Lúc này nums đã được sắp xếp; do đó, đáp án là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,5]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> nums đã được sắp xếp, do đó đáp án là 0.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,4]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể sắp xếp mảng bằng các lần dịch phải.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>nums</code> chứa các số nguyên phân biệt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng có thể sắp xếp được có nhiều nhất một vị trí giảm, và dãy thứ hai phải luôn nhỏ hơn $nums[0]$. Sau lần giảm đầu tiên, duyệt phần còn lại; nếu duyệt đến cuối mảng, cần $n-i$ lần dịch phải để khôi phục thứ tự.

<!-- thinking:end -->

Trước hết, ta dùng một con trỏ $i$ để duyệt mảng $nums$ từ trái sang phải, tìm một dãy tăng liên tiếp cho đến khi $i$ đến cuối mảng hoặc $nums[i - 1] > nums[i]$. Tiếp theo, ta dùng một con trỏ $k$ khác để duyệt mảng $nums$ từ $i + 1$, tìm một dãy tăng liên tiếp cho đến khi $k$ đến cuối mảng hoặc $nums[k - 1] > nums[k]$ và $nums[k] > nums[0]$. Nếu $k$ đến cuối mảng, nghĩa là mảng đã tăng dần, nên ta trả về $n - i$; ngược lại, ta trả về $-1$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(1)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumRightShifts(self, nums: List[int]) -> int:
        n = len(nums)
        i = 1
        while i < n and nums[i - 1] < nums[i]:
            i += 1
        k = i + 1
        while k < n and nums[k - 1] < nums[k] < nums[0]:
            k += 1
        return -1 if k < n else n - i
```

#### Java

```java
class Solution {
    public int minimumRightShifts(List<Integer> nums) {
        int n = nums.size();
        int i = 1;
        while (i < n && nums.get(i - 1) < nums.get(i)) {
            ++i;
        }
        int k = i + 1;
        while (k < n && nums.get(k - 1) < nums.get(k) && nums.get(k) < nums.get(0)) {
            ++k;
        }
        return k < n ? -1 : n - i;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumRightShifts(vector<int>& nums) {
        int n = nums.size();
        int i = 1;
        while (i < n && nums[i - 1] < nums[i]) {
            ++i;
        }
        int k = i + 1;
        while (k < n && nums[k - 1] < nums[k] && nums[k] < nums[0]) {
            ++k;
        }
        return k < n ? -1 : n - i;
    }
};
```

#### Go

```go
func minimumRightShifts(nums []int) int {
	n := len(nums)
	i := 1
	for i < n && nums[i-1] < nums[i] {
		i++
	}
	k := i + 1
	for k < n && nums[k-1] < nums[k] && nums[k] < nums[0] {
		k++
	}
	if k < n {
		return -1
	}
	return n - i
}
```

#### TypeScript

```ts
function minimumRightShifts(nums: number[]): number {
    const n = nums.length;
    let i = 1;
    while (i < n && nums[i - 1] < nums[i]) {
        ++i;
    }
    let k = i + 1;
    while (k < n && nums[k - 1] < nums[k] && nums[k] < nums[0]) {
        ++k;
    }
    return k < n ? -1 : n - i;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
