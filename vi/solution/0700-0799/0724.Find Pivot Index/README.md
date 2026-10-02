---
comments: true
difficulty: Easy
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [724. Find Pivot Index](https://leetcode.com/problems/find-pivot-index)

[中文文档](/solution/0700-0799/0724.Find%20Pivot%20Index/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy tính <strong>pivot index</strong> của mảng.</p>

<p><strong>Pivot index</strong> là chỉ số mà tổng các số nằm <strong>hoàn toàn</strong> bên trái bằng tổng các số nằm <strong>hoàn toàn</strong> bên phải chỉ số đó.</p>

<p>Nếu chỉ số nằm ở mép trái của mảng, tổng bên trái bằng <code>0</code> vì không có phần tử nào ở đó. Điều tương tự cũng áp dụng cho mép phải của mảng.</p>

<p>Trả về <em><strong>pivot index ngoài cùng bên trái</strong></em>. Nếu không tồn tại chỉ số nào như vậy, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,7,3,6,5,6]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Pivot index là 3.
Tổng bên trái = nums[0] + nums[1] + nums[2] = 1 + 7 + 3 = 11
Tổng bên phải = nums[4] + nums[5] = 5 + 6 = 11
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong>
Không có chỉ số nào thỏa mãn các điều kiện trong đề bài.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,-1]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Pivot index là 0.
Tổng bên trái = 0 (không có phần tử nào ở bên trái chỉ số 0)
Tổng bên phải = nums[1] + nums[2] = 1 + -1 = 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-1000 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài này giống bài&nbsp;1991:&nbsp;<a href="https://leetcode.com/problems/find-the-middle-index-in-array/" target="_blank">https://leetcode.com/problems/find-the-middle-index-in-array/</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Tìm chỉ số có tổng bên trái bằng tổng bên phải. Tính lại hai tổng ở mỗi chỉ số $i$ sẽ tốn $O(n^2)$ khi $n\le 10^4$.
>
> Tổng hai bên cộng với $nums[i]$ bằng tổng toàn mảng $S$, nên ta có $2\cdot\textit{left}+nums[i]=S$. Chỉ cần duyệt từ trái sang phải một lượt.
>
> Khởi tạo $\textit{right}$ bằng tổng toàn mảng, trừ $x$ trước khi so sánh với $\textit{left}$, rồi cộng $x$ vào tổng bên trái. Bộ nhớ phụ là $O(1)$.

<!-- thinking:end -->

Định nghĩa biến $left$ là tổng các phần tử nằm bên trái chỉ số $i$ trong mảng $\textit{nums}$, còn $right$ là tổng các phần tử nằm bên phải chỉ số $i$ trong mảng $\textit{nums}$. Ban đầu, $left = 0$, $right = \sum_{i = 0}^{n - 1} nums[i]$.

Duyệt mảng $\textit{nums}$. Với số hiện tại $x$, cập nhật $right = right - x$. Lúc này, nếu $left = right$, chỉ số $i$ hiện tại là pivot index và ta có thể trả về ngay. Nếu không, cập nhật $left = left + x$ rồi tiếp tục với phần tử kế tiếp.

Nếu duyệt hết mà không tìm thấy pivot index, trả về $-1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài mảng $\textit{nums}$.

Bài toán liên quan:

- [1991. Find the Middle Index in Array](https://github.com/doocs/leetcode/blob/main/solution/1900-1999/1991.Find%20the%20Middle%20Index%20in%20Array/README_EN.md)
- [2574. Left and Right Sum Differences](https://github.com/doocs/leetcode/blob/main/solution/2500-2599/2574.Left%20and%20Right%20Sum%20Differences/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pivotIndex(self, nums: List[int]) -> int:
        left, right = 0, sum(nums)
        for i, x in enumerate(nums):
            right -= x
            if left == right:
                return i
            left += x
        return -1
```

#### Java

```java
class Solution {
    public int pivotIndex(int[] nums) {
        int left = 0, right = Arrays.stream(nums).sum();
        for (int i = 0; i < nums.length; ++i) {
            right -= nums[i];
            if (left == right) {
                return i;
            }
            left += nums[i];
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        int left = 0, right = accumulate(nums.begin(), nums.end(), 0);
        for (int i = 0; i < nums.size(); ++i) {
            right -= nums[i];
            if (left == right) {
                return i;
            }
            left += nums[i];
        }
        return -1;
    }
};
```

#### Go

```go
func pivotIndex(nums []int) int {
	var left, right int
	for _, x := range nums {
		right += x
	}
	for i, x := range nums {
		right -= x
		if left == right {
			return i
		}
		left += x
	}
	return -1
}
```

#### TypeScript

```ts
function pivotIndex(nums: number[]): number {
    let left = 0,
        right = nums.reduce((a, b) => a + b);
    for (let i = 0; i < nums.length; ++i) {
        right -= nums[i];
        if (left == right) {
            return i;
        }
        left += nums[i];
    }
    return -1;
}
```

#### Rust

```rust
impl Solution {
    pub fn pivot_index(nums: Vec<i32>) -> i32 {
        let (mut left, mut right): (i32, i32) = (0, nums.iter().sum());
        for i in 0..nums.len() {
            right -= nums[i];
            if left == right {
                return i as i32;
            }
            left += nums[i];
        }
        -1
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var pivotIndex = function (nums) {
    let left = 0,
        right = nums.reduce((a, b) => a + b);
    for (let i = 0; i < nums.length; ++i) {
        right -= nums[i];
        if (left == right) {
            return i;
        }
        left += nums[i];
    }
    return -1;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
