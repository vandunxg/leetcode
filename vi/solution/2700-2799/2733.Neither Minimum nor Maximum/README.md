---
comments: true
difficulty: Easy
rating: 1147
source: Weekly Contest 349 Q1
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [2733. Neither Minimum nor Maximum](https://leetcode.com/problems/neither-minimum-nor-maximum)

[中文文档](/solution/2700-2799/2733.Neither%20Minimum%20nor%20Maximum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> chứa các số nguyên <strong>dương</strong> <strong>phân biệt</strong>, hãy tìm và trả về <strong>bất kỳ</strong> số nào trong mảng không phải là giá trị <strong>nhỏ nhất</strong> cũng không phải giá trị <strong>lớn nhất</strong> trong mảng, hoặc <strong><code>-1</code></strong> nếu không có số nào như vậy.</p>

<p>Trả về <em>số nguyên đã chọn.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,1,4]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trong ví dụ này, giá trị nhỏ nhất là 1 và giá trị lớn nhất là 4. Vì vậy, 2 hoặc 3 đều có thể là đáp án hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Vì không có số nào trong nums vừa không phải giá trị lớn nhất vừa không phải giá trị nhỏ nhất, nên không thể chọn được số thỏa mãn điều kiện đã cho. Do đó, không có đáp án.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,3]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Vì 2 không phải giá trị lớn nhất cũng không phải giá trị nhỏ nhất trong nums, nên đây là đáp án hợp lệ duy nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
	<li>Tất cả giá trị trong <code>nums</code> đều phân biệt</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Trả về một phần tử bất kỳ không phải là giá trị nhỏ nhất cũng không phải giá trị lớn nhất, hoặc $-1$ nếu không có phần tử nào. Sắp xếp rồi lấy giá trị ở giữa cũng được, nhưng ta chỉ cần tránh hai giá trị biên.
>
> Tính $mi$ và $mx$, sau đó duyệt tìm giá trị đầu tiên nằm giữa chúng.

<!-- thinking:end -->

Trước tiên, ta tìm giá trị nhỏ nhất và lớn nhất trong mảng, lần lượt ký hiệu là $mi$ và $mx$. Sau đó, ta duyệt qua mảng, tìm số đầu tiên không bằng $mi$ và cũng không bằng $mx$, rồi trả về số đó.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findNonMinOrMax(self, nums: List[int]) -> int:
        mi, mx = min(nums), max(nums)
        return next((x for x in nums if x != mi and x != mx), -1)
```

#### Java

```java
class Solution {
    public int findNonMinOrMax(int[] nums) {
        int mi = 100, mx = 0;
        for (int x : nums) {
            mi = Math.min(mi, x);
            mx = Math.max(mx, x);
        }
        for (int x : nums) {
            if (x != mi && x != mx) {
                return x;
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
    int findNonMinOrMax(vector<int>& nums) {
        auto [mi, mx] = minmax_element(nums.begin(), nums.end());
        for (int x : nums) {
            if (x != *mi && x != *mx) {
                return x;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func findNonMinOrMax(nums []int) int {
	mi, mx := slices.Min(nums), slices.Max(nums)
	for _, x := range nums {
		if x != mi && x != mx {
			return x
		}
	}
	return -1
}
```

#### Rust

```rust
impl Solution {
    pub fn find_non_min_or_max(nums: Vec<i32>) -> i32 {
        let mut mi = 100;
        let mut mx = 0;

        for &ele in nums.iter() {
            if ele < mi {
                mi = ele;
            }
            if ele > mx {
                mx = ele;
            }
        }

        for &ele in nums.iter() {
            if ele != mi && ele != mx {
                return ele;
            }
        }

        -1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
