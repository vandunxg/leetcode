---
comments: true
difficulty: Easy
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [747. Largest Number At Least Twice of Others](https://leetcode.com/problems/largest-number-at-least-twice-of-others)

[中文文档](/solution/0700-0799/0747.Largest%20Number%20At%20Least%20Twice%20of%20Others/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, trong đó số nguyên lớn nhất là <strong>duy nhất</strong>.</p>

<p>Hãy xác định phần tử lớn nhất trong mảng có <strong>ít nhất gấp đôi</strong> mọi số khác hay không. Nếu có, trả về <em><strong>chỉ số</strong> của phần tử lớn nhất; nếu không, trả về </em><code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,6,1,0]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> 6 là số nguyên lớn nhất.
Với mọi số x khác trong mảng, 6 lớn ít nhất gấp đôi x.
Giá trị 6 có chỉ số là 1, nên ta trả về 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> 4 nhỏ hơn gấp đôi giá trị 3, nên ta trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 50</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 100</code></li>
	<li>Phần tử lớn nhất trong <code>nums</code> là duy nhất.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Xác định xem giá trị lớn nhất duy nhất có ít nhất gấp đôi mọi giá trị khác hay không. $n\le 50$; chỉ cần so sánh hai giá trị lớn nhất.
>
> Vì giá trị lớn nhất là duy nhất, chỉ cần kiểm tra $x\ge 2y$. Dùng `nlargest(2)` rồi tìm `index` của $x$.

<!-- thinking:end -->

Ta có thể duyệt mảng $nums$ để tìm giá trị lớn nhất $x$ và giá trị lớn thứ hai $y$. Nếu $x \ge 2y$, trả về chỉ số của $x$; nếu không, trả về $-1$.

Ngoài ra, ta có thể tìm giá trị lớn nhất $x$ và đồng thời lưu chỉ số $k$ của nó. Sau đó duyệt mảng lần nữa. Nếu gặp phần tử $y$ ở vị trí khác $k$ thỏa mãn $x < 2y$, trả về $-1$. Nếu duyệt hết mà không gặp trường hợp đó, trả về $k$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def dominantIndex(self, nums: List[int]) -> int:
        x, y = nlargest(2, nums)
        return nums.index(x) if x >= 2 * y else -1
```

#### Java

```java
class Solution {
    public int dominantIndex(int[] nums) {
        int n = nums.length;
        int k = 0;
        for (int i = 0; i < n; ++i) {
            if (nums[k] < nums[i]) {
                k = i;
            }
        }
        for (int i = 0; i < n; ++i) {
            if (k != i && nums[k] < nums[i] * 2) {
                return -1;
            }
        }
        return k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int dominantIndex(vector<int>& nums) {
        int n = nums.size();
        int k = 0;
        for (int i = 0; i < n; ++i) {
            if (nums[k] < nums[i]) {
                k = i;
            }
        }
        for (int i = 0; i < n; ++i) {
            if (k != i && nums[k] < nums[i] * 2) {
                return -1;
            }
        }
        return k;
    }
};
```

#### Go

```go
func dominantIndex(nums []int) int {
	k := 0
	for i, x := range nums {
		if nums[k] < x {
			k = i
		}
	}
	for i, x := range nums {
		if k != i && nums[k] < x*2 {
			return -1
		}
	}
	return k
}
```

#### TypeScript

```ts
function dominantIndex(nums: number[]): number {
    let k = 0;
    for (let i = 0; i < nums.length; ++i) {
        if (nums[i] > nums[k]) {
            k = i;
        }
    }
    for (let i = 0; i < nums.length; ++i) {
        if (i !== k && nums[k] < nums[i] * 2) {
            return -1;
        }
    }
    return k;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var dominantIndex = function (nums) {
    let k = 0;
    for (let i = 0; i < nums.length; ++i) {
        if (nums[i] > nums[k]) {
            k = i;
        }
    }
    for (let i = 0; i < nums.length; ++i) {
        if (i !== k && nums[k] < nums[i] * 2) {
            return -1;
        }
    }
    return k;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
