---
comments: true
difficulty: Easy
rating: 1207
source: Weekly Contest 286 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2215. Find the Difference of Two Arrays](https://leetcode.com/problems/find-the-difference-of-two-arrays)

[Tài liệu tiếng Trung](/solution/2200-2299/2215.Find%20the%20Difference%20of%20Two%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums1</code> và <code>nums2</code>, hãy trả về <em>một danh sách</em> <code>answer</code> <em>có kích thước</em> <code>2</code> <em>trong đó:</em></p>

<ul>
	<li><code>answer[0]</code> <em>là danh sách tất cả các số nguyên <strong>khác nhau</strong> trong</em> <code>nums1</code> <em>mà <strong>không</strong> xuất hiện trong</em> <code>nums2</code><em>.</em></li>
	<li><code>answer[1]</code> <em>là danh sách tất cả các số nguyên <strong>khác nhau</strong> trong</em> <code>nums2</code> <em>mà <strong>không</strong> xuất hiện trong</em> <code>nums1</code>.</li>
</ul>

<p><strong>Lưu ý</strong> rằng các số nguyên trong các danh sách có thể được trả về theo <strong>bất kỳ</strong> thứ tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,3], nums2 = [2,4,6]
<strong>Đầu ra:</strong> [[1,3],[4,6]]
<strong>Giải thích:
</strong>Đối với nums1, nums1[1] = 2 xuất hiện ở chỉ số 0 của nums2, trong khi nums1[0] = 1 và nums1[2] = 3 không xuất hiện trong nums2. Vì vậy, answer[0] = [1,3].
Đối với nums2, nums2[0] = 2 xuất hiện ở chỉ số 1 của nums1, trong khi nums2[1] = 4 và nums2[2] = 6 không xuất hiện trong nums1. Vì vậy, answer[1] = [4,6].</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,3,3], nums2 = [1,1,2,2]
<strong>Đầu ra:</strong> [[3],[]]
<strong>Giải thích:
</strong>Đối với nums1, nums1[2] và nums1[3] không xuất hiện trong nums2. Vì nums1[2] == nums1[3], giá trị của chúng chỉ được đưa vào một lần, nên answer[0] = [3].
Mọi số nguyên trong nums2 đều xuất hiện trong nums1. Vì vậy, answer[1] = [].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 1000</code></li>
	<li><code>-1000 &lt;= nums1[i], nums2[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm các giá trị khác nhau chỉ xuất hiện trong $nums1$ và các giá trị chỉ xuất hiện trong $nums2$. Với mỗi giá trị, nếu duyệt tuyến tính mảng còn lại thì vẫn đáp ứng được khi $n \le 10^3$, nhưng các giá trị trùng lặp sẽ bị kiểm tra nhiều lần.
>
> Chuyển cả hai mảng thành set rồi lấy hiệu: $s_1 \setminus s_2$ và $s_2 \setminus s_1$. Việc kiểm tra một phần tử có thuộc hash set hay không có độ phức tạp kỳ vọng là hằng số.

<!-- thinking:end -->

Ta định nghĩa hai hash table $s1$ và $s2$ để lưu các phần tử lần lượt trong hai mảng $nums1$ và $nums2$. Sau đó, ta duyệt từng phần tử trong $s1$. Nếu phần tử này không có trong $s2$, ta thêm nó vào danh sách đầu tiên của đáp án. Tương tự, ta duyệt từng phần tử trong $s2$. Nếu phần tử này không có trong $s1$, ta thêm nó vào danh sách thứ hai của đáp án.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findDifference(self, nums1: List[int], nums2: List[int]) -> List[List[int]]:
        s1, s2 = set(nums1), set(nums2)
        return [list(s1 - s2), list(s2 - s1)]
```

#### Java

```java
class Solution {
    public List<List<Integer>> findDifference(int[] nums1, int[] nums2) {
        Set<Integer> s1 = convert(nums1);
        Set<Integer> s2 = convert(nums2);
        List<Integer> l1 = new ArrayList<>();
        List<Integer> l2 = new ArrayList<>();
        for (int v : s1) {
            if (!s2.contains(v)) {
                l1.add(v);
            }
        }
        for (int v : s2) {
            if (!s1.contains(v)) {
                l2.add(v);
            }
        }
        return List.of(l1, l2);
    }

    private Set<Integer> convert(int[] nums) {
        Set<Integer> s = new HashSet<>();
        for (int v : nums) {
            s.add(v);
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> findDifference(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> s1(nums1.begin(), nums1.end());
        unordered_set<int> s2(nums2.begin(), nums2.end());
        vector<vector<int>> ans(2);
        for (int v : s1) {
            if (!s2.contains(v)) {
                ans[0].push_back(v);
            }
        }
        for (int v : s2) {
            if (!s1.contains(v)) {
                ans[1].push_back(v);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findDifference(nums1 []int, nums2 []int) [][]int {
	s1, s2 := make(map[int]bool), make(map[int]bool)
	for _, v := range nums1 {
		s1[v] = true
	}
	for _, v := range nums2 {
		s2[v] = true
	}
	ans := make([][]int, 2)
	for v := range s1 {
		if !s2[v] {
			ans[0] = append(ans[0], v)
		}
	}
	for v := range s2 {
		if !s1[v] {
			ans[1] = append(ans[1], v)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findDifference(nums1: number[], nums2: number[]): number[][] {
    const s1: Set<number> = new Set(nums1);
    const s2: Set<number> = new Set(nums2);
    nums1.forEach(num => s2.delete(num));
    nums2.forEach(num => s1.delete(num));
    return [Array.from(s1), Array.from(s2)];
}
```

#### Rust

```rust
use std::collections::HashSet;
impl Solution {
    pub fn find_difference(nums1: Vec<i32>, nums2: Vec<i32>) -> Vec<Vec<i32>> {
        vec![
            nums1
                .iter()
                .filter_map(|&v| if nums2.contains(&v) { None } else { Some(v) })
                .collect::<HashSet<i32>>()
                .into_iter()
                .collect(),
            nums2
                .iter()
                .filter_map(|&v| if nums1.contains(&v) { None } else { Some(v) })
                .collect::<HashSet<i32>>()
                .into_iter()
                .collect(),
        ]
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[][]}
 */
var findDifference = function (nums1, nums2) {
    const s1 = new Set(nums1);
    const s2 = new Set(nums2);
    nums1.forEach(num => s2.delete(num));
    nums2.forEach(num => s1.delete(num));
    return [Array.from(s1), Array.from(s2)];
};
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums1
     * @param Integer[] $nums2
     * @return Integer[][]
     */
    function findDifference($nums1, $nums2) {
        $s1 = array_flip($nums1);
        $s2 = array_flip($nums2);

        $diff1 = array_diff_key($s1, $s2);
        $diff2 = array_diff_key($s2, $s1);

        return [array_keys($diff1), array_keys($diff2)];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
