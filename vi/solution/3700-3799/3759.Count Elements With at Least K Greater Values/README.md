---
comments: true
difficulty: Medium
rating: 1372
source: Weekly Contest 478 Q1
tags:
    - Array
    - Binary Search
    - Divide and Conquer
    - Quickselect
    - Sorting
---

<!-- problem:start -->

# [3759. Count Elements With at Least K Greater Values](https://leetcode.com/problems/count-elements-with-at-least-k-greater-values)

[中文文档](/solution/3700-3799/3759.Count%20Elements%20With%20at%20Least%20K%20Greater%20Values/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>k</code>.</p>

<p>Một phần tử trong <code>nums</code> được gọi là <strong>đạt yêu cầu</strong> nếu trong mảng tồn tại <strong>ít nhất</strong> <code>k</code> phần tử <strong>lớn hơn nghiêm ngặt</strong> nó.</p>

<p>Trả về một số nguyên biểu thị tổng số phần tử đạt yêu cầu trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,2], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mỗi phần tử 1 và 2 đều có ít nhất <code>k = 1</code> phần tử lớn hơn nó.<br />
​​​​​​​Không có phần tử nào lớn hơn 3. Do đó, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,5,5], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì tất cả phần tử đều bằng 5 nên không có phần tử nào lớn hơn phần tử khác. Do đó, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= k &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Một phần tử đạt yêu cầu khi và chỉ khi có ít nhất $k$ giá trị lớn hơn nghiêm ngặt nó. Nếu $k=0$, mọi phần tử đều đạt yêu cầu; ngược lại, sau khi sắp xếp, giá trị tại chỉ số $n-k$ là ngưỡng, và chỉ các phần tử nhỏ hơn nghiêm ngặt nằm bên trái nó mới được tính.

<!-- thinking:end -->

Nếu $k = 0$, tất cả phần tử trong mảng đều là phần tử đạt yêu cầu, vì vậy ta có thể trực tiếp trả về độ dài của mảng.

Nếu không, ta sắp xếp mảng và gọi $n$ là độ dài của mảng đã sắp xếp. Với mỗi chỉ số $i$ thỏa mãn $0 \leq i < n - k$, nếu phần tử tại chỉ số $i$ nhỏ hơn nghiêm ngặt phần tử tại chỉ số $n - k$ thì đó là một phần tử đạt yêu cầu. Ta chỉ cần đếm số phần tử như vậy và trả về kết quả.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countElements(self, nums: List[int], k: int) -> int:
        n = len(nums)
        if k == 0:
            return n
        nums.sort()
        return sum(nums[n - k] > nums[i] for i in range(n - k))
```

#### Java

```java
class Solution {
    public int countElements(int[] nums, int k) {
        int n = nums.length;
        if (k == 0) {
            return n;
        }
        Arrays.sort(nums);
        int ans = 0;
        for (int i = 0; i < n - k; ++i) {
            if (nums[n - k] > nums[i]) {
                ++ans;
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
    int countElements(vector<int>& nums, int k) {
        int n = nums.size();
        if (k == 0) {
            return n;
        }
        ranges::sort(nums);
        int ans = 0;
        for (int i = 0; i < n - k; ++i) {
            if (nums[n - k] > nums[i]) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countElements(nums []int, k int) int {
	n := len(nums)
	if k == 0 {
		return n
	}
	sort.Ints(nums)
	ans := 0
	for i := 0; i < n-k; i++ {
		if nums[n-k] > nums[i] {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countElements(nums: number[], k: number): number {
    const n = nums.length;
    if (k === 0) {
        return n;
    }
    nums.sort((a, b) => a - b);
    let ans = 0;
    for (let i = 0; i < n - k; ++i) {
        if (nums[n - k] > nums[i]) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
