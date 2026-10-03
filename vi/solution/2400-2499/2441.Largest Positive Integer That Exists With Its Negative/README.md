---
comments: true
difficulty: Easy
rating: 1167
source: Weekly Contest 315 Q1
tags:
    - Array
    - Hash Table
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [2441. Largest Positive Integer That Exists With Its Negative](https://leetcode.com/problems/largest-positive-integer-that-exists-with-its-negative)

[中文文档](/solution/2400-2499/2441.Largest%20Positive%20Integer%20That%20Exists%20With%20Its%20Negative/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> <strong>không chứa</strong> số 0, hãy tìm <strong>số nguyên dương lớn nhất</strong> <code>k</code> sao cho <code>-k</code> cũng xuất hiện trong mảng.</p>

<p>Trả về <em>số nguyên dương </em><code>k</code>. Nếu không tồn tại số nguyên như vậy, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,2,-3,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 3 là giá trị k hợp lệ duy nhất có thể tìm thấy trong mảng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,10,6,7,-7,1]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Cả 1 và 7 đều có giá trị đối tương ứng trong mảng. 7 có giá trị lớn hơn.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-10,8,6,7,-2,-3]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có giá trị k hợp lệ nào, nên ta trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>-1000 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>nums[i] != 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 1000$, ta kiểm tra xem cả $x$ và $-x$ có cùng xuất hiện hay không. Đưa các giá trị vào một set, sau đó chọn $x$ lớn nhất sao cho giá trị đối của nó tồn tại, nếu không có thì trả về $-1$.

<!-- thinking:end -->

Ta có thể dùng một hash table $s$ để lưu tất cả phần tử xuất hiện trong mảng, đồng thời dùng biến $ans$ để lưu số nguyên dương lớn nhất thỏa mãn yêu cầu của bài toán, ban đầu $ans = -1$.

Tiếp theo, ta duyệt từng phần tử $x$ trong hash table $s$. Nếu $-x$ tồn tại trong $s$, ta cập nhật $ans = \max(ans, x)$.

Sau khi duyệt xong, trả về $ans$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaxK(self, nums: List[int]) -> int:
        s = set(nums)
        return max((x for x in s if -x in s), default=-1)
```

#### Java

```java
class Solution {
    public int findMaxK(int[] nums) {
        int ans = -1;
        Set<Integer> s = new HashSet<>();
        for (int x : nums) {
            s.add(x);
        }
        for (int x : s) {
            if (s.contains(-x)) {
                ans = Math.max(ans, x);
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
    int findMaxK(vector<int>& nums) {
        unordered_set<int> s(nums.begin(), nums.end());
        int ans = -1;
        for (int x : s) {
            if (s.count(-x)) {
                ans = max(ans, x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findMaxK(nums []int) int {
	ans := -1
	s := map[int]bool{}
	for _, x := range nums {
		s[x] = true
	}
	for x := range s {
		if s[-x] && ans < x {
			ans = x
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findMaxK(nums: number[]): number {
    let ans = -1;
    const s = new Set(nums);
    for (const x of s) {
        if (s.has(-x)) {
            ans = Math.max(ans, x);
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashSet;
impl Solution {
    pub fn find_max_k(nums: Vec<i32>) -> i32 {
        let s = nums.into_iter().collect::<HashSet<i32>>();
        let mut ans = -1;
        for &x in s.iter() {
            if s.contains(&-x) {
                ans = ans.max(x);
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã sử dụng một set. Việc duyệt mảng ban đầu và truy vấn $-n$ cũng là phép kiểm tra tương tự, chỉ khác là ta duyệt theo thứ tự của mảng thay vì thứ tự của set.

<!-- thinking:end -->

<!-- tabs:start -->

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn find_max_k(nums: Vec<i32>) -> i32 {
        let mut ans = -1;
        let mut h = HashSet::new();

        for &n in &nums {
            h.insert(n);
        }

        for &n in &nums {
            if h.contains(&-n) && n > ans {
                ans = n;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
