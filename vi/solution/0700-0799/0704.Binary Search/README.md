---
comments: true
difficulty: Easy
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [704. Binary Search](https://leetcode.com/problems/binary-search)

[中文文档](/solution/0700-0799/0704.Binary%20Search/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> được sắp xếp tăng dần và số nguyên <code>target</code>. Hãy viết hàm tìm <code>target</code> trong <code>nums</code>. Nếu tìm thấy <code>target</code>, trả về chỉ số của nó; nếu không, trả về <code>-1</code>.</p>

<p>Thuật toán cần có độ phức tạp thời gian <code>O(log n)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,0,3,5,9,12], target = 9
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 9 có trong nums và chỉ số của nó là 4
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,0,3,5,9,12], target = 2
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> 2 không có trong nums nên trả về -1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>4</sup> &lt; nums[i], target &lt; 10<sup>4</sup></code></li>
	<li>Tất cả số nguyên trong <code>nums</code> đều <strong>khác nhau</strong>.</li>
	<li><code>nums</code> được sắp xếp tăng dần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Tìm $\textit{target}$ trong mảng tăng nghiêm ngặt. Với $n \le 10^4$, có thể quét tuần tự, nhưng mỗi lần so sánh bằng binary search có thể loại bỏ một nửa đoạn đang xét.
>
> Nếu $\textit{nums}[\textit{mid}] \ge \textit{target}$, đáp án không nằm bên phải nên cập nhật $r$ thành $\textit{mid}$; ngược lại, đáp án nằm sau $\textit{mid}$ nên cập nhật $l$ thành $\textit{mid}+1$.
>
> Khi $l=r$, ta đang ở giá trị đầu tiên không nhỏ hơn target, rồi chỉ cần so sánh thêm một lần. Độ phức tạp thời gian là $O(\log n)$, bộ nhớ phụ là $O(1)$.

<!-- thinking:end -->

Ta đặt biên trái $l=0$ và biên phải $r=n-1$ cho binary search.

Ở mỗi vòng lặp, ta tính vị trí giữa $\textit{mid}=(l+r)/2$, rồi so sánh $\textit{nums}[\textit{mid}]$ với $\textit{target}$.

- Nếu $\textit{nums}[\textit{mid}] \geq \textit{target}$, thì $\textit{target}$ nằm ở nửa trái, nên ta cập nhật biên phải $r$ thành $\textit{mid}$;
- Ngược lại, $\textit{target}$ nằm ở nửa phải, nên ta cập nhật biên trái $l$ thành $\textit{mid}+1$.

Vòng lặp kết thúc khi $l<r$; lúc này $\textit{nums}[l]$ là giá trị cần tìm. Nếu $\textit{nums}[l]=\textit{target}$ thì trả về $l$; nếu không thì trả về $-1$.

Độ phức tạp thời gian là $O(\log n)$, với $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1
        while l < r:
            mid = (l + r) >> 1
            if nums[mid] >= target:
                r = mid
            else:
                l = mid + 1
        return l if nums[l] == target else -1
```

#### Java

```java
class Solution {
    public int search(int[] nums, int target) {
        int l = 0, r = nums.length - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= target) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return nums[l] == target ? l : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= target) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return nums[l] == target ? l : -1;
    }
};
```

#### Go

```go
func search(nums []int, target int) int {
	l, r := 0, len(nums)-1
	for l < r {
		mid := (l + r) >> 1
		if nums[mid] >= target {
			r = mid
		} else {
			l = mid + 1
		}
	}
	if nums[l] == target {
		return l
	}
	return -1
}
```

#### TypeScript

```ts
function search(nums: number[], target: number): number {
    let [l, r] = [0, nums.length - 1];
    while (l < r) {
        const mid = (l + r) >> 1;
        if (nums[mid] >= target) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return nums[l] === target ? l : -1;
}
```

#### Rust

```rust
impl Solution {
    pub fn search(nums: Vec<i32>, target: i32) -> i32 {
        let mut l: usize = 0;
        let mut r: usize = nums.len() - 1;
        while l < r {
            let mid = (l + r) >> 1;
            if nums[mid] >= target {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        if nums[l] == target {
            l as i32
        } else {
            -1
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function (nums, target) {
    let [l, r] = [0, nums.length - 1];
    while (l < r) {
        const mid = (l + r) >> 1;
        if (nums[mid] >= target) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return nums[l] === target ? l : -1;
};
```

#### C#

```cs
public class Solution {
    public int Search(int[] nums, int target) {
        int l = 0, r = nums.Length - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= target) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return nums[l] == target ? l : -1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
