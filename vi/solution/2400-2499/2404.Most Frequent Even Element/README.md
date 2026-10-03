---
comments: true
difficulty: Easy
rating: 1259
source: Weekly Contest 310 Q1
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [2404. Most Frequent Even Element](https://leetcode.com/problems/most-frequent-even-element)

[中文文档](/solution/2400-2499/2404.Most%20Frequent%20Even%20Element/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, hãy trả về <em>phần tử chẵn xuất hiện nhiều nhất</em>.</p>

<p>Nếu có nhiều phần tử cùng tần suất, hãy trả về phần tử <strong>nhỏ nhất</strong>. Nếu không có phần tử nào như vậy, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,2,2,4,4,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Các phần tử chẵn là 0, 2 và 4. Trong số đó, 2 và 4 xuất hiện nhiều nhất.
Ta trả về phần tử nhỏ nhất là 2.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,4,4,9,2,4]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 4 là phần tử chẵn xuất hiện nhiều nhất.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [29,47,21,41,13,37,25,7]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có phần tử chẵn nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 2000$, chỉ cần duyệt một lượt để đếm tần suất các phần tử chẵn. Ta cần tìm phần tử chẵn xuất hiện nhiều nhất, nếu hòa thì chọn giá trị nhỏ nhất. Một hash map lưu số lần xuất hiện kết hợp với việc duyệt tuyến tính các cặp là đủ; không cần sắp xếp.

<!-- thinking:end -->

Ta dùng một hash table $cnt$ để đếm số lần xuất hiện của tất cả phần tử chẵn, sau đó tìm phần tử chẵn có số lần xuất hiện lớn nhất và giá trị nhỏ nhất.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mostFrequentEven(self, nums: List[int]) -> int:
        cnt = Counter(x for x in nums if x % 2 == 0)
        ans, mx = -1, 0
        for x, v in cnt.items():
            if v > mx or (v == mx and ans > x):
                ans, mx = x, v
        return ans
```

#### Java

```java
class Solution {
    public int mostFrequentEven(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            if (x % 2 == 0) {
                cnt.merge(x, 1, Integer::sum);
            }
        }
        int ans = -1, mx = 0;
        for (var e : cnt.entrySet()) {
            int x = e.getKey(), v = e.getValue();
            if (mx < v || (mx == v && ans > x)) {
                ans = x;
                mx = v;
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
    int mostFrequentEven(vector<int>& nums) {
        unordered_map<int, int> cnt;
        for (int x : nums) {
            if (x % 2 == 0) {
                ++cnt[x];
            }
        }
        int ans = -1, mx = 0;
        for (auto& [x, v] : cnt) {
            if (mx < v || (mx == v && ans > x)) {
                ans = x;
                mx = v;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func mostFrequentEven(nums []int) int {
	cnt := map[int]int{}
	for _, x := range nums {
		if x%2 == 0 {
			cnt[x]++
		}
	}
	ans, mx := -1, 0
	for x, v := range cnt {
		if mx < v || (mx == v && x < ans) {
			ans, mx = x, v
		}
	}
	return ans
}
```

#### TypeScript

```ts
function mostFrequentEven(nums: number[]): number {
    const cnt: Map<number, number> = new Map();
    for (const x of nums) {
        if (x % 2 === 0) {
            cnt.set(x, (cnt.get(x) ?? 0) + 1);
        }
    }
    let ans = -1;
    let mx = 0;
    for (const [x, v] of cnt) {
        if (mx < v || (mx === v && ans > x)) {
            ans = x;
            mx = v;
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;
impl Solution {
    pub fn most_frequent_even(nums: Vec<i32>) -> i32 {
        let mut cnt = HashMap::new();
        for &x in nums.iter() {
            if x % 2 == 0 {
                *cnt.entry(x).or_insert(0) += 1;
            }
        }
        let mut ans = -1;
        let mut mx = 0;
        for (&x, &v) in cnt.iter() {
            if mx < v || (mx == v && ans > x) {
                ans = x;
                mx = v;
            }
        }
        ans
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums
     * @return Integer
     */
    function mostFrequentEven($nums) {
        $max = $rs = -1;
        for ($i = 0; $i < count($nums); $i++) {
            if ($nums[$i] % 2 == 0) {
                $hashtable[$nums[$i]] += 1;
                if (
                    $hashtable[$nums[$i]] > $max ||
                    ($hashtable[$nums[$i]] == $max && $rs > $nums[$i])
                ) {
                    $max = $hashtable[$nums[$i]];
                    $rs = $nums[$i];
                }
            }
        }
        return $rs;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
