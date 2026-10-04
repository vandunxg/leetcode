---
comments: true
difficulty: Medium
rating: 2032
source: Weekly Contest 453 Q3
tags:
    - Queue
    - Array
    - Dynamic Programming
    - Prefix Sum
    - Sliding Window
    - Monotonic Queue
---

<!-- problem:start -->

# [3578. Count Partitions With Max-Min Difference at Most K](https://leetcode.com/problems/count-partitions-with-max-min-difference-at-most-k)

[中文文档](/solution/3500-3599/3578.Count%20Partitions%20With%20Max-Min%20Difference%20at%20Most%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>. Nhiệm vụ của bạn là chia <code>nums</code> thành một hoặc nhiều đoạn liên tiếp <strong>không rỗng</strong> sao cho trong mỗi đoạn, hiệu giữa phần tử <strong>lớn nhất</strong> và phần tử <strong>nhỏ nhất</strong> <strong>không vượt quá</strong> <code>k</code>.</p>

<p>Trả về tổng số cách chia <code>nums</code> thỏa mãn điều kiện này.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [9,4,1,3,7], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 6 cách chia hợp lệ sao cho hiệu giữa phần tử lớn nhất và nhỏ nhất trong mỗi đoạn không vượt quá <code>k = 4</code>:</p>

<ul>
	<li><code>[[9], [4], [1], [3], [7]]</code></li>
	<li><code>[[9], [4], [1], [3, 7]]</code></li>
	<li><code>[[9], [4], [1, 3], [7]]</code></li>
	<li><code>[[9], [4, 1], [3], [7]]</code></li>
	<li><code>[[9], [4, 1], [3, 7]]</code></li>
	<li><code>[[9], [4, 1, 3], [7]]</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,3,4], k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 2 cách chia hợp lệ thỏa mãn các điều kiện đã cho:</p>

<ul>
	<li><code>[[3], [3], [4]]</code></li>
	<li><code>[[3, 3], [4]]</code></li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + Hai con trỏ + Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi phần của một phép chia phải thỏa mãn $\max-\min \le k$. Gọi $f[r]$ là số cách chia prefix có độ dài $r$; phần cuối là một đoạn nào đó $[l,r]$, nên $f[r]$ là tổng $f[j]$ trên các đoạn cuối hợp lệ.
>
> Biên trái hợp lệ chỉ di chuyển sang phải: một ordered set lưu các giá trị cực trị của cửa sổ và tăng $l$ khi khoảng cách vượt quá $k$. Prefix sum $g$ tính tổng trên một đoạn trong $O(1)$.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là số cách chia $i$ phần tử đầu tiên. Nếu một mảng thỏa mãn hiệu giữa giá trị lớn nhất và nhỏ nhất không vượt quá $k$, thì mọi mảng con của nó cũng thỏa mãn điều kiện này. Vì vậy, ta có thể dùng hai con trỏ để duy trì một sliding window biểu diễn mảng con hiện tại.

Khi xét đến phần tử thứ $r$, ta cần tìm con trỏ trái $l$ sao cho mảng con từ $l$ đến $r$ có hiệu giữa giá trị lớn nhất và nhỏ nhất không vượt quá $k$. Ta có thể dùng ordered set để duy trì các phần tử trong cửa sổ hiện tại, từ đó nhanh chóng lấy được giá trị lớn nhất và nhỏ nhất.

Mỗi khi thêm một phần tử mới, ta chèn nó vào ordered set và kiểm tra hiệu giữa giá trị lớn nhất và nhỏ nhất trong cửa sổ hiện tại. Nếu hiệu này vượt quá $k$, ta di chuyển con trỏ trái $l$ cho đến khi điều kiện được thỏa mãn. Số cách chia kết thúc tại phần tử thứ $r$ là $f[l - 1] + f[l] + \ldots + f[r - 1]$. Ta có thể dùng mảng prefix sum để tính nhanh giá trị này.

Đáp án là $f[n]$, trong đó $n$ là độ dài của mảng.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPartitions(self, nums: List[int], k: int) -> int:
        mod = 10**9 + 7
        sl = SortedList()
        n = len(nums)
        f = [1] + [0] * n
        g = [1] + [0] * n
        l = 1
        for r, x in enumerate(nums, 1):
            sl.add(x)
            while sl[-1] - sl[0] > k:
                sl.remove(nums[l - 1])
                l += 1
            f[r] = (g[r - 1] - (g[l - 2] if l >= 2 else 0) + mod) % mod
            g[r] = (g[r - 1] + f[r]) % mod
        return f[n]
