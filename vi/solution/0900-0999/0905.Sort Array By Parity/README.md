---
comments: true
difficulty: Easy
tags:
    - Array
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [905. Sort Array By Parity](https://leetcode.com/problems/sort-array-by-parity)

[中文文档](/solution/0900-0999/0905.Sort%20Array%20By%20Parity/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy đưa tất cả số chẵn lên đầu mảng, tiếp theo là các số lẻ.</p>

<p>Hãy trả về <em><strong>một mảng bất kỳ</strong> thỏa mãn điều kiện này</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [3,1,2,4]
<strong>Output:</strong> [2,4,3,1]
<strong>Giải thích:</strong> Các kết quả [4,2,3,1], [2,4,1,3] và [4,2,1,3] cũng được chấp nhận.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [0]
<strong>Output:</strong> [0]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 5000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Số chẵn cần đứng trước số lẻ, còn thứ tự tương đối không bị ràng buộc. Có thể dùng mảng phụ, nhưng ta cũng có thể hoán vị trực tiếp trên mảng. Hai con trỏ di chuyển vào giữa: tăng con trỏ trái khi gặp số chẵn, giảm con trỏ phải khi gặp số lẻ; nếu không thì hoán đổi hai phần tử. Mỗi chỉ số chỉ được xét một số lần hằng số.

<!-- thinking:end -->

Ta dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến đầu và cuối mảng. Khi $i < j$, ta thực hiện các bước sau:

- Nếu $nums[i]$ là số chẵn, tăng $i$ thêm $1$.
- Nếu $nums[j]$ là số lẻ, giảm $j$ đi $1$.
- Nếu $nums[i]$ là số lẻ và $nums[j]$ là số chẵn, hoán đổi $nums[i]$ và $nums[j]$. Sau đó tăng $i$ thêm $1$ và giảm $j$ đi $1$.

Cuối cùng, trả về mảng $nums$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortArrayByParity(self, nums: List[int]) -> List[int]:
        i, j = 0, len(nums) - 1
        while i < j:
            if nums[i] % 2 == 0:
                i += 1
            elif nums[j] % 2 == 1:
                j -= 1
            else:
                nums[i], nums[j] = nums[j], nums[i]
                i, j = i + 1, j - 1
        return nums
```

#### Java

```java
class Solution {
    public int[] sortArrayByParity(int[] nums) {
        int i = 0, j = nums.length - 1;
        while (i < j) {
            if (nums[i] % 2 == 0) {
                ++i;
            } else if (nums[j] % 2 == 1) {
                --j;
            } else {
                int t = nums[i];
                nums[i] = nums[j];
                nums[j] = t;
                ++i;
                --j;
            }
        }
        return nums;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& nums) {
        int i = 0, j = nums.size() - 1;
        while (i < j) {
            if (nums[i] % 2 == 0) {
                ++i;
            } else if (nums[j] % 2 == 1) {
                --j;
            } else {
                swap(nums[i++], nums[j--]);
            }
        }
        return nums;
    }
};
```

#### Go

```go
func sortArrayByParity(nums []int) []int {
	for i, j := 0, len(nums)-1; i < j; {
		if nums[i]%2 == 0 {
			i++
		} else if nums[j]%2 == 1 {
			j--
		} else {
			nums[i], nums[j] = nums[j], nums[i]
		}
	}
	return nums
}
```

#### TypeScript

```ts
function sortArrayByParity(nums: number[]): number[] {
    for (let i = 0, j = nums.length - 1; i < j;) {
        if (nums[i] % 2 === 0) {
            ++i;
        } else if (nums[j] % 2 === 1) {
            --j;
        } else {
            [nums[i], nums[j]] = [nums[j], nums[i]];
            ++i;
            --j;
        }
    }
    return nums;
}
```

#### Rust

```rust
impl Solution {
    pub fn sort_array_by_parity(mut nums: Vec<i32>) -> Vec<i32> {
        let (mut i, mut j) = (0, nums.len() - 1);
        while i < j {
            if nums[i] % 2 == 0 {
                i += 1;
            } else if nums[j] % 2 == 1 {
                j -= 1;
            } else {
                nums.swap(i, j);
                i += 1;
                j -= 1;
            }
        }
        nums
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortArrayByParity = function (nums) {
    for (let i = 0, j = nums.length - 1; i < j;) {
        if (nums[i] % 2 === 0) {
            ++i;
        } else if (nums[j] % 2 === 1) {
            --j;
        } else {
            [nums[i], nums[j]] = [nums[j], nums[i]];
            ++i;
            --j;
        }
    }
    return nums;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
