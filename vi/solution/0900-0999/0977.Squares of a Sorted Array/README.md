---
comments: true
difficulty: Easy
tags:
    - Array
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [977. Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array)

[中文文档](/solution/0900-0999/0977.Squares%20of%20a%20Sorted%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> được sắp xếp theo thứ tự <strong>không giảm</strong>. Hãy trả về <em>mảng gồm <strong>bình phương của từng số</strong>, được sắp xếp theo thứ tự không giảm</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-4,-1,0,3,10]
<strong>Đầu ra:</strong> [0,1,9,16,100]
<strong>Giải thích:</strong> Sau khi bình phương, mảng trở thành [16,1,0,9,100].
Sau khi sắp xếp, mảng trở thành [0,1,9,16,100].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-7,-3,2,3,11]
<strong>Đầu ra:</strong> [4,9,9,49,121]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code><span>1 &lt;= nums.length &lt;= </span>10<sup>4</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>nums</code> được sắp xếp theo thứ tự <strong>không giảm</strong>.</li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bình phương từng phần tử rồi sắp xếp mảng mới là cách rất đơn giản. Bạn có thể tìm lời giải <code>O(n)</code> bằng một cách tiếp cận khác không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Mảng đã sắp xếp có thể chứa số âm nên cần sắp xếp lại các bình phương. Bình phương rồi sắp xếp mất $O(n\log n)$. Sau khi bình phương, giá trị ở hai đầu lớn còn ở giữa nhỏ; dùng hai con trỏ để chọn bình phương lớn hơn và ghi từ cuối mảng (hoặc thêm vào rồi đảo ngược), ta có thể xử lý trong thời gian tuyến tính.

<!-- thinking:end -->

Vì mảng $nums$ đã được sắp xếp theo thứ tự không giảm, bình phương của các số âm trong mảng giảm dần, còn bình phương của các số dương tăng dần. Ta có thể dùng hai con trỏ, mỗi con trỏ trỏ đến một đầu mảng. Mỗi lần so sánh bình phương của hai phần tử được trỏ tới, ta ghi bình phương lớn hơn vào cuối mảng kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $nums$. Không tính không gian của mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        ans = []
        i, j = 0, len(nums) - 1
        while i <= j:
            a = nums[i] * nums[i]
            b = nums[j] * nums[j]
            if a > b:
                ans.append(a)
                i += 1
            else:
                ans.append(b)
                j -= 1
        return ans[::-1]
```

#### Java

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        for (int i = 0, j = n - 1, k = n - 1; i <= j; --k) {
            int a = nums[i] * nums[i];
            int b = nums[j] * nums[j];
            if (a > b) {
                ans[k] = a;
                ++i;
            } else {
                ans[k] = b;
                --j;
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
    vector<int> sortedSquares(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n);
        for (int i = 0, j = n - 1, k = n - 1; i <= j; --k) {
            int a = nums[i] * nums[i];
            int b = nums[j] * nums[j];
            if (a > b) {
                ans[k] = a;
                ++i;
            } else {
                ans[k] = b;
                --j;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func sortedSquares(nums []int) []int {
	n := len(nums)
	ans := make([]int, n)
	for i, j, k := 0, n-1, n-1; i <= j; k-- {
		a := nums[i] * nums[i]
		b := nums[j] * nums[j]
		if a > b {
			ans[k] = a
			i++
		} else {
			ans[k] = b
			j--
		}
	}
	return ans
}
```

#### Rust

```rust
impl Solution {
    pub fn sorted_squares(nums: Vec<i32>) -> Vec<i32> {
        let n = nums.len();
        let mut ans = vec![0; n];
        let (mut i, mut j) = (0, n - 1);
        for k in (0..n).rev() {
            let a = nums[i] * nums[i];
            let b = nums[j] * nums[j];
            if a > b {
                ans[k] = a;
                i += 1;
            } else {
                ans[k] = b;
                j -= 1;
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortedSquares = function (nums) {
    const n = nums.length;
    const ans = Array(n).fill(0);
    for (let i = 0, j = n - 1, k = n - 1; i <= j; --k) {
        const [a, b] = [nums[i] * nums[i], nums[j] * nums[j]];
        if (a > b) {
            ans[k] = a;
            ++i;
        } else {
            ans[k] = b;
            --j;
        }
    }
    return ans;
};
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums
     * @return Integer[]
     */
    function sortedSquares($nums) {
        $n = count($nums);
        $ans = array_fill(0, $n, 0);
        for ($i = 0, $j = $n - 1, $k = $n - 1; $i <= $j; --$k) {
            $a = $nums[$i] * $nums[$i];
            $b = $nums[$j] * $nums[$j];
            if ($a > $b) {
                $ans[$k] = $a;
                ++$i;
            } else {
                $ans[$k] = $b;
                --$j;
            }
        }
        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
