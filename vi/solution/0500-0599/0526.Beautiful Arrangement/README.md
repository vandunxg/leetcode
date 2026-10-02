---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Backtracking
    - Bitmask
---

<!-- problem:start -->

# [526. Beautiful Arrangement](https://leetcode.com/problems/beautiful-arrangement)

[中文文档](/solution/0500-0599/0526.Beautiful%20Arrangement/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>n</code> số nguyên được đánh số từ <code>1</code> đến <code>n</code>. Một hoán vị <code>perm</code> (<strong>đánh số từ 1</strong>) của các số này được gọi là <strong>cách sắp xếp đẹp</strong> nếu với mọi <code>i</code> (<code>1 &lt;= i &lt;= n</code>), một trong các điều kiện sau đúng:</p>

<ul>
	<li><code>perm[i]</code> chia hết cho <code>i</code>.</li>
	<li><code>i</code> chia hết cho <code>perm[i]</code>.</li>
</ul>

<p>Cho số nguyên <code>n</code>, hãy trả về <em><strong>số lượng</strong> <strong>cách sắp xếp đẹp</strong> có thể tạo ra</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 2
<b>Giải thích:</b> 
Cách sắp xếp đẹp thứ nhất là [1,2]:
    - perm[1] = 1 chia hết cho i = 1
    - perm[2] = 2 chia hết cho i = 2
Cách sắp xếp đẹp thứ hai là [2,1]:
    - perm[1] = 2 chia hết cho i = 1
    - i = 2 chia hết cho perm[2] = 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 15</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Backtracking

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các hoán vị mà giá trị $j$ tại vị trí $i$ chia hết cho $i$ hoặc ngược lại. Không thể duyệt toàn bộ $15!$ hoán vị, nhưng mỗi vị trí chỉ có một số ít giá trị phù hợp.
>
> Tính trước các giá trị hợp lệ cho từng vị trí, sau đó backtrack theo vị trí và đánh dấu các số đã dùng. Khi đến $n+1$, ta tìm được một cách sắp xếp hợp lệ. Các danh sách giá trị chia hết giúp giới hạn tìm kiếm vào những lựa chọn khả thi.

<!-- thinking:end -->

Gán một số chưa dùng vào mỗi vị trí nếu điều kiện chia hết được thỏa mãn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countArrangement(self, n: int) -> int:
        def dfs(i):
            nonlocal ans, n
            if i == n + 1:
                ans += 1
                return
            for j in match[i]:
                if not vis[j]:
                    vis[j] = True
                    dfs(i + 1)
                    vis[j] = False

        ans = 0
        vis = [False] * (n + 1)
        match = defaultdict(list)
        for i in range(1, n + 1):
            for j in range(1, n + 1):
                if j % i == 0 or i % j == 0:
                    match[i].append(j)

        dfs(1)
        return ans
```

#### Java

```java
class Solution {
    private int n;
    private int ans;
    private boolean[] vis;
    private Map<Integer, List<Integer>> match;

    public int countArrangement(int n) {
        this.n = n;
        ans = 0;
        vis = new boolean[n + 1];
        match = new HashMap<>();
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (i % j == 0 || j % i == 0) {
                    match.computeIfAbsent(i, k -> new ArrayList<>()).add(j);
                }
            }
        }
        dfs(1);
        return ans;
    }

    private void dfs(int i) {
        if (i == n + 1) {
            ++ans;
            return;
        }
        if (!match.containsKey(i)) {
            return;
        }
        for (int j : match.get(i)) {
            if (!vis[j]) {
                vis[j] = true;
                dfs(i + 1);
                vis[j] = false;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int n;
    int ans;
    vector<bool> vis;
    unordered_map<int, vector<int>> match;

    int countArrangement(int n) {
        this->n = n;
        this->ans = 0;
        vis.resize(n + 1);
        for (int i = 1; i <= n; ++i)
            for (int j = 1; j <= n; ++j)
                if (i % j == 0 || j % i == 0)
                    match[i].push_back(j);
        dfs(1);
        return ans;
    }

    void dfs(int i) {
        if (i == n + 1) {
            ++ans;
            return;
        }
        for (int j : match[i]) {
            if (!vis[j]) {
                vis[j] = true;
                dfs(i + 1);
                vis[j] = false;
            }
        }
    }
};
```

#### Go

```go
func countArrangement(n int) int {
	ans := 0
	match := make(map[int][]int)
	for i := 1; i <= n; i++ {
		for j := 1; j <= n; j++ {
			if i%j == 0 || j%i == 0 {
				match[i] = append(match[i], j)
			}
		}
	}
	vis := make([]bool, n+1)

	var dfs func(i int)
	dfs = func(i int) {
		if i == n+1 {
			ans++
			return
		}
		for _, j := range match[i] {
			if !vis[j] {
				vis[j] = true
				dfs(i + 1)
				vis[j] = false
			}
		}
	}

	dfs(1)
	return ans
}
```

#### TypeScript

```ts
function countArrangement(n: number): number {
    const vis = new Array(n + 1).fill(0);
    const match = Array.from({ length: n + 1 }, () => []);
    for (let i = 1; i <= n; i++) {
        for (let j = 1; j <= n; j++) {
            if (i % j === 0 || j % i === 0) {
                match[i].push(j);
            }
        }
    }

    let res = 0;
    const dfs = (i: number, n: number) => {
        if (i === n + 1) {
            res++;
            return;
        }
        for (const j of match[i]) {
            if (!vis[j]) {
                vis[j] = true;
                dfs(i + 1, n);
                vis[j] = false;
            }
        }
    };
    dfs(1, n);
    return res;
}
```

#### Rust

```rust
impl Solution {
    fn dfs(i: usize, n: usize, mat: &Vec<Vec<usize>>, vis: &mut Vec<bool>, res: &mut i32) {
        if i == n + 1 {
            *res += 1;
            return;
        }
        for &j in mat[i].iter() {
            if !vis[j] {
                vis[j] = true;
                Self::dfs(i + 1, n, mat, vis, res);
                vis[j] = false;
            }
        }
    }

    pub fn count_arrangement(n: i32) -> i32 {
        let n = n as usize;
        let mut vis = vec![false; n + 1];
        let mut mat = vec![Vec::new(); n + 1];
        for i in 1..=n {
            for j in 1..=n {
                if i % j == 0 || j % i == 0 {
                    mat[i].push(j);
                }
            }
        }

        let mut res = 0;
        Self::dfs(1, n, &mat, &mut vis, &mut res);
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động nén trạng thái

<!-- thinking:start -->

> **Tư duy**
>
> Backtracking vẫn duyệt cây hoán vị và có thể gặp lại cùng một tập số chưa dùng tại cùng vị trí. Vì $n \le 15$, tập số đã dùng có thể biểu diễn bằng bitmask với tổng cộng $2^n$ trạng thái.
>
> $f[i]$ là số cách đạt đến trạng thái có mask số đã dùng bằng $i$. Số bit 1 cho biết vị trí kế tiếp; thử từng số chưa dùng $j$ thỏa mãn điều kiện chia hết với vị trí đó. $f[0]=1$, và đáp án là trạng thái có mask đầy đủ. Mỗi tập con chỉ được tính một lần.

<!-- thinking:end -->

$f[i]$ là số cách tạo mask $i$ biểu diễn các số đã chọn.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int countArrangement(int N) {
        int maxn = 1 << N;
        int[] f = new int[maxn];
        f[0] = 1;
        for (int i = 0; i < maxn; ++i) {
            int s = 1;
            for (int j = 0; j < N; ++j) {
                s += (i >> j) & 1;
            }
            for (int j = 1; j <= N; ++j) {
                if (((i >> (j - 1) & 1) == 0) && (s % j == 0 || j % s == 0)) {
                    f[i | (1 << (j - 1))] += f[i];
                }
            }
        }
        return f[maxn - 1];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
