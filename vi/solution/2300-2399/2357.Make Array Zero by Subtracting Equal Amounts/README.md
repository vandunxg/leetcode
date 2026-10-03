---
comments: true
difficulty: Easy
rating: 1225
source: Weekly Contest 304 Q1
tags:
    - Greedy
    - Array
    - Hash Table
    - Sorting
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2357. Make Array Zero by Subtracting Equal Amounts](https://leetcode.com/problems/make-array-zero-by-subtracting-equal-amounts)

[中文文档](/solution/2300-2399/2357.Make%20Array%20Zero%20by%20Subtracting%20Equal%20Amounts/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên không âm <code>nums</code>. Trong một phép toán, bạn phải:</p>

<ul>
	<li>Chọn một số nguyên dương <code>x</code> sao cho <code>x</code> nhỏ hơn hoặc bằng phần tử <strong>nhỏ nhất khác 0</strong> trong <code>nums</code>.</li>
	<li>Trừ <code>x</code> khỏi mọi phần tử <strong>dương</strong> trong <code>nums</code>.</li>
</ul>

<p>Trả về <em>số phép toán <strong>nhỏ nhất</strong> cần thực hiện để mọi phần tử trong </em><code>nums</code><em> bằng </em><code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5,0,3,5]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Trong phép toán đầu tiên, chọn x = 1. Khi đó, nums = [0,4,0,2,4].
Trong phép toán thứ hai, chọn x = 2. Khi đó, nums = [0,2,0,0,2].
Trong phép toán thứ ba, chọn x = 2. Khi đó, nums = [0,0,0,0,0].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mỗi phần tử trong nums đã bằng 0 nên không cần thực hiện phép toán nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table hoặc mảng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi phép toán chọn một $x$ dương và trừ nó khỏi mọi phần tử $\ge x$, qua đó loại bỏ một giá trị dương phân biệt. Vì $n \le 100$, chỉ cần đếm các giá trị đó.
>
> Các số 0 không bao giờ thay đổi và không tạo ra số dương mới. Đáp án là kích thước của $\{x \in nums: x>0\}$.

<!-- thinking:end -->

Ta nhận thấy rằng trong mỗi phép toán, tất cả các phần tử khác 0 giống nhau trong mảng $\textit{nums}$ có thể được giảm về $0$. Do đó, ta chỉ cần đếm số phần tử khác 0 phân biệt trong $\textit{nums}$, đây chính là số phép toán nhỏ nhất cần thực hiện. Để đếm các phần tử khác 0 phân biệt, ta có thể sử dụng hash table hoặc mảng.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperations(self, nums: List[int]) -> int:
        return len({x for x in nums if x})
```

#### Java

```java
class Solution {
    public int minimumOperations(int[] nums) {
        boolean[] s = new boolean[101];
        s[0] = true;
        int ans = 0;
        for (int x : nums) {
            if (!s[x]) {
                ++ans;
                s[x] = true;
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
    int minimumOperations(vector<int>& nums) {
        bool s[101]{};
        s[0] = true;
        int ans = 0;
        for (int& x : nums) {
            if (!s[x]) {
                ++ans;
                s[x] = true;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumOperations(nums []int) (ans int) {
	s := [101]bool{true}
	for _, x := range nums {
		if !s[x] {
			s[x] = true
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function minimumOperations(nums: number[]): number {
    const s = new Set(nums);
    s.delete(0);
    return s.size;
}
```

#### Rust

```rust
use std::collections::HashSet;
impl Solution {
    pub fn minimum_operations(nums: Vec<i32>) -> i32 {
        let mut s = nums.iter().collect::<HashSet<&i32>>();
        s.remove(&0);
        s.len() as i32
    }
}
```

#### C

```c
int minimumOperations(int* nums, int numsSize) {
    int vis[101] = {0};
    vis[0] = 1;
    int ans = 0;
    for (int i = 0; i < numsSize; i++) {
        if (vis[nums[i]]) {
            continue;
        }
        vis[nums[i]] = 1;
        ans++;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
