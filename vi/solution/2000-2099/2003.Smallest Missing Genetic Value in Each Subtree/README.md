---
comments: true
difficulty: Hard
rating: 2415
source: Weekly Contest 258 Q4
tags:
    - Tree
    - Depth-First Search
    - Union Find
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2003. Smallest Missing Genetic Value in Each Subtree](https://leetcode.com/problems/smallest-missing-genetic-value-in-each-subtree)

[中文文档](/solution/2000-2099/2003.Smallest%20Missing%20Genetic%20Value%20in%20Each%20Subtree/README.md)

## Mô tả

<!-- description:start -->

<p>Có một <strong>cây gia phả</strong> có gốc tại <code>0</code>, gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cung cấp một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>parents</code>, trong đó <code>parents[i]</code> là node cha của node <code>i</code>. Vì node <code>0</code> là <strong>root</strong>, nên <code>parents[0] == -1</code>.</p>

<p>Có <code>10<sup>5</sup></code> giá trị gene, mỗi giá trị được biểu diễn bằng một số nguyên trong khoảng <strong>đóng</strong> <code>[1, 10<sup>5</sup>]</code>. Bạn được cung cấp một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>nums</code>, trong đó <code>nums[i]</code> là một giá trị gene <strong>khác nhau </strong> của node <code>i</code>.</p>

<p>Trả về <em>một mảng </em><code>ans</code><em> có độ dài </em><code>n</code><em>, trong đó </em><code>ans[i]</code><em> là </em><em>giá trị gene <strong>nhỏ nhất</strong> bị <strong>thiếu</strong> trong cây con có gốc tại node </em><code>i</code>.</p>

<p><strong>Cây con</strong> có gốc tại một node <code>x</code> chứa node <code>x</code> và tất cả các node <strong>hậu duệ</strong> của nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2003.Smallest%20Missing%20Genetic%20Value%20in%20Each%20Subtree/images/case-1.png" style="width: 204px; height: 167px;" />
<pre>
<strong>Đầu vào:</strong> parents = [-1,0,0,2], nums = [1,2,3,4]
<strong>Đầu ra:</strong> [5,1,1,1]
<strong>Giải thích:</strong> Đáp án cho mỗi cây con được tính như sau:
- 0: Cây con chứa các node [0,1,2,3] với các giá trị [1,2,3,4]. 5 là giá trị nhỏ nhất bị thiếu.
- 1: Cây con chỉ chứa node 1 với giá trị 2. 1 là giá trị nhỏ nhất bị thiếu.
- 2: Cây con chứa các node [2,3] với các giá trị [3,4]. 1 là giá trị nhỏ nhất bị thiếu.
- 3: Cây con chỉ chứa node 3 với giá trị 4. 1 là giá trị nhỏ nhất bị thiếu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2003.Smallest%20Missing%20Genetic%20Value%20in%20Each%20Subtree/images/case-2.png" style="width: 247px; height: 168px;" />
<pre>
<strong>Đầu vào:</strong> parents = [-1,0,1,0,3,3], nums = [5,4,6,2,1,3]
<strong>Đầu ra:</strong> [7,1,1,4,2,1]
<strong>Giải thích:</strong> Đáp án cho mỗi cây con được tính như sau:
- 0: Cây con chứa các node [0,1,2,3,4,5] với các giá trị [5,4,6,2,1,3]. 7 là giá trị nhỏ nhất bị thiếu.
- 1: Cây con chứa các node [1,2] với các giá trị [4,6]. 1 là giá trị nhỏ nhất bị thiếu.
- 2: Cây con chỉ chứa node 2 với giá trị 6. 1 là giá trị nhỏ nhất bị thiếu.
- 3: Cây con chứa các node [3,4,5] với các giá trị [2,1,3]. 4 là giá trị nhỏ nhất bị thiếu.
- 4: Cây con chỉ chứa node 4 với giá trị 1. 2 là giá trị nhỏ nhất bị thiếu.
- 5: Cây con chỉ chứa node 5 với giá trị 3. 1 là giá trị nhỏ nhất bị thiếu.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> parents = [-1,2,3,0,2,4,1], nums = [2,3,4,5,6,7,8]
<strong>Đầu ra:</strong> [1,1,1,1,1,1,1]
<strong>Giải thích:</strong> Giá trị 1 bị thiếu trong tất cả các cây con.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == parents.length == nums.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= parents[i] &lt;= n - 1</code> với <code>i != 0</code></li>
	<li><code>parents[0] == -1</code></li>
	<li><code>parents</code> biểu diễn một cây hợp lệ.</li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li>Mỗi <code>nums[i]</code> là duy nhất.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Với $n \le 10^5$, nếu duyệt riêng từng cây con thì các node sẽ bị duyệt lại quá nhiều lần. Các giá trị gene là duy nhất, nên mex $> 1$ chỉ có thể xuất hiện trên đường đi từ node có gene $1$ đến root; mọi node khác đều có đáp án là $1$.
>
> Khi đi ngược theo đường đi này, tập gene chỉ mở rộng (node cha bổ sung các cây con của những node anh em). Các gene đã đánh dấu không cần được duyệt lại.
>
> DFS từ $idx$ về phía root sẽ điền vào $has$; một con trỏ $i$ tăng dần tìm giá trị nhỏ nhất bị thiếu và ghi kết quả vào $ans[idx]$.

<!-- thinking:end -->

Ta nhận thấy mỗi node có một giá trị gene duy nhất, nên chỉ cần tìm node $idx$ có giá trị gene bằng $1$; mọi node ngoại trừ các node trên đường đi từ node $idx$ đến node root $0$ đều có đáp án bằng $1$.

Do đó, ta khởi tạo mảng đáp án $ans$ bằng $[1,1,...,1]$ và tập trung tìm đáp án cho từng node trên đường đi từ node $idx$ đến node root $0$.

Ta bắt đầu từ node $idx$ và sử dụng tìm kiếm theo chiều sâu để đánh dấu các giá trị gene xuất hiện trong cây con có gốc tại $idx$, rồi lưu chúng vào mảng $has$. Trong quá trình tìm kiếm, ta dùng mảng $vis$ để đánh dấu các node đã duyệt nhằm tránh duyệt lặp.

Tiếp theo, ta bắt đầu từ $i=2$ và liên tục tìm giá trị gene đầu tiên chưa xuất hiện; đó là đáp án cho node $idx$. $i$ luôn tăng, vì các giá trị gene là duy nhất, nên ta luôn có thể tìm được một giá trị gene chưa xuất hiện trong $[1,..n+1]$.

Sau đó, ta cập nhật đáp án cho node $idx$, tức là $ans[idx]=i$, rồi cập nhật $idx$ thành node cha của nó để tiếp tục quá trình trên cho đến khi $idx=-1$, nghĩa là đã đi đến node root $0$.

Cuối cùng, ta trả về mảng đáp án $ans$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestMissingValueSubtree(
        self, parents: List[int], nums: List[int]
    ) -> List[int]:
        def dfs(i: int):
            if vis[i]:
                return
            vis[i] = True
            if nums[i] < len(has):
                has[nums[i]] = True
            for j in g[i]:
                dfs(j)

        n = len(nums)
        ans = [1] * n
        g = [[] for _ in range(n)]
        idx = -1
        for i, p in enumerate(parents):
            if i:
                g[p].append(i)
            if nums[i] == 1:
                idx = i
        if idx == -1:
            return ans
        vis = [False] * n
        has = [False] * (n + 2)
        i = 2
        while idx != -1:
            dfs(idx)
            while has[i]:
                i += 1
            ans[idx] = i
            idx = parents[idx]
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private boolean[] vis;
    private boolean[] has;
    private int[] nums;

    public int[] smallestMissingValueSubtree(int[] parents, int[] nums) {
        int n = nums.length;
        this.nums = nums;
        g = new List[n];
        vis = new boolean[n];
        has = new boolean[n + 2];
        Arrays.setAll(g, i -> new ArrayList<>());
        int idx = -1;
        for (int i = 0; i < n; ++i) {
            if (i > 0) {
                g[parents[i]].add(i);
            }
            if (nums[i] == 1) {
                idx = i;
            }
        }
        int[] ans = new int[n];
        Arrays.fill(ans, 1);
        if (idx == -1) {
            return ans;
        }
        for (int i = 2; idx != -1; idx = parents[idx]) {
            dfs(idx);
            while (has[i]) {
                ++i;
            }
            ans[idx] = i;
        }
        return ans;
    }

    private void dfs(int i) {
        if (vis[i]) {
            return;
        }
        vis[i] = true;
        if (nums[i] < has.length) {
            has[nums[i]] = true;
        }
        for (int j : g[i]) {
            dfs(j);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> smallestMissingValueSubtree(vector<int>& parents, vector<int>& nums) {
        int n = nums.size();
        vector<int> g[n];
        bool vis[n];
        bool has[n + 2];
        memset(vis, false, sizeof(vis));
        memset(has, false, sizeof(has));
        int idx = -1;
        for (int i = 0; i < n; ++i) {
            if (i) {
                g[parents[i]].push_back(i);
            }
            if (nums[i] == 1) {
                idx = i;
            }
        }
        vector<int> ans(n, 1);
        if (idx == -1) {
            return ans;
        }
        function<void(int)> dfs = [&](int i) {
            if (vis[i]) {
                return;
            }
            vis[i] = true;
            if (nums[i] < n + 2) {
                has[nums[i]] = true;
            }
            for (int j : g[i]) {
                dfs(j);
            }
        };
        for (int i = 2; ~idx; idx = parents[idx]) {
            dfs(idx);
            while (has[i]) {
                ++i;
            }
            ans[idx] = i;
        }
        return ans;
    }
};
```

#### Go

```go
func smallestMissingValueSubtree(parents []int, nums []int) []int {
	n := len(nums)
	g := make([][]int, n)
	vis := make([]bool, n)
	has := make([]bool, n+2)
	idx := -1
	ans := make([]int, n)
	for i, p := range parents {
		if i > 0 {
			g[p] = append(g[p], i)
		}
		if nums[i] == 1 {
			idx = i
		}
		ans[i] = 1
	}
	if idx < 0 {
		return ans
	}
	var dfs func(int)
	dfs = func(i int) {
		if vis[i] {
			return
		}
		vis[i] = true
		if nums[i] < len(has) {
			has[nums[i]] = true
		}
		for _, j := range g[i] {
			dfs(j)
		}
	}
	for i := 2; idx != -1; idx = parents[idx] {
		dfs(idx)
		for has[i] {
			i++
		}
		ans[idx] = i
	}
	return ans
}
```

#### TypeScript

```ts
function smallestMissingValueSubtree(parents: number[], nums: number[]): number[] {
    const n = nums.length;
    const g: number[][] = Array.from({ length: n }, () => []);
    const vis: boolean[] = Array(n).fill(false);
    const has: boolean[] = Array(n + 2).fill(false);
    const ans: number[] = Array(n).fill(1);
    let idx = -1;
    for (let i = 0; i < n; ++i) {
        if (i) {
            g[parents[i]].push(i);
        }
        if (nums[i] === 1) {
            idx = i;
        }
    }
    if (idx === -1) {
        return ans;
    }
    const dfs = (i: number): void => {
        if (vis[i]) {
            return;
        }
        vis[i] = true;
        if (nums[i] < has.length) {
            has[nums[i]] = true;
        }
        for (const j of g[i]) {
            dfs(j);
        }
    };
    for (let i = 2; ~idx; idx = parents[idx]) {
        dfs(idx);
        while (has[i]) {
            ++i;
        }
        ans[idx] = i;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn smallest_missing_value_subtree(parents: Vec<i32>, nums: Vec<i32>) -> Vec<i32> {
        fn dfs(
            i: usize,
            vis: &mut Vec<bool>,
            has: &mut Vec<bool>,
            g: &Vec<Vec<usize>>,
            nums: &Vec<i32>,
        ) {
            if vis[i] {
                return;
            }
            vis[i] = true;
            if nums[i] < (has.len() as i32) {
                has[nums[i] as usize] = true;
            }
            for &j in &g[i] {
                dfs(j, vis, has, g, nums);
            }
        }

        let n = nums.len();
        let mut ans = vec![1; n];
        let mut g: Vec<Vec<usize>> = vec![vec![]; n];
        let mut idx = -1;
        for (i, &p) in parents.iter().enumerate() {
            if i > 0 {
                g[p as usize].push(i);
            }
            if nums[i] == 1 {
                idx = i as i32;
            }
        }
        if idx == -1 {
            return ans;
        }
        let mut vis = vec![false; n];
        let mut has = vec![false; (n + 2) as usize];
        let mut i = 2;
        let mut idx_mut = idx;
        while idx_mut != -1 {
            dfs(idx_mut as usize, &mut vis, &mut has, &g, &nums);
            while has[i] {
                i += 1;
            }
            ans[idx_mut as usize] = i as i32;
            idx_mut = parents[idx_mut as usize];
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
