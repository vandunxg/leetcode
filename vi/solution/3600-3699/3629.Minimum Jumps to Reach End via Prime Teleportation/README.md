---
comments: true
difficulty: Medium
rating: 2139
source: Weekly Contest 460 Q3
tags:
    - Breadth-First Search
    - Array
    - Hash Table
    - Math
    - Number Theory
---

<!-- problem:start -->

# [3629. Minimum Jumps to Reach End via Prime Teleportation](https://leetcode.com/problems/minimum-jumps-to-reach-end-via-prime-teleportation)

[中文文档](/solution/3600-3699/3629.Minimum%20Jumps%20to%20Reach%20End%20via%20Prime%20Teleportation/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Bạn bắt đầu tại chỉ số 0 và mục tiêu là đi đến chỉ số <code>n - 1</code>.</p>

<p>Từ mỗi chỉ số <code>i</code>, bạn có thể thực hiện một trong các thao tác sau:</p>

<ul>
    <li><strong>Bước sang ô kề</strong>: Nhảy đến chỉ số <code>i + 1</code> hoặc <code>i - 1</code>, nếu chỉ số đó nằm trong phạm vi mảng.</li>
    <li><strong>Dịch chuyển qua số nguyên tố</strong>: Nếu <code>nums[i]</code> là <span data-keyword="prime-number">số nguyên tố</span> <code>p</code>, bạn có thể nhảy ngay lập tức đến bất kỳ chỉ số <code>j != i</code> nào sao cho <code>nums[j] % p == 0</code>.</li>
</ul>

<p>Trả về số lần nhảy <strong>ít nhất</strong> cần thực hiện để đi đến chỉ số <code>n - 1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,4,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi bước nhảy tối ưu là:</p>

<ul>
    <li>Bắt đầu tại chỉ số <code>i = 0</code>. Thực hiện một bước sang ô kề đến chỉ số 1.</li>
    <li>Tại chỉ số <code>i = 1</code>, <code>nums[1] = 2</code> là số nguyên tố. Vì vậy, ta dịch chuyển đến chỉ số <code>i = 3</code> vì <code>nums[3] = 6</code> chia hết cho 2.</li>
</ul>

<p>Do đó, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,4,7,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi bước nhảy tối ưu là:</p>

<ul>
    <li>Bắt đầu tại chỉ số <code>i = 0</code>. Thực hiện một bước sang ô kề đến chỉ số <code>i = 1</code>.</li>
    <li>Tại chỉ số <code>i = 1</code>, <code>nums[1] = 3</code> là số nguyên tố. Vì vậy, ta dịch chuyển đến chỉ số <code>i = 4</code> vì <code>nums[4] = 9</code> chia hết cho 3.</li>
</ul>

<p>Do đó, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,6,5,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Vì không thể dịch chuyển, ta di chuyển qua <code>0 &rarr; 1 &rarr; 2 &rarr; 3</code>. Do đó, đáp án là 3.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + BFS

<!-- thinking:start -->

> **Tư duy**
>
> Từ $i$, ta có thể đi đến một chỉ số kề hoặc dịch chuyển đến mọi $j$ có giá trị chia hết cho một thừa số nguyên tố của $\textit{nums}[i]$. Vì đây là đường đi ngắn nhất trên một đồ thị không trọng số, ta sử dụng BFS.
>
> Nếu quét toàn bộ mảng trong mỗi lần dịch chuyển, thời gian sẽ tăng quá lớn. Ta tiền xử lý các thừa số nguyên tố và với mỗi số nguyên tố $p$, lưu danh sách $g[p]$ gồm các chỉ số có giá trị là bội của $p$.
>
> BFS mở rộng từ $i\pm 1$ và $g[\textit{nums}[i]]$, sau đó xóa danh sách này để mỗi số nguyên tố chỉ được sử dụng một lần. Lớp BFS đầu tiên chạm đến $n-1$ chính là đáp án.

<!-- thinking:end -->

Trước hết, ta tiền xử lý danh sách các thừa số nguyên tố cho mọi số đến $10^6$ và lưu chúng trong $\textit{factors}$.

Sau đó, ta xây dựng một đồ thị $g$. Với mỗi chỉ số $i$ và mỗi $p \in \textit{factors}[nums[i]]$, ta thêm $i$ vào $g[p]$. Nhờ đó, ta có danh sách các chỉ số có thể đến được bằng cách dịch chuyển qua mỗi số nguyên tố $p$.

Tiếp theo, ta sử dụng tìm kiếm theo chiều rộng để tìm số lần nhảy ít nhất. Ta duy trì một queue $q$ để lưu các chỉ số hiện có thể đi đến, ban đầu $q$ chỉ chứa chỉ số $0$. Mỗi khi lấy một chỉ số $i$ khỏi $q$, nếu $i$ là chỉ số đích $n - 1$, ta trả về số lần nhảy hiện tại. Nếu không, ta thêm tất cả các chỉ số trong $g[nums[i]]$ vào $q$ và xóa chúng khỏi $g[nums[i]]$ để tránh thăm lại. Đồng thời, ta cũng thêm các chỉ số kề $i + 1$ và $i - 1$ vào $q$ nếu chúng nằm trong phạm vi mảng.

Độ phức tạp thời gian là $O(n \log M)$ và độ phức tạp không gian là $O(n \log M)$, trong đó $n$ là độ dài của mảng và $M$ là giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
mx = 10**6 + 1
factors = [[] for _ in range(mx)]
for i in range(2, mx):
    if not factors[i]:
        for j in range(i, mx, i):
            factors[j].append(i)


class Solution:
    def minJumps(self, nums: List[int]) -> int:
        n = len(nums)
        g = defaultdict(list)
        for i, x in enumerate(nums):
            for p in factors[x]:
                g[p].append(i)
        ans = 0
        vis = [False] * n
        vis[0] = True
        q = [0]
        while 1:
            nq = []
            for i in q:
                if i == n - 1:
                    return ans
                idx = g[nums[i]]
                idx.append(i + 1)
                if i:
                    idx.append(i - 1)
                for j in idx:
                    if not vis[j]:
                        vis[j] = True
                        nq.append(j)
                idx.clear()
            q = nq
            ans += 1
```

#### Java

```java
class Solution {
    private static final int mx = 1000001;
    private static final List<Integer>[] factors = new List[mx];

    static {
        for (int i = 0; i < mx; i++) {
            factors[i] = new ArrayList<>();
        }
        for (int i = 2; i < mx; i++) {
            if (factors[i].isEmpty()) {
                for (int j = i; j < mx; j += i) {
                    factors[j].add(i);
                }
            }
        }
    }

    public int minJumps(int[] nums) {
        int n = nums.length;
        Map<Integer, List<Integer>> g = new HashMap<>();
        for (int i = 0; i < n; i++) {
            int x = nums[i];
            for (int p : factors[x]) {
                g.computeIfAbsent(p, k -> new ArrayList<>()).add(i);
            }
        }
        int ans = 0;
        boolean[] vis = new boolean[n];
        vis[0] = true;
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(0);
        while (true) {
            Deque<Integer> nq = new ArrayDeque<>();
            for (int i : q) {
                if (i == n - 1) {
                    return ans;
                }
                List<Integer> idx = g.getOrDefault(nums[i], new ArrayList<>());
                idx.add(i + 1);
                if (i > 0) {
                    idx.add(i - 1);
                }
                for (int j : idx) {
                    if (!vis[j]) {
                        vis[j] = true;
                        nq.offer(j);
                    }
                }
                idx.clear();
            }
            q = nq;
            ans++;
        }
    }
}
```

#### C++

```cpp
const int mx = 1e6 + 1;
vector<int> factors[mx];

int init = [] {
    for (int i = 2; i < mx; ++i) {
        if (factors[i].empty()) {
            for (int j = i; j < mx; j += i) {
                factors[j].push_back(i);
            }
        }
    }
    return 0;
}();

class Solution {
public:
    int minJumps(vector<int>& nums) {
        int n = nums.size();
        unordered_map<int, vector<int>> g;
        for (int i = 0; i < n; i++) {
            int x = nums[i];
            for (int p : factors[x]) {
                g[p].push_back(i);
            }
        }
        int ans = 0;
        vector<bool> vis(n, false);
        vis[0] = true;
        queue<int> q;
        q.push(0);
        while (true) {
            queue<int> nq;
            while (!q.empty()) {
                int i = q.front();
                q.pop();
                if (i == n - 1) {
                    return ans;
                }
                vector<int> idx = g[nums[i]];
                idx.push_back(i + 1);
                if (i > 0) {
                    idx.push_back(i - 1);
                }
                for (int j : idx) {
                    if (!vis[j]) {
                        vis[j] = true;
                        nq.push(j);
                    }
                }
                g[nums[i]].clear();
            }
            q = nq;
            ans++;
        }
    }
};
```

#### Go

```go
const mx = 1000001

var factors [mx][]int

func init() {
    for i := 2; i < mx; i++ {
        if len(factors[i]) == 0 {
            for j := i; j < mx; j += i {
                factors[j] = append(factors[j], i)
            }
        }
    }
}

func minJumps(nums []int) int {
    n := len(nums)
    g := make(map[int][]int)
    for i, x := range nums {
        for _, p := range factors[x] {
            g[p] = append(g[p], i)
        }
    }
    ans := 0
    vis := make([]bool, n)
    vis[0] = true
    q := []int{0}
    for {
        nq := []int{}
        for _, i := range q {
            if i == n-1 {
                return ans
            }
            idx := append([]int{}, g[nums[i]]...)
            idx = append(idx, i+1)
            if i > 0 {
                idx = append(idx, i-1)
            }
            for _, j := range idx {
                if !vis[j] {
                    vis[j] = true
                    nq = append(nq, j)
                }
            }
            g[nums[i]] = []int{}
        }
        q = nq
        ans++
    }
}
```

#### TypeScript

```ts
const mx = 1000001;
const factors: number[][] = Array(mx);

for (let i = 0; i < mx; i++) {
    factors[i] = [];
}
for (let i = 2; i < mx; i++) {
    if (factors[i].length === 0) {
        for (let j = i; j < mx; j += i) {
            factors[j].push(i);
        }
    }
}

function minJumps(nums: number[]): number {
    const n = nums.length;
    const g = new Map<number, number[]>();
    for (let i = 0; i < n; i++) {
        const x = nums[i];
        for (const p of factors[x]) {
            if (!g.has(p)) {
                g.set(p, []);
            }
            g.get(p)!.push(i);
        }
    }
    let ans = 0;
    const vis = new Array(n).fill(false);
    vis[0] = true;
    let q: number[] = [0];
    while (true) {
        const nq: number[] = [];
        for (const i of q) {
            if (i === n - 1) {
                return ans;
            }
            const idx = [...(g.get(nums[i]) || [])];
            idx.push(i + 1);
            if (i > 0) {
                idx.push(i - 1);
            }
            for (const j of idx) {
                if (!vis[j]) {
                    vis[j] = true;
                    nq.push(j);
                }
            }
            if (g.has(nums[i])) {
                g.get(nums[i])!.length = 0;
            }
        }
        q = nq;
        ans++;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
