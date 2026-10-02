---
comments: true
difficulty: Easy
tags:
    - Array
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [922. Sort Array By Parity II](https://leetcode.com/problems/sort-array-by-parity-ii)

[中文文档](/solution/0900-0999/0922.Sort%20Array%20By%20Parity%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, trong đó một nửa số nguyên là <strong>số lẻ</strong>, nửa còn lại là <strong>số chẵn</strong>.</p>

<p>Hãy sắp xếp mảng sao cho nếu <code>nums[i]</code> là số lẻ thì <code>i</code> là <strong>số lẻ</strong>, còn nếu <code>nums[i]</code> là số chẵn thì <code>i</code> là <strong>số chẵn</strong>.</p>

<p>Hãy trả về <em>bất kỳ mảng nào thỏa mãn điều kiện này</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,2,5,7]
<strong>Đầu ra:</strong> [4,5,2,7]
<strong>Giải thích:</strong> [4,7,2,5], [2,5,4,7], [2,7,4,5] cũng là các đáp án được chấp nhận.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3]
<strong>Đầu ra:</strong> [2,3]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>nums.length</code> là số chẵn.</li>
	<li>Một nửa số nguyên trong <code>nums</code> là số chẵn.</li>
	<li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài này in-place không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai pointer

<!-- thinking:start -->

> **Tư duy**
>
> Các chỉ số chẵn phải chứa số chẵn, còn chỉ số lẻ phải chứa số lẻ; số lượng hai loại bằng nhau. Có thể dùng mảng phụ, nhưng ta cũng có thể hoán đổi trực tiếp tại chỗ. $i$ duyệt các chỉ số chẵn; nếu phần tử ở đó là số lẻ, $j$ duyệt các chỉ số lẻ cho đến khi tìm được số chẵn rồi hoán đổi hai phần tử. $j$ chỉ tăng nên lượt duyệt có độ phức tạp tuyến tính.

<!-- thinking:end -->

Ta dùng hai pointer $i$ và $j$ lần lượt trỏ đến các chỉ số chẵn và lẻ. Ban đầu, $i = 0$ và $j = 1$.

Khi $i$ trỏ đến một chỉ số chẵn, nếu $\textit{nums}[i]$ là số lẻ, ta tìm một chỉ số lẻ $j$ sao cho $\textit{nums}[j]$ là số chẵn, rồi hoán đổi $\textit{nums}[i]$ với $\textit{nums}[j]$. Tiếp tục duyệt cho đến khi $i$ đến cuối mảng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortArrayByParityII(self, nums: List[int]) -> List[int]:
        n, j = len(nums), 1
        for i in range(0, n, 2):
            if nums[i] % 2:
                while nums[j] % 2:
                    j += 2
                nums[i], nums[j] = nums[j], nums[i]
        return nums
```

#### Java

```java
class Solution {
    public int[] sortArrayByParityII(int[] nums) {
        for (int i = 0, j = 1; i < nums.length; i += 2) {
            if (nums[i] % 2 == 1) {
                while (nums[j] % 2 == 1) {
                    j += 2;
                }
                int t = nums[i];
                nums[i] = nums[j];
                nums[j] = t;
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
    vector<int> sortArrayByParityII(vector<int>& nums) {
        for (int i = 0, j = 1; i < nums.size(); i += 2) {
            if (nums[i] % 2) {
                while (nums[j] % 2) {
                    j += 2;
                }
                swap(nums[i], nums[j]);
            }
        }
        return nums;
    }
};
```

#### Go

```go
func sortArrayByParityII(nums []int) []int {
	for i, j := 0, 1; i < len(nums); i += 2 {
		if nums[i]%2 == 1 {
			for nums[j]%2 == 1 {
				j += 2
			}
			nums[i], nums[j] = nums[j], nums[i]
		}
	}
	return nums
}
```

#### TypeScript

```ts
function sortArrayByParityII(nums: number[]): number[] {
    for (let i = 0, j = 1; i < nums.length; i += 2) {
        if (nums[i] % 2) {
            while (nums[j] % 2) {
                j += 2;
            }
            [nums[i], nums[j]] = [nums[j], nums[i]];
        }
    }
    return nums;
}
```

#### Rust

```rust
impl Solution {
    pub fn sort_array_by_parity_ii(mut nums: Vec<i32>) -> Vec<i32> {
        let n = nums.len();
        let mut j = 1;
        for i in (0..n).step_by(2) {
            if nums[i] % 2 != 0 {
                while nums[j] % 2 != 0 {
                    j += 2;
                }
                nums.swap(i, j);
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
var sortArrayByParityII = function (nums) {
    for (let i = 0, j = 1; i < nums.length; i += 2) {
        if (nums[i] % 2) {
            while (nums[j] % 2) {
                j += 2;
            }
            [nums[i], nums[j]] = [nums[j], nums[i]];
        }
    }
    return nums;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
