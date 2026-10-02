---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Backtracking
    - Bitmask
---

<!-- problem:start -->

# [465. Optimal Account Balancing 🔒](https://leetcode.com/problems/optimal-account-balancing)

[中文文档](/solution/0400-0499/0465.Optimal%20Account%20Balancing/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng giao dịch <code>transactions</code>, trong đó <code>transactions[i] = [from<sub>i</sub>, to<sub>i</sub>, amount<sub>i</sub>]</code> cho biết người có <code>ID = from<sub>i</sub></code> đã đưa <code>amount<sub>i</sub> $</code> cho người có <code>ID = to<sub>i</sub></code>.</p>

<p>Hãy trả về <em>số giao dịch ít nhất cần thực hiện để tất toán các khoản nợ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> transactions = [[0,1,10],[2,0,5]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Người #0 đã đưa $10 cho người #1.
Người #2 đã đưa $5 cho người #0.
Cần hai giao dịch. Một cách tất toán khoản nợ là người #1 trả $5 cho mỗi người #0 và #2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> transactions = [[0,1,10],[1,0,1],[1,2,5],[2,0,5]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Người #0 đã đưa $10 cho người #1.
Người #1 đã đưa $1 cho người #0.
Người #1 đã đưa $5 cho người #2.
Người #2 đã đưa $5 cho người #0.
Vì vậy, người #1 chỉ cần đưa $4 cho người #0 là tất cả khoản nợ được tất toán.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= transactions.length &lt;= 8</code></li>
	<li><code>transactions[i].length == 3</code></li>
	<li><code>0 &lt;= from<sub>i</sub>, to<sub>i</sub> &lt; 12</code></li>
	<li><code>from<sub>i</sub> != to<sub>i</sub></code></li>
	<li><code>1 &lt;= amount<sub>i</sub> &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần ít giao dịch nhất để đưa số dư của mọi người về $0$. Bỏ qua những người đã có số dư bằng $0$. Việc thử mọi thứ tự giao dịch giữa tối đa $m\le 12$ người còn lại vẫn quá lớn; subset DP phù hợp hơn.
>
> Một tập con có tổng số dư bằng $0$ cần tối đa $|S|-1$ giao dịch. $f[i]$ là số giao dịch ít nhất cho mask $i$: chỉ mask có tổng bằng $0$ mới khả thi; khởi tạo bằng $|i|-1$, rồi thử $f[j]+f[i\oplus j]$ với các tập con thực sự không rỗng.
>
> Liệt kê các tập con có độ phức tạp $O(3^m)$. Trước tiên, gộp các giao dịch thành số dư khác $0$, rồi chỉ thực hiện chuyển khoản trên những mask có tổng số dư bằng $0$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minTransfers(self, transactions: List[List[int]]) -> int:
        g = defaultdict(int)
        for f, t, x in transactions:
            g[f] -= x
            g[t] += x
        nums = [x for x in g.values() if x]
        m = len(nums)
        f = [inf] * (1 << m)
        f[0] = 0
        for i in range(1, 1 << m):
            s = 0
            for j, x in enumerate(nums):
                if i >> j & 1:
                    s += x
            if s == 0:
                f[i] = i.bit_count() - 1
                j = (i - 1) & i
                while j > 0:
                    f[i] = min(f[i], f[j] + f[i ^ j])
                    j = (j - 1) & i
        return f[-1]
```

#### Java

```java
class Solution {
    public int minTransfers(int[][] transactions) {
        int[] g = new int[12];
        for (var t : transactions) {
            g[t[0]] -= t[2];
            g[t[1]] += t[2];
        }
        List<Integer> nums = new ArrayList<>();
        for (int x : g) {
            if (x != 0) {
                nums.add(x);
            }
        }
        int m = nums.size();
        int[] f = new int[1 << m];
        Arrays.fill(f, 1 << 29);
        f[0] = 0;
        for (int i = 1; i < 1 << m; ++i) {
            int s = 0;
            for (int j = 0; j < m; ++j) {
                if ((i >> j & 1) == 1) {
                    s += nums.get(j);
                }
            }
            if (s == 0) {
                f[i] = Integer.bitCount(i) - 1;
                for (int j = (i - 1) & i; j > 0; j = (j - 1) & i) {
                    f[i] = Math.min(f[i], f[j] + f[i ^ j]);
                }
            }
        }
        return f[(1 << m) - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minTransfers(vector<vector<int>>& transactions) {
        int g[12]{};
        for (auto& t : transactions) {
            g[t[0]] -= t[2];
            g[t[1]] += t[2];
        }
        vector<int> nums;
        for (int x : g) {
            if (x) {
                nums.push_back(x);
            }
        }
        int m = nums.size();
        int f[1 << m];
        memset(f, 0x3f, sizeof(f));
        f[0] = 0;
        for (int i = 1; i < 1 << m; ++i) {
            int s = 0;
            for (int j = 0; j < m; ++j) {
                if (i >> j & 1) {
                    s += nums[j];
                }
            }
            if (s == 0) {
                f[i] = __builtin_popcount(i) - 1;
                for (int j = (i - 1) & i; j; j = (j - 1) & i) {
                    f[i] = min(f[i], f[j] + f[i ^ j]);
                }
            }
        }
        return f[(1 << m) - 1];
    }
};
```

#### Go

```go
func minTransfers(transactions [][]int) int {
	g := [12]int{}
	for _, t := range transactions {
		g[t[0]] -= t[2]
		g[t[1]] += t[2]
	}
	nums := []int{}
	for _, x := range g {
		if x != 0 {
			nums = append(nums, x)
		}
	}
	m := len(nums)
	f := make([]int, 1<<m)
	for i := 1; i < 1<<m; i++ {
		f[i] = 1 << 29
		s := 0
		for j, x := range nums {
			if i>>j&1 == 1 {
				s += x
			}
		}
		if s == 0 {
			f[i] = bits.OnesCount(uint(i)) - 1
			for j := (i - 1) & i; j > 0; j = (j - 1) & i {
				f[i] = min(f[i], f[j]+f[i^j])
			}
		}
	}
	return f[1<<m-1]
}
```

#### TypeScript

```ts
function minTransfers(transactions: number[][]): number {
    const g: number[] = new Array(12).fill(0);
    for (const [f, t, x] of transactions) {
        g[f] -= x;
        g[t] += x;
    }
    const nums = g.filter(x => x !== 0);
    const m = nums.length;
    const f: number[] = new Array(1 << m).fill(1 << 29);
    f[0] = 0;
    for (let i = 1; i < 1 << m; ++i) {
        let s = 0;
        for (let j = 0; j < m; ++j) {
            if (((i >> j) & 1) === 1) {
                s += nums[j];
            }
        }
        if (s === 0) {
            f[i] = bitCount(i) - 1;
            for (let j = (i - 1) & i; j; j = (j - 1) & i) {
                f[i] = Math.min(f[i], f[j] + f[i ^ j]);
            }
        }
    }
    return f[(1 << m) - 1];
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
