---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [325. Maximum Size Subarray Sum Equals k 🔒](https://leetcode.com/problems/maximum-size-subarray-sum-equals-k)

[中文文档](/solution/0300-0399/0325.Maximum%20Size%20Subarray%20Sum%20Equals%20k/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>, hãy trả về <em>độ dài lớn nhất của một </em><span data-keyword="subarray"><em>mảng con</em></span><em> có tổng bằng</em> <code>k</code>. Nếu không có mảng con nào như vậy, trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,-1,5,-2,3], k = 3
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Mảng con [1, -1, 5, -2] có tổng bằng 3 và là mảng con dài nhất thỏa mãn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-2,-1,2,1], k = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Mảng con [-1, 2] có tổng bằng 1 và là mảng con dài nhất thỏa mãn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>9</sup>&nbsp;&lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm mảng con dài nhất có tổng bằng $k$. Hai vòng lặp sẽ tốn $O(n^2)$. Tổng mảng con là hiệu hai prefix sum $s_i-s_j=k$, nên ta cần tìm vị trí sớm nhất có $s_j=s_i-k$.
>
> Lưu chỉ số xuất hiện đầu tiên của mỗi prefix sum (bao gồm $0\mapsto -1$). Nếu đã có $s-k$, cập nhật độ dài bằng $i-j$; chỉ lưu $s$ khi chưa xuất hiện để giữ lại vị trí sớm nhất.

<!-- thinking:end -->

Ta dùng hash table $\textit{d}$ để lưu chỉ số xuất hiện đầu tiên của mỗi prefix sum trong mảng $\textit{nums}$, khởi tạo $\textit{d}[0] = -1$. Ngoài ra, ta định nghĩa biến $\textit{s}$ để theo dõi prefix sum hiện tại.

Tiếp theo, ta duyệt mảng $\textit{nums}$. Với số hiện tại $\textit{nums}[i]$, ta cập nhật prefix sum thành $\textit{s} = \textit{s} + \textit{nums}[i]$. Nếu $\textit{s} - k$ có trong hash table $\textit{d}$, đặt $\textit{j} = \textit{d}[\textit{s} - k]$; khi đó, mảng con kết thúc tại $\textit{nums}[i]$ và thỏa điều kiện có độ dài $i - j$. Ta dùng biến $\textit{ans}$ để lưu độ dài lớn nhất tìm được. Sau đó, nếu $\textit{s}$ chưa có trong hash table, ta lưu $\textit{s}$ cùng chỉ số tương ứng $i$ bằng cách đặt $\textit{d}[\textit{s}] = i$. Nếu đã có thì không cập nhật $\textit{d}[\textit{s}]$. Có thể có nhiều chỉ số $i$ cùng prefix sum $\textit{s}$, vì vậy ta chỉ lưu chỉ số nhỏ nhất để tìm được mảng con dài nhất.

Sau khi duyệt xong, ta trả về $\textit{ans}$.



Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSubArrayLen(self, nums: List[int], k: int) -> int:
        d = {0: -1}
        ans = s = 0
        for i, x in enumerate(nums):
            s += x
            if s - k in d:
                ans = max(ans, i - d[s - k])
            if s not in d:
                d[s] = i
        return ans
```

#### Java

```java
class Solution {
    public int maxSubArrayLen(int[] nums, int k) {
        Map<Long, Integer> d = new HashMap<>();
        d.put(0L, -1);
        int ans = 0;
        long s = 0;
        for (int i = 0; i < nums.length; ++i) {
            s += nums[i];
            ans = Math.max(ans, i - d.getOrDefault(s - k, i));
            d.putIfAbsent(s, i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSubArrayLen(vector<int>& nums, int k) {
        unordered_map<long long, int> d{{0, -1}};
        int ans = 0;
        long long s = 0;
        for (int i = 0; i < nums.size(); ++i) {
            s += nums[i];
            if (d.count(s - k)) {
                ans = max(ans, i - d[s - k]);
            }
            if (!d.count(s)) {
                d[s] = i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxSubArrayLen(nums []int, k int) (ans int) {
	d := map[int]int{0: -1}
	s := 0
	for i, x := range nums {
		s += x
		if j, ok := d[s-k]; ok && ans < i-j {
			ans = i - j
		}
		if _, ok := d[s]; !ok {
			d[s] = i
		}
	}
	return
}
```

#### TypeScript

```ts
function maxSubArrayLen(nums: number[], k: number): number {
    const d: Map<number, number> = new Map();
    d.set(0, -1);
    let ans = 0;
    let s = 0;
    for (let i = 0; i < nums.length; ++i) {
        s += nums[i];
        if (d.has(s - k)) {
            ans = Math.max(ans, i - d.get(s - k)!);
        }
        if (!d.has(s)) {
            d.set(s, i);
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn max_sub_array_len(nums: Vec<i32>, k: i32) -> i32 {
        let mut d = HashMap::new();
        d.insert(0, -1);
        let mut ans = 0;
        let mut s = 0;

        for (i, &x) in nums.iter().enumerate() {
            s += x;
            if let Some(&j) = d.get(&(s - k)) {
                ans = ans.max((i as i32) - j);
            }
            d.entry(s).or_insert(i as i32);
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var maxSubArrayLen = function (nums, k) {
    const d = new Map();
    d.set(0, -1);
    let ans = 0;
    let s = 0;
    for (let i = 0; i < nums.length; ++i) {
        s += nums[i];
        if (d.has(s - k)) {
            ans = Math.max(ans, i - d.get(s - k));
        }
        if (!d.has(s)) {
            d.set(s, i);
        }
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int MaxSubArrayLen(int[] nums, int k) {
        var d = new Dictionary<int, int>();
        d[0] = -1;
        int ans = 0;
        int s = 0;
        for (int i = 0; i < nums.Length; i++) {
            s += nums[i];
            if (d.ContainsKey(s - k)) {
                ans = Math.Max(ans, i - d[s - k]);
            }
            if (!d.ContainsKey(s)) {
                d[s] = i;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
