---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Array
    - Two Pointers
    - Binary Search
    - Floyd Cycle Detection
    - Pigeonhole Principle
---

<!-- problem:start -->

# [287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number)

[中文文档](/solution/0200-0299/0287.Find%20the%20Duplicate%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> chứa&nbsp;<code>n + 1</code> số nguyên, trong đó mỗi số nằm trong đoạn <code>[1, n]</code>.</p>

<p>Trong <code>nums</code> chỉ có <strong>một số bị lặp lại</strong>. Hãy trả về <em>số&nbsp;bị&nbsp;lặp&nbsp;lại đó</em>.</p>

<p>Bạn phải giải bài toán mà <strong>không</strong> sửa đổi mảng <code>nums</code> và chỉ dùng bộ nhớ phụ có kích thước hằng số.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,4,2,2]
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1,3,4,2]
<strong>Đầu ra:</strong> 3
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,3,3,3,3]
<strong>Đầu ra:</strong> 3</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>nums.length == n + 1</code></li>
	<li><code>1 &lt;= nums[i] &lt;= n</code></li>
	<li>Tất cả số nguyên trong <code>nums</code> chỉ xuất hiện <strong>một lần</strong>, ngoại trừ <strong>chính xác một số nguyên</strong> xuất hiện <strong>từ hai lần trở lên</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><b>Câu hỏi mở rộng:</b></p>

<ul>
	<li>Làm thế nào chứng minh rằng trong <code>nums</code> chắc chắn phải có ít nhất một số bị lặp?</li>
	<li>Bạn có thể giải bài toán với độ phức tạp thời gian tuyến tính không?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Ta không thể thay đổi mảng hay dùng hash table. Theo nguyên lý Dirichlet, nếu có nhiều hơn $x$ giá trị nằm trong $[1,x]$, thì số bị lặp phải nằm trong đoạn đó.
>
> Tìm kiếm nhị phân trên miền giá trị: đếm số phần tử $\le mid$; nếu số lượng đó lớn hơn $mid$, tìm ở nửa trái, nếu không thì tìm ở nửa phải.

<!-- thinking:end -->

Ta có thể nhận thấy rằng nếu số phần tử nằm trong $[1,..x]$ lớn hơn $x$, thì số bị lặp phải nằm trong $[1,..x]$; nếu không, số bị lặp phải nằm trong $[x+1,..n]$.

Vì vậy, ta có thể dùng tìm kiếm nhị phân để tìm $x$, và ở mỗi bước kiểm tra xem số phần tử trong $[1,..x]$ có lớn hơn $x$ hay không. Nhờ đó, ta xác định được số bị lặp nằm trong đoạn nào và thu hẹp phạm vi tìm kiếm cho đến khi tìm thấy số đó.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        def f(x: int) -> bool:
            return sum(v <= x for v in nums) > x

        return bisect_left(range(len(nums)), True, key=f)
```

#### Java

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int l = 0, r = nums.length - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            int cnt = 0;
            for (int v : nums) {
                if (v <= mid) {
                    ++cnt;
                }
            }
            if (cnt > mid) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            int cnt = 0;
            for (int& v : nums) {
                cnt += v <= mid;
            }
            if (cnt > mid) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func findDuplicate(nums []int) int {
	return sort.Search(len(nums), func(x int) bool {
		cnt := 0
		for _, v := range nums {
			if v <= x {
				cnt++
			}
		}
		return cnt > x
	})
}
```

#### TypeScript

```ts
function findDuplicate(nums: number[]): number {
    let l = 0;
    let r = nums.length - 1;
    while (l < r) {
        const mid = (l + r) >> 1;
        let cnt = 0;
        for (const v of nums) {
            if (v <= mid) {
                ++cnt;
            }
        }
        if (cnt > mid) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn find_duplicate(nums: Vec<i32>) -> i32 {
        let mut left = 0;
        let mut right = nums.len() - 1;

        while left < right {
            let mid = (left + right) >> 1;
            let cnt = nums.iter().filter(|x| **x <= (mid as i32)).count();
            if cnt > mid {
                right = mid;
            } else {
                left = mid + 1;
            }
        }

        left as i32
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var findDuplicate = function (nums) {
    let l = 0;
    let r = nums.length - 1;
    while (l < r) {
        const mid = (l + r) >> 1;
        let cnt = 0;
        for (const v of nums) {
            if (v <= mid) {
                ++cnt;
            }
        }
        if (cnt > mid) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
