---
comments: true
difficulty: Easy
rating: 1212
source: Weekly Contest 386 Q1
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [3046. Split the Array](https://leetcode.com/problems/split-the-array)

[中文文档](/solution/3000-3099/3046.Split%20the%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <strong>chẵn</strong>. Bạn cần chia mảng thành hai phần <code>nums1</code> và <code>nums2</code> sao cho:</p>

<ul>
	<li><code>nums1.length == nums2.length == nums.length / 2</code>.</li>
	<li><code>nums1</code> chỉ chứa các phần tử <strong>phân biệt </strong>.</li>
	<li><code>nums2</code> cũng chỉ chứa các phần tử <strong>phân biệt</strong>.</li>
</ul>

<p>Trả về <code>true</code><em> nếu có thể chia mảng, và </em><code>false</code> <em>nếu không thể</em><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,1,2,2,3,4]
<strong>Output:</strong> true
<strong>Giải thích:</strong> Một trong những cách chia nums là nums1 = [1,2,3] và nums2 = [1,2,4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,1,1,1]
<strong>Output:</strong> false
<strong>Giải thích:</strong> Cách chia duy nhất là nums1 = [1,1] và nums2 = [1,1]. Cả nums1 và nums2 đều không chứa các phần tử phân biệt. Vì vậy, ta trả về false.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>nums.length % 2 == 0 </code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta chia mảng thành hai tập hợp có cùng kích thước, trong đó các phần tử thuộc mỗi tập hợp đều phân biệt. $n \le 100$.
>
> Mỗi giá trị có thể xuất hiện nhiều nhất hai lần, mỗi tập hợp một lần; lần xuất hiện thứ ba sẽ khiến một phía bị trùng phần tử.
>
> Vì vậy, chỉ cần kiểm tra tần suất lớn nhất có nhỏ hơn $3$ hay không.

<!-- thinking:end -->

Theo đề bài, ta cần chia mảng thành hai phần sao cho các phần tử trong mỗi phần đều phân biệt. Vì vậy, ta có thể đếm số lần xuất hiện của mỗi phần tử trong mảng. Nếu một phần tử xuất hiện từ ba lần trở lên, nó không thể thỏa mãn yêu cầu của đề bài. Ngược lại, ta có thể chia mảng thành hai phần.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPossibleToSplit(self, nums: List[int]) -> bool:
        return max(Counter(nums).values()) < 3
```

#### Java

```java
class Solution {
    public boolean isPossibleToSplit(int[] nums) {
        int[] cnt = new int[101];
        for (int x : nums) {
            if (++cnt[x] >= 3) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isPossibleToSplit(vector<int>& nums) {
        int cnt[101]{};
        for (int x : nums) {
            if (++cnt[x] >= 3) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func isPossibleToSplit(nums []int) bool {
	cnt := [101]int{}
	for _, x := range nums {
		cnt[x]++
		if cnt[x] >= 3 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function isPossibleToSplit(nums: number[]): boolean {
    const cnt: number[] = Array(101).fill(0);
    for (const x of nums) {
        if (++cnt[x] >= 3) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn is_possible_to_split(nums: Vec<i32>) -> bool {
        let mut cnt = HashMap::new();
        for &x in &nums {
            *cnt.entry(x).or_insert(0) += 1;
        }
        *cnt.values().max().unwrap_or(&0) < 3
    }
}
```

#### C#

```cs
public class Solution {
    public bool IsPossibleToSplit(int[] nums) {
        int[] cnt = new int[101];
        foreach (int x in nums) {
            if (++cnt[x] >= 3) {
                return false;
            }
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
