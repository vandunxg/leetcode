---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [350. Intersection of Two Arrays II](https://leetcode.com/problems/intersection-of-two-arrays-ii)

[中文文档](/solution/0300-0399/0350.Intersection%20of%20Two%20Arrays%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code>, hãy trả về <em>mảng giao của chúng</em>. Mỗi phần tử trong kết quả xuất hiện số lần bằng tần suất nhỏ hơn của nó trong hai mảng; có thể trả kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,2,1], nums2 = [2,2]
<strong>Đầu ra:</strong> [2,2]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [4,9,5], nums2 = [9,4,9,8,4]
<strong>Đầu ra:</strong> [4,9]
<strong>Giải thích:</strong> [9,4] cũng được chấp nhận.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums1[i], nums2[i] &lt;= 1000</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>Nếu mảng đã cho được sắp xếp sẵn thì sao? Bạn sẽ tối ưu thuật toán như thế nào?</li>
	<li>Nếu <code>nums1</code> nhỏ hơn nhiều so với <code>nums2</code> thì sao? Thuật toán nào phù hợp hơn?</li>
	<li>Nếu các phần tử của <code>nums2</code> được lưu trên đĩa và bộ nhớ không đủ để nạp toàn bộ phần tử cùng lúc thì sao?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table

<!-- thinking:start -->

> **Tư duy**
>
> Kết quả giao phải giữ số lần xuất hiện, tức là lấy giá trị nhỏ hơn trong hai tần suất. Khác với bài 349, ta không thể loại bỏ các giá trị trùng nhau.
>
> Đếm tần suất trong $nums1$, rồi duyệt $nums2$: nếu một giá trị còn số lần xuất hiện chưa dùng thì thêm nó vào kết quả và giảm tần suất đi một. Số lần mỗi giá trị xuất hiện trong kết quả là giá trị nhỏ hơn giữa hai tần suất.

<!-- thinking:end -->

Ta dùng hash table $\textit{cnt}$ để đếm số lần xuất hiện của từng phần tử trong mảng $\textit{nums1}$. Sau đó, duyệt mảng $\textit{nums2}$. Nếu phần tử $x$ có trong $\textit{cnt}$ và tần suất của nó lớn hơn $0$, ta thêm $x$ vào đáp án rồi giảm tần suất đi một.

Duyệt xong thì trả về mảng kết quả.

Độ phức tạp thời gian là $O(m + n)$ và độ phức tạp không gian là $O(m)$, trong đó $m$ và $n$ lần lượt là độ dài của hai mảng $\textit{nums1}$ và $\textit{nums2}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        cnt = Counter(nums1)
        ans = []
        for x in nums2:
            if cnt[x]:
                ans.append(x)
                cnt[x] -= 1
        return ans
```

#### Java

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        int[] cnt = new int[1001];
        for (int x : nums1) {
            ++cnt[x];
        }
        List<Integer> ans = new ArrayList<>();
        for (int x : nums2) {
            if (cnt[x]-- > 0) {
                ans.add(x);
            }
        }
        return ans.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> cnt;
        for (int x : nums1) {
            ++cnt[x];
        }
        vector<int> ans;
        for (int x : nums2) {
            if (cnt[x]-- > 0) {
                ans.push_back(x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func intersect(nums1 []int, nums2 []int) (ans []int) {
	cnt := map[int]int{}
	for _, x := range nums1 {
		cnt[x]++
	}
	for _, x := range nums2 {
		if cnt[x] > 0 {
			ans = append(ans, x)
			cnt[x]--
		}
	}
	return
}
```

#### TypeScript

```ts
function intersect(nums1: number[], nums2: number[]): number[] {
    const cnt: Record<number, number> = {};
    for (const x of nums1) {
        cnt[x] = (cnt[x] || 0) + 1;
    }
    const ans: number[] = [];
    for (const x of nums2) {
        if (cnt[x]-- > 0) {
            ans.push(x);
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn intersect(nums1: Vec<i32>, nums2: Vec<i32>) -> Vec<i32> {
        let mut cnt = HashMap::new();
        for &x in &nums1 {
            *cnt.entry(x).or_insert(0) += 1;
        }
        let mut ans = Vec::new();
        for &x in &nums2 {
            if let Some(count) = cnt.get_mut(&x) {
                if *count > 0 {
                    ans.push(x);
                    *count -= 1;
                }
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersect = function (nums1, nums2) {
    const cnt = {};
    for (const x of nums1) {
        cnt[x] = (cnt[x] || 0) + 1;
    }
    const ans = [];
    for (const x of nums2) {
        if (cnt[x]-- > 0) {
            ans.push(x);
        }
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int[] Intersect(int[] nums1, int[] nums2) {
        Dictionary<int, int> cnt = new Dictionary<int, int>();
        foreach (int x in nums1) {
            if (cnt.ContainsKey(x)) {
                cnt[x]++;
            } else {
                cnt[x] = 1;
            }
        }
        List<int> ans = new List<int>();
        foreach (int x in nums2) {
            if (cnt.ContainsKey(x) && cnt[x] > 0) {
                ans.Add(x);
                cnt[x]--;
            }
        }
        return ans.ToArray();
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums1
     * @param Integer[] $nums2
     * @return Integer[]
     */
    function intersect($nums1, $nums2) {
        $cnt = [];
        foreach ($nums1 as $x) {
            if (isset($cnt[$x])) {
                $cnt[$x]++;
            } else {
                $cnt[$x] = 1;
            }
        }

        $ans = [];
        foreach ($nums2 as $x) {
            if (isset($cnt[$x]) && $cnt[$x] > 0) {
                $ans[] = $x;
                $cnt[$x]--;
            }
        }

        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
