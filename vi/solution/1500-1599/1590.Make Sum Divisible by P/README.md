---
comments: true
difficulty: Medium
rating: 2038
source: Biweekly Contest 35 Q3
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [1590. Make Sum Divisible by P](https://leetcode.com/problems/make-sum-divisible-by-p)

[中文文档](/solution/1500-1599/1590.Make%20Sum%20Divisible%20by%20P/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên dương <code>nums</code>, hãy xóa <strong>mảng con nhỏ nhất</strong> (có thể <strong>rỗng</strong>) sao cho <strong>tổng</strong> các phần tử còn lại chia hết cho <code>p</code>. <strong>Không được</strong> xóa toàn bộ mảng.</p>

<p>Trả về <em>độ dài mảng con nhỏ nhất cần xóa, hoặc </em><code>-1</code><em> nếu không thể</em>.</p>

<p><strong>Mảng con</strong> là một đoạn liên tiếp các phần tử trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [3,1,4,2], p = 6
<strong>Output:</strong> 1
<strong>Giải thích:</strong> Tổng các phần tử trong nums là 10, không chia hết cho 6. Ta có thể xóa mảng con [4], khi đó tổng các phần tử còn lại là 6 và chia hết cho 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [6,3,5,2], p = 9
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Không thể xóa một phần tử để có tổng chia hết cho 9. Cách tốt nhất là xóa mảng con [5,2], còn lại [6,3] có tổng bằng 9.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3], p = 3
<strong>Output:</strong> 0
<strong>Giải thích:</strong> Tổng ở đây là 6, vốn đã chia hết cho 3. Vì vậy ta không cần xóa gì.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= p &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Xóa mảng con ngắn nhất để tổng còn lại chia hết cho $p$. Với $n\le 10^5$, không thể duyệt mọi đoạn. Nếu tổng có phần dư $k$ khi chia cho $p$, đoạn bị xóa cũng phải có phần dư $k$.
>
> Duyệt các prefix sum theo modulo $p$ và lưu chỉ số cuối cùng của mỗi phần dư. Với phần dư $cur$, tìm $(cur-k)\bmod p$; khoảng cách giữa hai chỉ số là một ứng viên. Không được xóa toàn bộ mảng.

<!-- thinking:end -->

Trước hết, ta tính tổng tất cả phần tử trong mảng $\textit{nums}$ theo modulo $p$, gọi là $k$. Nếu $k$ bằng $0$, tổng các phần tử trong mảng $\textit{nums}$ là bội của $p$, nên ta trả về $0$.

Nếu $k$ khác $0$, ta cần tìm mảng con ngắn nhất sao cho sau khi xóa, tổng các phần tử còn lại có phần dư bằng $0$ khi chia cho $p$.

Ta có thể duyệt mảng $\textit{nums}$, duy trì prefix sum hiện tại theo modulo $p$, gọi là $cur$. Ta dùng hash table $last$ để ghi nhận lần xuất hiện cuối cùng của mỗi prefix sum theo modulo $p$.

If there exists a subarray ending at $\textit{nums}[i]$ such that removing this subarray makes the sum of the remaining elements modulo $p$ equal to $0$, we need to find a previous prefix sum modulo $p$ equal to $target$ at position $j$ such that $(target + k - cur) \bmod p = 0$. If found, we can remove the subarray $\textit{nums}[j+1,..i]$ to make the sum of the remaining elements modulo $p$ equal to $0$; here $p$ is the divisor.

Do đó, nếu tồn tại $target = (cur - k + p) \bmod p$, ta cập nhật đáp án bằng $\min(ans, i - j)$. Sau đó, cập nhật $last[cur]$ thành $i$. Ta tiếp tục duyệt mảng $\textit{nums}$ đến cuối để nhận đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSubarray(self, nums: List[int], p: int) -> int:
        k = sum(nums) % p
        if k == 0:
            return 0
        last = {0: -1}
        cur = 0
        ans = len(nums)
        for i, x in enumerate(nums):
            cur = (cur + x) % p
            target = (cur - k + p) % p
            if target in last:
                ans = min(ans, i - last[target])
            last[cur] = i
        return -1 if ans == len(nums) else ans
```

#### Java

```java
class Solution {
    public int minSubarray(int[] nums, int p) {
        int k = 0;
        for (int x : nums) {
            k = (k + x) % p;
        }
        if (k == 0) {
            return 0;
        }
        Map<Integer, Integer> last = new HashMap<>();
        last.put(0, -1);
        int n = nums.length;
        int ans = n;
        int cur = 0;
        for (int i = 0; i < n; ++i) {
            cur = (cur + nums[i]) % p;
            int target = (cur - k + p) % p;
            if (last.containsKey(target)) {
                ans = Math.min(ans, i - last.get(target));
            }
            last.put(cur, i);
        }
        return ans == n ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSubarray(vector<int>& nums, int p) {
        int k = 0;
        for (int& x : nums) {
            k = (k + x) % p;
        }
        if (k == 0) {
            return 0;
        }
        unordered_map<int, int> last;
        last[0] = -1;
        int n = nums.size();
        int ans = n;
        int cur = 0;
        for (int i = 0; i < n; ++i) {
            cur = (cur + nums[i]) % p;
            int target = (cur - k + p) % p;
            if (last.count(target)) {
                ans = min(ans, i - last[target]);
            }
            last[cur] = i;
        }
        return ans == n ? -1 : ans;
    }
};
```

#### Go

```go
func minSubarray(nums []int, p int) int {
	k := 0
	for _, x := range nums {
		k = (k + x) % p
	}
	if k == 0 {
		return 0
	}
	last := map[int]int{0: -1}
	n := len(nums)
	ans := n
	cur := 0
	for i, x := range nums {
		cur = (cur + x) % p
		target := (cur - k + p) % p
		if j, ok := last[target]; ok {
			ans = min(ans, i-j)
		}
		last[cur] = i
	}
	if ans == n {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minSubarray(nums: number[], p: number): number {
    let k = 0;
    for (const x of nums) {
        k = (k + x) % p;
    }
    if (k === 0) {
        return 0;
    }
    const last = new Map<number, number>();
    last.set(0, -1);
    const n = nums.length;
    let ans = n;
    let cur = 0;
    for (let i = 0; i < n; ++i) {
        cur = (cur + nums[i]) % p;
        const target = (cur - k + p) % p;
        if (last.has(target)) {
            const j = last.get(target)!;
            ans = Math.min(ans, i - j);
        }
        last.set(cur, i);
    }
    return ans === n ? -1 : ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn min_subarray(nums: Vec<i32>, p: i32) -> i32 {
        let mut k = 0;
        for &x in &nums {
            k = (k + x) % p;
        }
        if k == 0 {
            return 0;
        }

        let mut last = HashMap::new();
        last.insert(0, -1);
        let n = nums.len();
        let mut ans = n as i32;
        let mut cur = 0;

        for i in 0..n {
            cur = (cur + nums[i]) % p;
            let target = (cur - k + p) % p;
            if let Some(&prev_idx) = last.get(&target) {
                ans = ans.min(i as i32 - prev_idx);
            }
            last.insert(cur, i as i32);
        }

        if ans == n as i32 {
            -1
        } else {
            ans
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} p
 * @return {number}
 */
var minSubarray = function (nums, p) {
    let k = 0;
    for (const x of nums) {
        k = (k + x) % p;
    }
    if (k === 0) {
        return 0;
    }
    const last = new Map();
    last.set(0, -1);
    const n = nums.length;
    let ans = n;
    let cur = 0;
    for (let i = 0; i < n; ++i) {
        cur = (cur + nums[i]) % p;
        const target = (cur - k + p) % p;
        if (last.has(target)) {
            const j = last.get(target);
            ans = Math.min(ans, i - j);
        }
        last.set(cur, i);
    }
    return ans === n ? -1 : ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
