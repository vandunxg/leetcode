---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
    - Counting
    - Sorting
    - Sliding Window
---

<!-- problem:start -->

# [594. Longest Harmonious Subsequence](https://leetcode.com/problems/longest-harmonious-subsequence)

[中文文档](/solution/0500-0599/0594.Longest%20Harmonious%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Một mảng được gọi là hài hòa nếu hiệu giữa giá trị lớn nhất và nhỏ nhất của nó <b>bằng chính xác</b> <code>1</code>.</p>

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về độ dài của <span data-keyword="subsequence-array">dãy con</span> hài hòa dài nhất trong số tất cả các dãy con có thể tạo thành.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,2,2,5,2,3,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con hài hòa dài nhất là <code>[3,2,2,2,3]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các dãy con hài hòa dài nhất là <code>[1,2]</code>, <code>[2,3]</code> và <code>[3,4]</code>; mỗi dãy đều có độ dài 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không tồn tại dãy con hài hòa.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Dãy con hài hòa có giá trị lớn nhất và nhỏ nhất chênh lệch đúng $1$. Thứ tự không quan trọng, chỉ các giá trị mới quan trọng. Không khả thi nếu liệt kê mọi dãy con.
>
> Đếm tần suất xuất hiện. Với mỗi $x$ có giá trị liền kề $x+1$, tổng $cnt[x]+cnt[x+1]$ là độ dài lớn nhất của dãy con chỉ gồm hai giá trị này. Lấy giá trị lớn nhất.

<!-- thinking:end -->

Ta có thể dùng hash table $\textit{cnt}$ để ghi lại số lần xuất hiện của mỗi phần tử trong mảng $\textit{nums}$. Sau đó, duyệt từng cặp key-value $(x, c)$ trong hash table. Nếu hash table có key $x + 1$, tổng số lần xuất hiện của $x$ và $x + 1$, tức $c + \textit{cnt}[x + 1]$, tạo thành một dãy con hài hòa. Ta chỉ cần tìm độ dài lớn nhất trong các dãy con hài hòa đó.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLHS(self, nums: List[int]) -> int:
        cnt = Counter(nums)
        return max((c + cnt[x + 1] for x, c in cnt.items() if cnt[x + 1]), default=0)
```

#### Java

```java
class Solution {
    public int findLHS(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            cnt.merge(x, 1, Integer::sum);
        }
        int ans = 0;
        for (var e : cnt.entrySet()) {
            int x = e.getKey(), c = e.getValue();
            if (cnt.containsKey(x + 1)) {
                ans = Math.max(ans, c + cnt.get(x + 1));
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
    int findLHS(vector<int>& nums) {
        unordered_map<int, int> cnt;
        for (int x : nums) {
            ++cnt[x];
        }
        int ans = 0;
        for (auto& [x, c] : cnt) {
            if (cnt.contains(x + 1)) {
                ans = max(ans, c + cnt[x + 1]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findLHS(nums []int) (ans int) {
	cnt := map[int]int{}
	for _, x := range nums {
		cnt[x]++
	}
	for x, c := range cnt {
		if c1, ok := cnt[x+1]; ok {
			ans = max(ans, c+c1)
		}
	}
	return
}
```

#### TypeScript

```ts
function findLHS(nums: number[]): number {
    const cnt: Record<number, number> = {};
    for (const x of nums) {
        cnt[x] = (cnt[x] || 0) + 1;
    }
    let ans = 0;
    for (const [x, c] of Object.entries(cnt)) {
        const y = +x + 1;
        if (cnt[y]) {
            ans = Math.max(ans, c + cnt[y]);
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn find_lhs(nums: Vec<i32>) -> i32 {
        let mut cnt = HashMap::new();
        for &x in &nums {
            *cnt.entry(x).or_insert(0) += 1;
        }
        let mut ans = 0;
        for (&x, &c) in &cnt {
            if let Some(&y) = cnt.get(&(x + 1)) {
                ans = ans.max(c + y);
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
