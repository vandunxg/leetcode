---
comments: true
difficulty: Easy
tags:
    - Array
    - Math
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [3667. Sort Array By Absolute Value 🔒](https://leetcode.com/problems/sort-array-by-absolute-value)

[中文文档](/solution/3600-3699/3667.Sort%20Array%20By%20Absolute%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Sắp xếp lại các phần tử của <code>nums</code> theo thứ tự <strong>không giảm</strong> của giá trị tuyệt đối.</p>

<p>Trả về <strong>bất kỳ</strong> mảng đã sắp xếp lại nào thỏa mãn điều kiện này.</p>

<p><strong>Lưu ý</strong>: Giá trị tuyệt đối của một số nguyên x được định nghĩa như sau:</p>

<ul>
	<li><code>x</code> nếu <code>x &gt;= 0</code></li>
	<li><code>-x</code> nếu <code>x &lt; 0</code></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,-1,-4,1,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1,1,3,-4,5]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Giá trị tuyệt đối của các phần tử trong <code>nums</code> lần lượt là 3, 1, 4, 1, 5.</li>
	<li>Sắp xếp chúng theo thứ tự tăng dần, ta được 1, 1, 3, 4, 5.</li>
	<li>Điều này tương ứng với <code>[-1, 1, 3, -4, 5]</code>. Một cách sắp xếp lại khác có thể là <code>[1, -1, 3, -4, 5].</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-100,100]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-100,100]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Giá trị tuyệt đối của các phần tử trong <code>nums</code> lần lượt là 100, 100.</li>
	<li>Sắp xếp chúng theo thứ tự tăng dần, ta được 100, 100.</li>
	<li>Điều này tương ứng với <code>[-100, 100]</code>. Một cách sắp xếp lại khác có thể là <code>[100, -100]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>-100 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp tùy chỉnh

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp theo giá trị tuyệt đối. Không cần khóa phụ. Với $n\le 100$, đây là một lần sắp xếp tùy chỉnh đơn giản.
>
> Khóa sắp xếp là $\lvert x\rvert$. Stable sort giữ nguyên thứ tự ban đầu của các phần tử có cùng giá trị tuyệt đối, và thứ tự đó vẫn đúng.

<!-- thinking:end -->

Ta có thể sử dụng một hàm sắp xếp tùy chỉnh để sắp xếp mảng, trong đó tiêu chí sắp xếp là giá trị tuyệt đối của mỗi phần tử.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortByAbsoluteValue(self, nums: List[int]) -> List[int]:
        return sorted(nums, key=lambda x: abs(x))
```

#### Java

```java
class Solution {
    public int[] sortByAbsoluteValue(int[] nums) {
        return Arrays.stream(nums)
            .boxed()
            .sorted(Comparator.comparingInt(Math::abs))
            .mapToInt(Integer::intValue)
            .toArray();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> sortByAbsoluteValue(vector<int>& nums) {
        sort(nums.begin(), nums.end(), [](int a, int b) {
            return abs(a) < abs(b);
        });
        return nums;
    }
};
```

#### Go

```go
func sortByAbsoluteValue(nums []int) []int {
	slices.SortFunc(nums, func(a, b int) int {
		return abs(a) - abs(b)
	})
	return nums
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
function sortByAbsoluteValue(nums: number[]): number[] {
    return nums.sort((a, b) => Math.abs(a) - Math.abs(b));
}
```

#### Rust

```rust
impl Solution {
    pub fn sort_by_absolute_value(mut nums: Vec<i32>) -> Vec<i32> {
        nums.sort_by_key(|&x| x.abs());
        nums
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
