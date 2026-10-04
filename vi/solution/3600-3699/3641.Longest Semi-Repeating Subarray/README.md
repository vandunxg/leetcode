---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [3641. Longest Semi-Repeating Subarray 🔒](https://leetcode.com/problems/longest-semi-repeating-subarray)

[中文文档](/solution/3600-3699/3641.Longest%20Semi-Repeating%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>k</code>.</p>

<p>Mảng con <strong>bán lặp</strong> là một mảng con liên tiếp trong đó có nhiều nhất <code>k</code> phần tử lặp lại (tức là xuất hiện nhiều hơn một lần).</p>

<p>Trả về độ dài của mảng con <strong>bán lặp</strong> dài nhất trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,1,2,3,4], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con bán lặp dài nhất là <code>[2, 3, 1, 2, 3, 4]</code>, có hai phần tử lặp lại (2 và 3).</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1,1,1], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con bán lặp dài nhất là <code>[1, 1, 1, 1, 1]</code>, chỉ có một phần tử lặp lại (1).</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1,1,1], k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con bán lặp dài nhất là <code>[1]</code>, không có phần tử nào lặp lại.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Một cửa sổ là bán lặp khi số lượng giá trị xuất hiện ít nhất hai lần không vượt quá $k$. Cửa sổ càng dài thì điều kiện càng chặt, vì vậy đầu trái chỉ di chuyển sang phải khi đầu phải tiến lên.
>
> Dùng frequency map để cập nhật số lượng phần tử lặp lại: tăng bộ đếm khi một tần suất tăng từ $1$ lên $2$, và giảm khi nó giảm từ $2$ xuống $1$. Thu hẹp phía trái khi bộ đếm này vượt quá $k$.
>
> Mỗi chỉ số được thêm vào và loại khỏi cửa sổ đúng một lần; đáp án là cửa sổ hợp lệ dài nhất.

<!-- thinking:end -->

Ta dùng hai con trỏ $l$ và $r$ để duy trì một sliding window, trong đó con trỏ phải liên tục di chuyển sang phải, đồng thời dùng hash table $\textit{cnt}$ để ghi lại số lần xuất hiện của mỗi phần tử trong cửa sổ hiện tại.

Khi số lần xuất hiện của một phần tử thay đổi từ $1$ thành $2$, điều đó cho biết có thêm một phần tử lặp lại, nên ta tăng bộ đếm số phần tử lặp lại $\textit{cur}$ lên $1$. Khi bộ đếm số phần tử lặp lại vượt quá $k$, cửa sổ hiện tại không còn thỏa mãn điều kiện, và ta cần di chuyển con trỏ trái cho đến khi bộ đếm số phần tử lặp lại không lớn hơn $k$. Trong quá trình di chuyển con trỏ trái, nếu số lần xuất hiện của một phần tử thay đổi từ $2$ thành $1$, điều đó cho biết số phần tử lặp lại đã giảm đi một, nên ta giảm bộ đếm số phần tử lặp lại đi $1$. Sau đó, ta cập nhật đáp án, tức là $\textit{ans} = \max(\textit{ans}, r - l + 1)$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSubarray(self, nums: List[int], k: int) -> int:
        cnt = defaultdict(int)
        ans = cur = l = 0
        for r, x in enumerate(nums):
            cnt[x] += 1
            cur += cnt[x] == 2
            while cur > k:
                cnt[nums[l]] -= 1
                cur -= cnt[nums[l]] == 1
                l += 1
            ans = max(ans, r - l + 1)
        return ans
```

#### Java

```java
class Solution {
    public int longestSubarray(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        int ans = 0, cur = 0, l = 0;
        for (int r = 0; r < nums.length; ++r) {
            if (cnt.merge(nums[r], 1, Integer::sum) == 2) {
                ++cur;
            }
            while (cur > k) {
                if (cnt.merge(nums[l++], -1, Integer::sum) == 1) {
                    --cur;
                }
            }
            ans = Math.max(ans, r - l + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& nums, int k) {
        unordered_map<int, int> cnt;
        int ans = 0, cur = 0, l = 0;
        for (int r = 0; r < nums.size(); ++r) {
            if (++cnt[nums[r]] == 2) {
                ++cur;
            }
            while (cur > k) {
                if (--cnt[nums[l++]] == 1) {
                    --cur;
                }
            }
            ans = max(ans, r - l + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func longestSubarray(nums []int, k int) (ans int) {
	cnt := make(map[int]int)
	cur, l := 0, 0
	for r := 0; r < len(nums); r++ {
		if cnt[nums[r]]++; cnt[nums[r]] == 2 {
			cur++
		}
		for cur > k {
			if cnt[nums[l]]--; cnt[nums[l]] == 1 {
				cur--
			}
			l++
		}
		ans = max(ans, r-l+1)
	}
	return
}
```

#### TypeScript

```ts
function longestSubarray(nums: number[], k: number): number {
    const cnt: Map<number, number> = new Map();
    let [ans, cur, l] = [0, 0, 0];
    for (let r = 0; r < nums.length; r++) {
        cnt.set(nums[r], (cnt.get(nums[r]) || 0) + 1);
        if (cnt.get(nums[r]) === 2) {
            cur++;
        }

        while (cur > k) {
            cnt.set(nums[l], cnt.get(nums[l])! - 1);
            if (cnt.get(nums[l]) === 1) {
                cur--;
            }
            l++;
        }

        ans = Math.max(ans, r - l + 1);
    }

    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn longest_subarray(nums: Vec<i32>, k: i32) -> i32 {
        let mut cnt = HashMap::new();
        let mut ans = 0;
        let mut cur = 0;
        let mut l = 0;

        for r in 0..nums.len() {
            let entry = cnt.entry(nums[r]).or_insert(0);
            *entry += 1;
            if *entry == 2 {
                cur += 1;
            }

            while cur > k {
                let entry = cnt.entry(nums[l]).or_insert(0);
                *entry -= 1;
                if *entry == 1 {
                    cur -= 1;
                }
                l += 1;
            }

            ans = ans.max(r - l + 1);
        }

        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
