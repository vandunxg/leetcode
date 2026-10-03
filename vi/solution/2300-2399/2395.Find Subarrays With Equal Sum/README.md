---
comments: true
difficulty: Easy
rating: 1249
source: Biweekly Contest 86 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2395. Find Subarrays With Equal Sum](https://leetcode.com/problems/find-subarrays-with-equal-sum)

[中文文档](/solution/2300-2399/2395.Find%20Subarrays%20With%20Equal%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>, hãy xác định xem có tồn tại <strong>hai</strong> mảng con có độ dài <code>2</code> và có <strong>tổng</strong> bằng nhau hay không. Lưu ý rằng hai mảng con phải bắt đầu tại các <strong>chỉ số khác nhau</strong>.</p>

<p>Trả về <code>true</code><em> nếu tồn tại hai mảng con như vậy, và </em><code>false</code><em> nếu không.</em></p>

<p><b>Mảng con</b> là một dãy phần tử liên tiếp, không rỗng trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,2,4]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Hai mảng con có các phần tử [4,2] và [2,4] có cùng tổng là 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không có hai mảng con độ dài 2 nào có cùng tổng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,0,0]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Hai mảng con [nums[0],nums[1]] và [nums[1],nums[2]] có cùng tổng là 0.
Lưu ý rằng mặc dù hai mảng con có cùng nội dung, chúng vẫn được xem là khác nhau vì nằm ở các vị trí khác nhau trong mảng ban đầu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Bài toán yêu cầu tìm hai mảng con độ dài $2$ khác nhau nhưng có cùng tổng. Vì $n \le 1000$, ta có thể lưu tổng của các cặp phần tử liền kề.
>
> Duyệt qua các tổng của từng cặp: nếu gặp một tổng đã xuất hiện thì ta tìm được đáp án; nếu chưa, thêm tổng đó vào tập hợp. Các mảng con dài hơn không liên quan.

<!-- thinking:end -->

Ta có thể duyệt mảng $nums$ và dùng một bảng băm $vis$ để ghi nhận tổng của mọi cặp phần tử liền kề trong mảng. Nếu tổng của hai phần tử hiện tại đã xuất hiện trong bảng băm, ta trả về `true`. Ngược lại, thêm tổng của hai phần tử hiện tại vào bảng băm.

Nếu duyệt hết mảng mà không tìm thấy hai mảng con thỏa mãn điều kiện, ta trả về `false`.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findSubarrays(self, nums: List[int]) -> bool:
        vis = set()
        for a, b in pairwise(nums):
            if (x := a + b) in vis:
                return True
            vis.add(x)
        return False
```

#### Java

```java
class Solution {
    public boolean findSubarrays(int[] nums) {
        Set<Integer> vis = new HashSet<>();
        for (int i = 1; i < nums.length; ++i) {
            if (!vis.add(nums[i - 1] + nums[i])) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool findSubarrays(vector<int>& nums) {
        unordered_set<int> vis;
        for (int i = 1; i < nums.size(); ++i) {
            int x = nums[i - 1] + nums[i];
            if (vis.count(x)) {
                return true;
            }
            vis.insert(x);
        }
        return false;
    }
};
```

#### Go

```go
func findSubarrays(nums []int) bool {
	vis := map[int]bool{}
	for i, b := range nums[1:] {
		x := nums[i] + b
		if vis[x] {
			return true
		}
		vis[x] = true
	}
	return false
}
```

#### TypeScript

```ts
function findSubarrays(nums: number[]): boolean {
    const vis: Set<number> = new Set<number>();
    for (let i = 1; i < nums.length; ++i) {
        const x = nums[i - 1] + nums[i];
        if (vis.has(x)) {
            return true;
        }
        vis.add(x);
    }
    return false;
}
```

#### Rust

```rust
use std::collections::HashSet;
impl Solution {
    pub fn find_subarrays(nums: Vec<i32>) -> bool {
        let n = nums.len();
        let mut set = HashSet::new();
        for i in 1..n {
            if !set.insert(nums[i - 1] + nums[i]) {
                return true;
            }
        }
        false
    }
}
```

#### C

```c
bool findSubarrays(int* nums, int numsSize) {
    for (int i = 1; i < numsSize - 1; i++) {
        for (int j = i + 1; j < numsSize; j++) {
            if (nums[i - 1] + nums[i] == nums[j - 1] + nums[j]) {
                return true;
            }
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
