---
comments: true
difficulty: Easy
rating: 1206
source: Weekly Contest 334 Q1
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [2574. Left and Right Sum Differences](https://leetcode.com/problems/left-and-right-sum-differences)

[中文文档](/solution/2500-2599/2574.Left%20and%20Right%20Sum%20Differences/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> có kích thước <code>n</code>.</p>

<p>Định nghĩa hai mảng <code>leftSum</code> và <code>rightSum</code> như sau:</p>

<ul>
	<li><code>leftSum[i]</code> là tổng các phần tử nằm bên trái chỉ số <code>i</code> trong mảng <code>nums</code>. Nếu không có phần tử nào như vậy, <code>leftSum[i] = 0</code>.</li>
	<li><code>rightSum[i]</code> là tổng các phần tử nằm bên phải chỉ số <code>i</code> trong mảng <code>nums</code>. Nếu không có phần tử nào như vậy, <code>rightSum[i] = 0</code>.</li>
</ul>

<p>Hãy trả về một mảng số nguyên <code>answer</code> có kích thước <code>n</code>, trong đó <code>answer[i] = |leftSum[i] - rightSum[i]|</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,4,8,3]
<strong>Đầu ra:</strong> [15,1,11,22]
<strong>Giải thích:</strong> Mảng leftSum là [0,10,14,22] và mảng rightSum là [15,11,3,0].
Mảng answer là [|0 - 15|,|10 - 11|,|14 - 3|,|22 - 0|] = [15,1,11,22].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1]
<strong>Đầu ra:</strong> [0]
<strong>Giải thích:</strong> Mảng leftSum là [0] và mảng rightSum là [0].
Mảng answer là [|0 - 0|] = [0].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chỉ số cần hiệu tuyệt đối giữa tổng bên trái và tổng bên phải. Có thể dùng hai mảng tổng tiền tố, nhưng chỉ cần một lần duyệt: bắt đầu với tổng ở bên phải, trừ $x$ trước khi ghi $|l-r|$, rồi cộng $x$ vào tổng bên trái.

<!-- thinking:end -->

Ta định nghĩa biến $l$ biểu diễn tổng các phần tử nằm bên trái chỉ số $i$ trong mảng $\textit{nums}$, và biến $r$ biểu diễn tổng các phần tử nằm bên phải chỉ số $i$ trong mảng $\textit{nums}$. Ban đầu, $l = 0$, $r = \sum_{i = 0}^{n - 1} \textit{nums}[i]$.

Ta duyệt qua mảng $\textit{nums}$. Với số hiện tại $x$, ta cập nhật $r = r - x$. Lúc này, $l$ và $r$ lần lượt biểu diễn tổng các phần tử nằm bên trái và bên phải chỉ số $i$ trong mảng $\textit{nums}$. Ta thêm hiệu tuyệt đối giữa $l$ và $r$ vào mảng đáp án $\textit{ans}$, sau đó cập nhật $l = l + x$.

Sau khi duyệt xong, ta trả về mảng đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$, không tính phần không gian dành cho giá trị trả về.

Bài toán tương tự:

- [0724. Find Pivot Index](https://github.com/doocs/leetcode/blob/main/solution/0700-0799/0724.Find%20Pivot%20Index/README_EN.md)
- [1991. Find the Middle Index in Array](https://github.com/doocs/leetcode/blob/main/solution/1900-1999/1991.Find%20the%20Middle%20Index%20in%20Array/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def leftRightDifference(self, nums: List[int]) -> List[int]:
        l, r = 0, sum(nums)
        ans = []
        for x in nums:
            r -= x
            ans.append(abs(l - r))
            l += x
        return ans
```

#### Java

```java
class Solution {
    public int[] leftRightDifference(int[] nums) {
        int l = 0, r = 0;
        for (int x : nums) {
            r += x;
        }
        int n = nums.length;
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            r -= nums[i];
            ans[i] = Math.abs(l - r);
            l += nums[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> leftRightDifference(vector<int>& nums) {
        int l = 0, r = 0;
        for (int x : nums) {
            r += x;
        }
        int n = nums.size();
        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            r -= nums[i];
            ans[i] = abs(l - r);
            l += nums[i];
        }
        return ans;
    }
};
```

#### Go

```go
func leftRightDifference(nums []int) []int {
	l, r := 0, 0
	for _, x := range nums {
		r += x
	}
	n := len(nums)
	ans := make([]int, n)
	for i, x := range nums {
		r -= x
		ans[i] = abs(l - r)
		l += x
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function leftRightDifference(nums: number[]): number[] {
    let [l, r] = [0, nums.reduce((a, b) => a + b, 0)];
    const ans: number[] = [];
    for (const x of nums) {
        r -= x;
        ans.push(Math.abs(l - r));
        l += x;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn left_right_difference(nums: Vec<i32>) -> Vec<i32> {
        let mut l = 0;
        let mut r: i32 = nums.iter().sum();
        let mut ans = Vec::with_capacity(nums.len());
        for x in nums {
            r -= x;
            ans.push((l - r).abs());
            l += x;
        }
        ans
    }
}
```

#### C

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* leftRightDifference(int* nums, int numsSize, int* returnSize) {
    *returnSize = numsSize;
    int* ans = (int*) malloc(sizeof(int) * numsSize);

    int l = 0, r = 0;
    for (int i = 0; i < numsSize; ++i) {
        r += nums[i];
    }

    for (int i = 0; i < numsSize; ++i) {
        r -= nums[i];
        ans[i] = abs(l - r);
        l += nums[i];
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
