---
comments: true
difficulty: Easy
rating: 1234
source: Weekly Contest 498 Q1
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [3903. Smallest Stable Index I](https://leetcode.com/problems/smallest-stable-index-i)

[中文文档](/solution/3900-3999/3903.Smallest%20Stable%20Index%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>k</code>.</p>

<p>Với mỗi chỉ số <code>i</code>, định nghĩa <strong>điểm bất ổn</strong> là <code>max(nums[0..i]) - min(nums[i..n - 1])</code>.</p>

<p>Nói cách khác:</p>

<ul>
	<li><code>max(nums[0..i])</code> là giá trị <strong>lớn nhất</strong> trong các phần tử từ chỉ số 0 đến chỉ số <code>i</code>.</li>
	<li><code>min(nums[i..n - 1])</code> là giá trị <strong>nhỏ nhất</strong> trong các phần tử từ chỉ số <code>i</code> đến chỉ số <code>n - 1</code>.</li>
</ul>

<p>Một chỉ số <code>i</code> được gọi là <strong>ổn định</strong> nếu điểm bất ổn của nó <strong>nhỏ hơn hoặc bằng</strong> <code>k</code>.</p>

<p>Trả về chỉ số ổn định <strong>nhỏ nhất</strong>. Nếu không tồn tại chỉ số như vậy, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,0,1,4], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tại chỉ số 0: Giá trị lớn nhất trong <code>[5]</code> là 5, còn giá trị nhỏ nhất trong <code>[5, 0, 1, 4]</code> là 0, nên điểm bất ổn là <code>5 - 0 = 5</code>.</li>
	<li>Tại chỉ số 1: Giá trị lớn nhất trong <code>[5, 0]</code> là 5, còn giá trị nhỏ nhất trong <code>[0, 1, 4]</code> là 0, nên điểm bất ổn là <code>5 - 0 = 5</code>.</li>
	<li>Tại chỉ số 2: Giá trị lớn nhất trong <code>[5, 0, 1]</code> là 5, còn giá trị nhỏ nhất trong <code>[1, 4]</code> là 1, nên điểm bất ổn là <code>5 - 1 = 4</code>.</li>
	<li>Tại chỉ số 3: Giá trị lớn nhất trong <code>[5, 0, 1, 4]</code> là 5, còn giá trị nhỏ nhất trong <code>[4]</code> là 4, nên điểm bất ổn là <code>5 - 4 = 1</code>.</li>
	<li>Đây là chỉ số đầu tiên có điểm bất ổn nhỏ hơn hoặc bằng <code>k = 3</code>. Vì vậy, đáp án là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,2,1], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tại chỉ số 0, điểm bất ổn là <code>3 - 1 = 2</code>.</li>
	<li>Tại chỉ số 1, điểm bất ổn là <code>3 - 1 = 2</code>.</li>
	<li>Tại chỉ số 2, điểm bất ổn là <code>3 - 1 = 2</code>.</li>
	<li>Không giá trị nào trong số này nhỏ hơn hoặc bằng <code>k = 1</code>, nên đáp án là -1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0], k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tại chỉ số 0, điểm bất ổn là <code>0 - 0 = 0</code>, nhỏ hơn hoặc bằng <code>k = 0</code>. Do đó, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Quét lại giá trị lớn nhất của tiền tố và giá trị nhỏ nhất của hậu tố tại mỗi chỉ số sẽ tốn $O(n^2)$. Với $n\le 100$, cách này vẫn đáp ứng được, nhưng ta có thể chuẩn bị hai giá trị này chỉ trong một lần duyệt.
>
> Điểm bất ổn tại $i$ chỉ phụ thuộc vào giá trị lớn nhất trên $[0,i]$ và giá trị nhỏ nhất trên $[i,n-1]$. Giá trị thứ hai không phụ thuộc vào thứ tự duyệt $i$ và có thể được tính từ phải sang trái; còn giá trị thứ nhất tăng dần khi ta đi từ trái sang phải.
>
> Trước hết, tiền xử lý các giá trị nhỏ nhất của hậu tố vào mảng $\textit{right}$, sau đó duy trì giá trị lớn nhất của tiền tố trong biến $\textit{left}$ và trả về $i$ đầu tiên thỏa mãn $\textit{left}-\textit{right}[i]\le k$.

<!-- thinking:end -->

Trước tiên, ta tiền xử lý một mảng $\textit{right}$, trong đó $\textit{right}[i]$ biểu diễn giá trị nhỏ nhất trong các phần tử của $nums$ từ chỉ số $i$ đến chỉ số $n - 1$. Ta có thể tính mảng $\textit{right}$ bằng cách duyệt $nums$ từ cuối về đầu.

Tiếp theo, ta duyệt mảng $nums$ từ đầu đến cuối và duy trì một biến $\textit{left}$, biểu diễn giá trị lớn nhất trong các phần tử của $nums$ từ chỉ số $0$ đến chỉ số $i$. Với mỗi chỉ số $i$, ta tính điểm bất ổn là $\textit{left} - \textit{right}[i]$. Nếu điểm bất ổn nhỏ hơn hoặc bằng $k$, ta trả về chỉ số $i$. Nếu duyệt hết mảng mà không tìm thấy chỉ số nào như vậy, ta trả về $-1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def firstStableIndex(self, nums: list[int], k: int) -> int:
        n = len(nums)
        right = [nums[-1]] * n
        for i in range(n - 2, -1, -1):
            right[i] = min(right[i + 1], nums[i])
        left = 0
        for i, x in enumerate(nums):
            left = max(left, x)
            if left - right[i] <= k:
                return i
        return -1
```

#### Java

```java
class Solution {
    public int firstStableIndex(int[] nums, int k) {
        int n = nums.length;
        int[] right = new int[n];
        right[n - 1] = nums[n - 1];

        for (int i = n - 2; i >= 0; i--) {
            right[i] = Math.min(right[i + 1], nums[i]);
        }

        int left = 0;
        for (int i = 0; i < n; i++) {
            left = Math.max(left, nums[i]);
            if (left - right[i] <= k) {
                return i;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int firstStableIndex(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> right(n);
        right[n - 1] = nums[n - 1];

        for (int i = n - 2; i >= 0; --i) {
            right[i] = min(right[i + 1], nums[i]);
        }

        int left = 0;
        for (int i = 0; i < n; ++i) {
            left = max(left, nums[i]);
            if (left - right[i] <= k) {
                return i;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func firstStableIndex(nums []int, k int) int {
	n := len(nums)
	right := make([]int, n)
	right[n-1] = nums[n-1]

	for i := n - 2; i >= 0; i-- {
		right[i] = min(right[i+1], nums[i])
	}

	left := 0
	for i, x := range nums {
		left = max(left, x)
		if left-right[i] <= k {
			return i
		}
	}
	return -1
}
```

#### TypeScript

```ts
function firstStableIndex(nums: number[], k: number): number {
    const n = nums.length;
    const right = new Array<number>(n);
    right[n - 1] = nums[n - 1];

    for (let i = n - 2; i >= 0; i--) {
        right[i] = Math.min(right[i + 1], nums[i]);
    }

    let left = 0;
    for (let i = 0; i < n; i++) {
        left = Math.max(left, nums[i]);
        if (left - right[i] <= k) {
            return i;
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
