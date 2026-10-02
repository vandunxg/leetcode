---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [540. Single Element in a Sorted Array](https://leetcode.com/problems/single-element-in-a-sorted-array)

[中文文档](/solution/0500-0599/0540.Single%20Element%20in%20a%20Sorted%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên đã sắp xếp, trong đó mỗi phần tử xuất hiện đúng hai lần, ngoại trừ một phần tử chỉ xuất hiện một lần.</p>

<p>Hãy trả về <em>phần tử duy nhất chỉ xuất hiện một lần</em>.</p>

<p>Lời giải phải có độ phức tạp thời gian <code>O(log n)</code> và độ phức tạp không gian <code>O(1)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [1,1,2,3,3,4,4,8,8]
<strong>Đầu ra:</strong> 2
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [3,3,7,7,10,11,11]
<strong>Đầu ra:</strong> 10
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mọi phần tử, ngoại trừ một phần tử đơn lẻ, đều xuất hiện thành cặp liền kề; ta cần tìm phần tử đơn lẻ đó trong $O(\log n)$.
>
> Các cặp nằm ở những chỉ số chẵn-lẻ liền nhau. Nếu `mid` vẫn khớp với `mid ⊕ 1`, phần tử đơn lẻ nằm bên phải; nếu không, nó nằm bên trái (có thể chính là `mid`). Dùng XOR giúp không cần tách riêng trường hợp chỉ số chẵn và lẻ.

<!-- thinking:end -->

Mảng $\textit{nums}$ đã được sắp xếp và ta cần tìm phần tử chỉ xuất hiện một lần trong thời gian $\textit{O}(\log n)$. Vì vậy, ta dùng tìm kiếm nhị phân để giải bài toán này.

Ta đặt biên trái của tìm kiếm nhị phân là $\textit{l} = 0$ và biên phải là $\textit{r} = n - 1$, trong đó $n$ là độ dài mảng.

Ở mỗi bước, ta lấy vị trí giữa $\textit{mid} = (l + r) / 2$. Nếu chỉ số $\textit{mid}$ chẵn, ta so sánh $\textit{nums}[\textit{mid}]$ với $\textit{nums}[\textit{mid} + 1]$. Nếu chỉ số $\textit{mid}$ lẻ, ta so sánh $\textit{nums}[\textit{mid}]$ với $\textit{nums}[\textit{mid} - 1]$. Vì vậy, ta có thể thống nhất bằng cách so sánh $\textit{nums}[\textit{mid}]$ với $\textit{nums}[\textit{mid} \oplus 1]$, trong đó $\oplus$ là phép XOR.

Nếu $\textit{nums}[\textit{mid}] \neq \textit{nums}[\textit{mid} \oplus 1]$, đáp án nằm trong $[\textit{l}, \textit{mid}]$, nên ta đặt $\textit{r} = \textit{mid}$. Nếu $\textit{nums}[\textit{mid}] = \textit{nums}[\textit{mid} \oplus 1]$, đáp án nằm trong $[\textit{mid} + 1, \textit{r}]$, nên ta đặt $\textit{l} = \textit{mid} + 1$. Ta tiếp tục tìm kiếm nhị phân cho đến khi $\textit{l} = \textit{r}$; khi đó $\textit{nums}[\textit{l}]$ là phần tử chỉ xuất hiện một lần.

Độ phức tạp thời gian là $\textit{O}(\log n)$, trong đó $n$ là độ dài mảng $\textit{nums}$. Độ phức tạp không gian là $\textit{O}(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def singleNonDuplicate(self, nums: List[int]) -> int:
        l, r = 0, len(nums) - 1
        while l < r:
            mid = (l + r) >> 1
            if nums[mid] != nums[mid ^ 1]:
                r = mid
            else:
                l = mid + 1
        return nums[l]
```

#### Java

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int l = 0, r = nums.length - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] != nums[mid ^ 1]) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return nums[l];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] != nums[mid ^ 1]) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return nums[l];
    }
};
```

#### Go

```go
func singleNonDuplicate(nums []int) int {
	l, r := 0, len(nums)-1
	for l < r {
		mid := (l + r) >> 1
		if nums[mid] != nums[mid^1] {
			r = mid
		} else {
			l = mid + 1
		}
	}
	return nums[l]
}
```

#### TypeScript

```ts
function singleNonDuplicate(nums: number[]): number {
    let [l, r] = [0, nums.length - 1];
    while (l < r) {
        const mid = (l + r) >> 1;
        if (nums[mid] !== nums[mid ^ 1]) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return nums[l];
}
```

#### Rust

```rust
impl Solution {
    pub fn single_non_duplicate(nums: Vec<i32>) -> i32 {
        let mut l = 0;
        let mut r = nums.len() - 1;
        while l < r {
            let mid = (l + r) >> 1;
            if nums[mid] != nums[mid ^ 1] {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        nums[l]
    }
}
```

#### C

```c
int singleNonDuplicate(int* nums, int numsSize) {
    int l = 0, r = numsSize - 1;
    while (l < r) {
        int mid = (l + r) >> 1;
        if (nums[mid] != nums[mid ^ 1]) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return nums[l];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
