---
comments: true
difficulty: Easy
tags:
    - Greedy
    - Array
    - Math
    - Polygon
    - Sorting
    - Quick Sort
---

<!-- problem:start -->

# [976. Largest Perimeter Triangle](https://leetcode.com/problems/largest-perimeter-triangle)

[中文文档](/solution/0900-0999/0976.Largest%20Perimeter%20Triangle/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về <em>chu vi lớn nhất của tam giác có diện tích khác 0, được tạo từ ba độ dài trong mảng</em>. Nếu không thể tạo thành tam giác có diện tích khác 0, trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [2,1,2]
<strong>Output:</strong> 5
<strong>Giải thích:</strong> Có thể tạo thành tam giác với ba cạnh dài 1, 2 và 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,1,10]
<strong>Output:</strong> 0
<strong>Giải thích:</strong> 
Không thể dùng ba cạnh dài 1, 1 và 2 để tạo thành tam giác.
Không thể dùng ba cạnh dài 1, 1 và 10 để tạo thành tam giác.
Không thể dùng ba cạnh dài 1, 2 và 10 để tạo thành tam giác.
Vì không thể chọn ba cạnh nào để tạo thành tam giác có diện tích khác 0, ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sorting + Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Ba cạnh $a\le b\le c$ tạo thành tam giác khi và chỉ khi $a+b>c$, và ta cần chu vi lớn nhất. Duyệt mọi bộ ba sẽ tốn thời gian bậc ba. Sau khi sắp xếp, thử $c$ từ lớn xuống nhỏ cùng hai cạnh liền trước nó; bộ ba đầu tiên thỏa bất đẳng thức là tối ưu. Nếu không, cạnh $c$ đó không thể tạo thành tam giác hợp lệ.

<!-- thinking:end -->

Giả sử ba cạnh của tam giác là $a \leq b \leq c$. Tam giác có diện tích khác 0 khi và chỉ khi $a + b \gt c$.

Ta có thể duyệt cạnh lớn nhất $c$, rồi chọn hai cạnh lớn nhất còn lại làm $a$ và $b$. Nếu $a + b \gt c$, ta tạo được tam giác có diện tích khác 0 với chu vi lớn nhất có thể; nếu không, tiếp tục xét cạnh lớn tiếp theo làm $c$.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestPerimeter(self, nums: List[int]) -> int:
        nums.sort()
        for i in range(len(nums) - 1, 1, -1):
            if (c := nums[i - 1] + nums[i - 2]) > nums[i]:
                return c + nums[i]
        return 0
```

#### Java

```java
class Solution {
    public int largestPerimeter(int[] nums) {
        Arrays.sort(nums);
        for (int i = nums.length - 1; i >= 2; --i) {
            int c = nums[i - 1] + nums[i - 2];
            if (c > nums[i]) {
                return c + nums[i];
            }
        }
        return 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int largestPerimeter(vector<int>& nums) {
        ranges::sort(nums);
        for (int i = nums.size() - 1; i > 1; --i) {
            int c = nums[i - 1] + nums[i - 2];
            if (c > nums[i]) {
                return c + nums[i];
            }
        }
        return 0;
    }
};
```

#### Go

```go
func largestPerimeter(nums []int) int {
	sort.Ints(nums)
	for i := len(nums) - 1; i >= 2; i-- {
		if c := nums[i-1] + nums[i-2]; c > nums[i] {
			return c + nums[i]
		}
	}
	return 0
}
```

#### TypeScript

```ts
function largestPerimeter(nums: number[]): number {
    nums.sort((a, b) => a - b);
    for (let i = nums.length - 1; i > 1; --i) {
        const [a, b, c] = nums.slice(i - 2, i + 1);
        if (a + b > c) {
            return a + b + c;
        }
    }
    return 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn largest_perimeter(mut nums: Vec<i32>) -> i32 {
        let n = nums.len();
        nums.sort_unstable_by(|a, b| b.cmp(&a));
        for i in 2..n {
            let (a, b, c) = (nums[i - 2], nums[i - 1], nums[i]);
            if a < b + c {
                return a + b + c;
            }
        }
        0
    }
}
```

#### C

```c
int cmp(const void* a, const void* b) {
    return *(int*) b - *(int*) a;
}

int largestPerimeter(int* nums, int numsSize) {
    qsort(nums, numsSize, sizeof(int), cmp);
    for (int i = 2; i < numsSize; i++) {
        if (nums[i - 2] < nums[i - 1] + nums[i]) {
            return nums[i - 2] + nums[i - 1] + nums[i];
        }
    }
    return 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
