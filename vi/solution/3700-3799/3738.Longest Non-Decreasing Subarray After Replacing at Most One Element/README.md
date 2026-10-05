---
comments: true
difficulty: Medium
rating: 1811
source: Biweekly Contest 169 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3738. Longest Non-Decreasing Subarray After Replacing at Most One Element](https://leetcode.com/problems/longest-non-decreasing-subarray-after-replacing-at-most-one-element)

[中文文档](/solution/3700-3799/3738.Longest%20Non-Decreasing%20Subarray%20After%20Replacing%20at%20Most%20One%20Element/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Bạn được phép thay thế <strong>nhiều nhất</strong> một phần tử trong mảng bằng một giá trị nguyên bất kỳ do bạn chọn.</p>

<p>Hãy trả về độ dài của <strong><span data-keyword="subarray">mảng con</span> không giảm dài nhất</strong> có thể thu được sau khi thực hiện nhiều nhất một lần thay thế.</p>

<p>Một mảng được gọi là <strong>không giảm</strong> nếu mỗi phần tử lớn hơn hoặc bằng phần tử đứng trước nó (nếu có).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thay <code>nums[3] = 1</code> bằng 3, ta được mảng [1, 2, 3, 3, 2].</p>

<p>Mảng con không giảm dài nhất là [1, 2, 3, 3], có độ dài bằng 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,2,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả phần tử trong <code>nums</code> đều bằng nhau, nên mảng đã không giảm và toàn bộ <code>nums</code> tạo thành một mảng con có độ dài 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân rã tiền tố và hậu tố + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi thay đổi nhiều nhất một chỉ số, đoạn không giảm dài nhất có thể đi qua chỉ số đó. Ta tiền xử lý độ dài đoạn không giảm kết thúc hoặc bắt đầu tại mỗi $i$. Khi thay thế chỉ số $i$, ta nối được hai đoạn bên trái và bên phải nếu $nums[i-1]\le nums[i+1]$; nếu không, ta giữ phía dài hơn cùng với ô được thay thế.

<!-- thinking:end -->

Ta có thể dùng hai mảng $\textit{left}$ và $\textit{right}$ để lưu độ dài của mảng con không giảm dài nhất kết thúc và bắt đầu tại mỗi vị trí tương ứng. Ban đầu, $\textit{left}[i] = 1$ và $\textit{right}[i] = 1$.

Sau đó, ta duyệt mảng trong phạm vi $[1, n-1]$. Nếu $\textit{nums}[i] \geq \textit{nums}[i-1]$, ta cập nhật $\textit{left}[i]$ thành $\textit{left}[i-1] + 1$. Tương tự, ta duyệt ngược mảng trong phạm vi $[n-2, 0]$. Nếu $\textit{nums}[i] \leq \textit{nums}[i+1]$, ta cập nhật $\textit{right}[i]$ thành $\textit{right}[i+1] + 1$.

Tiếp theo, ta có thể tính đáp án cuối cùng bằng cách liệt kê từng vị trí. Với mỗi vị trí $i$, ta có thể tính độ dài của mảng con không giảm dài nhất có tâm tại $i$ như sau:

1. Nếu các phần tử ở bên trái và bên phải của $i$ không thỏa mãn $\textit{nums}[i-1] \leq \textit{nums}[i+1]$, ta chỉ có thể chọn mảng con không giảm từ bên trái hoặc bên phải, nên đáp án là $\max(\textit{left}[i-1], \textit{right}[i+1]) + 1$.
2. Ngược lại, ta có thể thay thế vị trí $i$ bằng một giá trị phù hợp để nối các mảng con không giảm ở bên trái và bên phải, nên đáp án là $\textit{left}[i-1] + \textit{right}[i+1] + 1$.

Cuối cùng, ta lấy giá trị lớn nhất trong tất cả các vị trí làm đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSubarray(self, nums: List[int]) -> int:
        n = len(nums)
        left = [1] * n
        right = [1] * n
        for i in range(1, n):
            if nums[i] >= nums[i - 1]:
                left[i] = left[i - 1] + 1
        for i in range(n - 2, -1, -1):
            if nums[i] <= nums[i + 1]:
                right[i] = right[i + 1] + 1
        ans = max(left)
        for i in range(n):
            a = 0 if i - 1 < 0 else left[i - 1]
            b = 0 if i + 1 >= n else right[i + 1]
            if i - 1 >= 0 and i + 1 < n and nums[i - 1] > nums[i + 1]:
                ans = max(ans, a + 1, b + 1)
            else:
                ans = max(ans, a + b + 1)
        return ans
```

#### Java

```java
class Solution {
    public int longestSubarray(int[] nums) {
        int n = nums.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, 1);
        Arrays.fill(right, 1);
        int ans = 1;

        for (int i = 1; i < n; i++) {
            if (nums[i] >= nums[i - 1]) {
                left[i] = left[i - 1] + 1;
                ans = Math.max(ans, left[i]);
            }
        }

        for (int i = n - 2; i >= 0; i--) {
            if (nums[i] <= nums[i + 1]) {
                right[i] = right[i + 1] + 1;
            }
        }

        for (int i = 0; i < n; i++) {
            int a = (i - 1 < 0) ? 0 : left[i - 1];
            int b = (i + 1 >= n) ? 0 : right[i + 1];
            if (i - 1 >= 0 && i + 1 < n && nums[i - 1] > nums[i + 1]) {
                ans = Math.max(ans, Math.max(a + 1, b + 1));
            } else {
                ans = Math.max(ans, a + b + 1);
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
    int longestSubarray(vector<int>& nums) {
        int n = nums.size();
        vector<int> left(n, 1), right(n, 1);

        for (int i = 1; i < n; ++i) {
            if (nums[i] >= nums[i - 1]) {
                left[i] = left[i - 1] + 1;
            }
        }

        for (int i = n - 2; i >= 0; --i) {
            if (nums[i] <= nums[i + 1]) {
                right[i] = right[i + 1] + 1;
            }
        }

        int ans = ranges::max(left);

        for (int i = 0; i < n; ++i) {
            int a = (i - 1 < 0) ? 0 : left[i - 1];
            int b = (i + 1 >= n) ? 0 : right[i + 1];
            if (i - 1 >= 0 && i + 1 < n && nums[i - 1] > nums[i + 1]) {
                ans = max({ans, a + 1, b + 1});
            } else {
                ans = max(ans, a + b + 1);
            }
        }

        return ans;
    }
};
```

#### Go

```go
func longestSubarray(nums []int) int {
	n := len(nums)
	left := make([]int, n)
	right := make([]int, n)
	for i := range left {
		left[i], right[i] = 1, 1
	}

	for i := 1; i < n; i++ {
		if nums[i] >= nums[i-1] {
			left[i] = left[i-1] + 1
		}
	}

	for i := n - 2; i >= 0; i-- {
		if nums[i] <= nums[i+1] {
			right[i] = right[i+1] + 1
		}
	}

	ans := slices.Max(left)

	for i := 0; i < n; i++ {
		a := 0
		if i > 0 {
			a = left[i-1]
		}
		b := 0
		if i+1 < n {
			b = right[i+1]
		}
		if i > 0 && i+1 < n && nums[i-1] > nums[i+1] {
			ans = max(ans, max(a+1, b+1))
		} else {
			ans = max(ans, a+b+1)
		}
	}

	return ans
}
```

#### TypeScript

```ts
function longestSubarray(nums: number[]): number {
    const n = nums.length;
    const left: number[] = Array(n).fill(1);
    const right: number[] = Array(n).fill(1);

    for (let i = 1; i < n; i++) {
        if (nums[i] >= nums[i - 1]) {
            left[i] = left[i - 1] + 1;
        }
    }

    for (let i = n - 2; i >= 0; i--) {
        if (nums[i] <= nums[i + 1]) {
            right[i] = right[i + 1] + 1;
        }
    }

    let ans = Math.max(...left);

    for (let i = 0; i < n; i++) {
        const a = i - 1 < 0 ? 0 : left[i - 1];
        const b = i + 1 >= n ? 0 : right[i + 1];
        if (i - 1 >= 0 && i + 1 < n && nums[i - 1] > nums[i + 1]) {
            ans = Math.max(ans, Math.max(a + 1, b + 1));
        } else {
            ans = Math.max(ans, a + b + 1);
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
