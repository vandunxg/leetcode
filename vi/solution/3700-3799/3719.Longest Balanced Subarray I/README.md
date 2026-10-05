---
comments: true
difficulty: Medium
rating: 1467
source: Weekly Contest 472 Q2
tags:
    - Segment Tree
    - Array
    - Hash Table
    - Divide and Conquer
    - Prefix Sum
---

<!-- problem:start -->

# [3719. Longest Balanced Subarray I](https://leetcode.com/problems/longest-balanced-subarray-i)

[中文文档](/solution/3700-3799/3719.Longest%20Balanced%20Subarray%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Một <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> được gọi là <strong>cân bằng</strong> nếu số lượng <strong>các số chẵn phân biệt</strong> trong mảng con bằng số lượng <strong>các số lẻ phân biệt</strong>.</p>

<p>Hãy trả về độ dài của mảng con cân bằng <strong>dài nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,5,4,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mảng con cân bằng dài nhất là <code>[2, 5, 4, 3]</code>.</li>
	<li>Mảng này có 2 số chẵn phân biệt <code>[2, 4]</code> và 2 số lẻ phân biệt <code>[5, 3]</code>. Do đó, đáp án là 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,2,2,5,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mảng con cân bằng dài nhất là <code>[3, 2, 2, 5, 4]</code>.</li>
	<li>Mảng này có 2 số chẵn phân biệt <code>[2, 4]</code> và 2 số lẻ phân biệt <code>[3, 5]</code>. Do đó, đáp án là 5.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mảng con cân bằng dài nhất là <code>[2, 3, 2]</code>.</li>
	<li>Mảng này có 1 số chẵn phân biệt <code>[2]</code> và 1 số lẻ phân biệt <code>[3]</code>. Do đó, đáp án là 3.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1500</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Cân bằng số lượng các giá trị chẵn phân biệt với các giá trị lẻ phân biệt, vì vậy không được đếm một giá trị trùng lặp nhiều lần. Với giới hạn đề bài, $O(n^2)$ là đủ: cố định đầu trái, quét đầu phải, và chỉ cập nhật bộ đếm chẵn/lẻ khi một giá trị xuất hiện lần đầu.

<!-- thinking:end -->

Chúng ta có thể liệt kê điểm đầu trái $i$ của mảng con, sau đó liệt kê điểm đầu phải $j$ từ điểm đầu trái. Trong quá trình liệt kê, chúng ta dùng một hash table $\textit{vis}$ để ghi lại các số đã xuất hiện trong mảng con, đồng thời dùng một mảng $\textit{cnt}$ có độ dài $2$ để lần lượt ghi lại số lượng các số chẵn phân biệt và số lẻ phân biệt trong mảng con. Khi $\textit{cnt}[0] = \textit{cnt}[1]$, chúng ta cập nhật đáp án $\textit{ans} = \max(\textit{ans}, j - i + 1)$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestBalanced(self, nums: List[int]) -> int:
        n = len(nums)
        ans = 0
        for i in range(n):
            cnt = [0, 0]
            vis = set()
            for j in range(i, n):
                if nums[j] not in vis:
                    cnt[nums[j] & 1] += 1
                    vis.add(nums[j])
                if cnt[0] == cnt[1]:
                    ans = max(ans, j - i + 1)
        return ans
```

#### Java

```java
class Solution {
    public int longestBalanced(int[] nums) {
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            Set<Integer> vis = new HashSet<>();
            int[] cnt = new int[2];
            for (int j = i; j < n; ++j) {
                if (vis.add(nums[j])) {
                    ++cnt[nums[j] & 1];
                }
                if (cnt[0] == cnt[1]) {
                    ans = Math.max(ans, j - i + 1);
                }
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
    int longestBalanced(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            unordered_set<int> vis;
            int cnt[2]{};
            for (int j = i; j < n; ++j) {
                if (!vis.contains(nums[j])) {
                    vis.insert(nums[j]);
                    ++cnt[nums[j] & 1];
                }
                if (cnt[0] == cnt[1]) {
                    ans = max(ans, j - i + 1);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestBalanced(nums []int) (ans int) {
	n := len(nums)
	for i := 0; i < n; i++ {
		vis := map[int]bool{}
		cnt := [2]int{}
		for j := i; j < n; j++ {
			if !vis[nums[j]] {
				vis[nums[j]] = true
				cnt[nums[j]&1]++
			}
			if cnt[0] == cnt[1] {
				ans = max(ans, j-i+1)
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function longestBalanced(nums: number[]): number {
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        const vis = new Set<number>();
        const cnt: number[] = Array(2).fill(0);
        for (let j = i; j < n; ++j) {
            if (!vis.has(nums[j])) {
                vis.add(nums[j]);
                ++cnt[nums[j] & 1];
            }
            if (cnt[0] === cnt[1]) {
                ans = Math.max(ans, j - i + 1);
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn longest_balanced(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut ans: i32 = 0;

        for i in 0..n {
            let mut vis: HashSet<i32> = HashSet::new();
            let mut cnt = [0i32; 2];

            for j in i..n {
                if !vis.contains(&nums[j]) {
                    vis.insert(nums[j]);
                    let idx = (nums[j] & 1) as usize;
                    cnt[idx] += 1;
                }
                if cnt[0] == cnt[1] {
                    ans = ans.max((j - i + 1) as i32);
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