```

#### Java

```java
class Solution {
    public int countPartitions(int[] nums, int k) {
        final int mod = (int) 1e9 + 7;
        TreeMap<Integer, Integer> sl = new TreeMap<>();
        int n = nums.length;
        int[] f = new int[n + 1];
        int[] g = new int[n + 1];
        f[0] = 1;
        g[0] = 1;
        int l = 1;
        for (int r = 1; r <= n; r++) {
            int x = nums[r - 1];
            sl.merge(x, 1, Integer::sum);
            while (sl.lastKey() - sl.firstKey() > k) {
                if (sl.merge(nums[l - 1], -1, Integer::sum) == 0) {
                    sl.remove(nums[l - 1]);
                }
                ++l;
            }
            f[r] = (g[r - 1] - (l >= 2 ? g[l - 2] : 0) + mod) % mod;
            g[r] = (g[r - 1] + f[r]) % mod;
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countPartitions(vector<int>& nums, int k) {
        const int mod = 1e9 + 7;
        multiset<int> sl;
        int n = nums.size();
        vector<int> f(n + 1, 0), g(n + 1, 0);
        f[0] = 1;
        g[0] = 1;
        int l = 1;
        for (int r = 1; r <= n; ++r) {
            int x = nums[r - 1];
            sl.insert(x);
            while (*sl.rbegin() - *sl.begin() > k) {
                sl.erase(sl.find(nums[l - 1]));
                ++l;
            }
            f[r] = (g[r - 1] - (l >= 2 ? g[l - 2] : 0) + mod) % mod;
            g[r] = (g[r - 1] + f[r]) % mod;
        }
        return f[n];
    }
};
```

#### Go

```go
func countPartitions(nums []int, k int) int {
	const mod int = 1e9 + 7
	sl := redblacktree.New[int, int]()
	merge := func(st *redblacktree.Tree[int, int], x, v int) {
		c, _ := st.Get(x)
		if c+v == 0 {
			st.Remove(x)
		} else {
			st.Put(x, c+v)
		}
	}
	n := len(nums)
	f := make([]int, n+1)
	g := make([]int, n+1)
	f[0], g[0] = 1, 1
	for l, r := 1, 1; r <= n; r++ {
		merge(sl, nums[r-1], 1)
		for sl.Right().Key-sl.Left().Key > k {
			merge(sl, nums[l-1], -1)
			l++
		}
		f[r] = g[r-1]
		if l >= 2 {
			f[r] = (f[r] - g[l-2] + mod) % mod
		}
		g[r] = (g[r-1] + f[r]) % mod
	}
	return f[n]
}
```

#### TypeScript

```ts
import { AvlTree } from 'datastructures-js';

function countPartitions(nums: number[], k: number): number {
    const mod = 1_000_000_007;
    const n = nums.length;

    const cnt = new Map<number, number>();
    const st = new AvlTree<number>((a, b) => a - b);

    function insert(x: number) {
        const c = (cnt.get(x) || 0) + 1;
        cnt.set(x, c);
        if (c === 1) {
            st.insert(x);
        }
    }

    function erase(x: number) {
        const c = cnt.get(x)! - 1;
        cnt.set(x, c);
        if (c === 0) {
            st.remove(x);
        }
    }

    const f = Array(n + 1).fill(0);
    const g = Array(n + 1).fill(0);
    f[0] = 1;
    g[0] = 1;

    for (let l = 1, r = 1; r <= n; ++r) {
        const x = nums[r - 1];
        insert(x);

        while (st.max()!.getValue() - st.min()!.getValue() > k) {
            erase(nums[l - 1]);
            l++;
        }

        f[r] = (g[r - 1] - (l >= 2 ? g[l - 2] : 0) + mod) % mod;
        g[r] = (g[r - 1] + f[r]) % mod;
    }

    return f[n];
}
```

#### Rust

```rust
use std::collections::BTreeMap;

impl Solution {
    pub fn count_partitions(nums: Vec<i32>, k: i32) -> i32 {
        const mod_val: i32 = 1_000_000_007;
        let n = nums.len();
        let mut f = vec![0; n + 1];
        let mut g = vec![0; n + 1];
        f[0] = 1;
        g[0] = 1;
        let mut sl = BTreeMap::new();
        let mut l = 1;
        for r in 1..=n {
            let x = nums[r - 1];
            *sl.entry(x).or_insert(0) += 1;
            while sl.keys().last().unwrap() - sl.keys().next().unwrap() > k {
                let val = nums[l - 1];
                if let Some(cnt) = sl.get_mut(&val) {
                    *cnt -= 1;
                    if *cnt == 0 {
                        sl.remove(&val);
                    }
                }
                l += 1;
            }
            f[r] = (g[r - 1] - if l >= 2 { g[l - 2] } else { 0 } + mod_val) % mod_val;
            g[r] = (g[r - 1] + f[r]) % mod_val;
        }
        f[n]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
