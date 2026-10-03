---
comments: true
difficulty: Hard
rating: 2391
source: Weekly Contest 299 Q4
tags:
    - Bit Manipulation
    - Tree
    - Depth-First Search
    - Array
---

<!-- problem:start -->

# [2322. Minimum Score After Removals on a Tree](https://leetcode.com/problems/minimum-score-after-removals-on-a-tree)

[中文文档](/solution/2300-2399/2322.Minimum%20Score%20After%20Removals%20on%20a%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây liên thông vô hướng gồm <code>n</code> nút được đánh số từ <code>0</code> đến <code>n - 1</code> và <code>n - 1</code> cạnh.</p>

<p>Cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>nums</code> có độ dài <code>n</code>, trong đó <code>nums[i]</code> biểu thị giá trị của nút thứ <code>i<sup>th</sup></code>. Bạn cũng được cho một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết có một cạnh nối nút <code>a<sub>i</sub></code> và nút <code>b<sub>i</sub></code> trong cây.</p>

<p>Xóa hai cạnh <strong>khác nhau</strong> của cây để tạo thành ba thành phần liên thông. Với một cặp cạnh bị xóa, thực hiện các bước sau:</p>

<ol>
	<li>Tính XOR của tất cả giá trị các nút trong <strong>từng</strong> thành phần trong ba thành phần.</li>
	<li><strong>Hiệu</strong> giữa giá trị XOR <strong>lớn nhất</strong> và giá trị XOR <strong>nhỏ nhất</strong> là <strong>điểm số</strong> của cặp cạnh đó.</li>
</ol>

<ul>
	<li>Ví dụ, giả sử ba thành phần có các giá trị nút lần lượt là <code>[4,5,7]</code>, <code>[1,9]</code> và <code>[3,3,3]</code>. Ba giá trị XOR là <code>4 ^ 5 ^ 7 = <u><strong>6</strong></u></code>, <code>1 ^ 9 = <u><strong>8</strong></u></code> và <code>3 ^ 3 ^ 3 = <u><strong>3</strong></u></code>. Giá trị XOR lớn nhất là <code>8</code> và nhỏ nhất là <code>3</code>. Khi đó, điểm số là <code>8 - 3 = 5</code>.</li>
</ul>

<p>Trả về <em><strong>điểm số nhỏ nhất</strong> trong mọi cặp cạnh có thể xóa trên cây đã cho</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2322.Minimum%20Score%20After%20Removals%20on%20a%20Tree/images/ex1drawio.png" style="width: 193px; height: 190px;" />
<pre>
<strong>Đầu vào:</strong> nums = [1,5,5,4,11], edges = [[0,1],[1,2],[1,3],[3,4]]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Hình minh họa phía trên cho thấy một cách để thực hiện một cặp xóa cạnh.
- Thành phần <sup>thứ nhất</sup> có các nút [1,3,4] với các giá trị [5,4,11]. Giá trị XOR của nó là 5 ^ 4 ^ 11 = 10.
- Thành phần <sup>thứ hai</sup> có nút [0] với giá trị [1]. Giá trị XOR của nó là 1 = 1.
- Thành phần <sup>thứ ba</sup> có nút [2] với giá trị [5]. Giá trị XOR của nó là 5 = 5.
Điểm số là hiệu giữa giá trị XOR lớn nhất và nhỏ nhất, tức là 10 - 1 = 9.
Có thể chứng minh rằng không có cặp xóa cạnh nào khác cho điểm số nhỏ hơn 9.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2322.Minimum%20Score%20After%20Removals%20on%20a%20Tree/images/ex2drawio.png" style="width: 287px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> nums = [5,5,2,4,4,2], edges = [[0,1],[1,2],[5,2],[4,3],[1,3]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Hình minh họa phía trên cho thấy một cách để thực hiện một cặp xóa cạnh.
- Thành phần <sup>thứ nhất</sup> có các nút [3,4] với các giá trị [4,4]. Giá trị XOR của nó là 4 ^ 4 = 0.
- Thành phần <sup>thứ hai</sup> có các nút [1,0] với các giá trị [5,5]. Giá trị XOR của nó là 5 ^ 5 = 0.
- Thành phần <sup>thứ ba</sup> có các nút [2,5] với các giá trị [2,2]. Giá trị XOR của nó là 2 ^ 2 = 0.
Điểm số là hiệu giữa giá trị XOR lớn nhất và nhỏ nhất, tức là 0 - 0 = 0.
Không thể có điểm số nhỏ hơn 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>3 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>8</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li><code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + Tổng XOR của cây con

<!-- thinking:start -->

> **Tư duy**
>
> Xóa hai cạnh tạo ra ba thành phần; điểm số là khoảng biến thiên của ba giá trị XOR. Vì $n \le 1000$, việc ghép các cạnh và tính lại XOR sẽ tốn nhiều chi phí. Tổng XOR $s$ của toàn bộ cây là cố định, còn XOR của một thành phần chính là XOR của một cây con sau khi chọn gốc.
>
> Trước tiên xóa một cạnh để thu được XOR $s_1$ của phía chứa gốc. DFS bên trong khối đó; XOR $s_2$ của mỗi cây con tương ứng với lần cắt thứ hai. Ba giá trị là $s\oplus s_1$, $s_2$ và $s_1\oplus s_2$. Thử mọi gốc và mọi nút kề sẽ bao phủ tất cả các cặp cạnh không thứ tự.

<!-- thinking:end -->

Ta ký hiệu tổng XOR của cây là $s$, tức là $s = \text{nums}[0] \oplus \text{nums}[1] \oplus \ldots \oplus \text{nums}[n-1]$.

Tiếp theo, ta lần lượt chọn mỗi nút $i$ trong $[0..n)$ làm gốc của cây, đồng thời coi cạnh nối nút gốc với một nút con $j$ là cạnh đầu tiên bị xóa. Khi đó, cây được chia thành hai thành phần liên thông. Ta ký hiệu tổng XOR của thành phần liên thông chứa nút gốc $i$ là $s_1$, sau đó thực hiện DFS trên thành phần liên thông chứa nút gốc $i$ để tính tổng XOR của mỗi cây con, gọi mỗi tổng XOR được tính bởi DFS là $s_2$. Tổng XOR của ba thành phần liên thông là $s \oplus s_1$, $s_2$ và $s_1 \oplus s_2$. Ta cần tính giá trị lớn nhất và nhỏ nhất của ba tổng XOR này, lần lượt là $\textit{mx}$ và $\textit{mn}$. Với mỗi trường hợp được duyệt, điểm số là $\textit{mx} - \textit{mn}$. Ta lấy giá trị nhỏ nhất trong tất cả các trường hợp làm đáp án.

Có thể tính tổng XOR của mỗi cây con bằng DFS. Ta định nghĩa hàm $\text{dfs}(i, fa)$, biểu thị việc bắt đầu DFS từ nút $i$, trong đó $fa$ là cha của nút $i$. Hàm trả về tổng XOR của cây con có gốc là nút $i$.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là số nút của cây.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumScore(self, nums: List[int], edges: List[List[int]]) -> int:
        def dfs(i: int, fa: int) -> int:
            res = nums[i]
            for j in g[i]:
                if j != fa:
                    res ^= dfs(j, i)
            return res

        def dfs2(i: int, fa: int) -> int:
            nonlocal s, s1, ans
            res = nums[i]
            for j in g[i]:
                if j != fa:
                    s2 = dfs2(j, i)
                    res ^= s2
                    mx = max(s ^ s1, s2, s1 ^ s2)
                    mn = min(s ^ s1, s2, s1 ^ s2)
                    ans = min(ans, mx - mn)
            return res

        g = defaultdict(list)
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        s = reduce(lambda x, y: x ^ y, nums)
        n = len(nums)
        ans = inf
        for i in range(n):
            for j in g[i]:
                s1 = dfs(i, j)
                dfs2(i, j)
        return ans
```

#### Java

```java
class Solution {
    private int[] nums;
    private List<Integer>[] g;
    private int ans = Integer.MAX_VALUE;
    private int s;
    private int s1;

    public int minimumScore(int[] nums, int[][] edges) {
        int n = nums.length;
        this.nums = nums;
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        for (int x : nums) {
            s ^= x;
        }
        for (int i = 0; i < n; ++i) {
            for (int j : g[i]) {
                s1 = dfs(i, j);
                dfs2(i, j);
            }
        }
        return ans;
    }

    private int dfs(int i, int fa) {
        int res = nums[i];
        for (int j : g[i]) {
            if (j != fa) {
                res ^= dfs(j, i);
            }
        }
        return res;
    }

    private int dfs2(int i, int fa) {
        int res = nums[i];
        for (int j : g[i]) {
            if (j != fa) {
                int s2 = dfs2(j, i);
                res ^= s2;
                int mx = Math.max(Math.max(s ^ s1, s2), s1 ^ s2);
                int mn = Math.min(Math.min(s ^ s1, s2), s1 ^ s2);
                ans = Math.min(ans, mx - mn);
            }
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumScore(vector<int>& nums, vector<vector<int>>& edges) {
        int n = nums.size();
        vector<int> g[n];
        for (const auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        int s = 0, s1 = 0;
        int ans = INT_MAX;
        for (int x : nums) {
            s ^= x;
        }
        auto dfs = [&](this auto&& dfs, int i, int fa) -> int {
            int res = nums[i];
            for (int j : g[i]) {
                if (j != fa) {
                    res ^= dfs(j, i);
                }
            }
            return res;
        };
        auto dfs2 = [&](this auto&& dfs2, int i, int fa) -> int {
            int res = nums[i];
            for (int j : g[i]) {
                if (j != fa) {
                    int s2 = dfs2(j, i);
                    res ^= s2;
                    int mx = max({s ^ s1, s2, s1 ^ s2});
                    int mn = min({s ^ s1, s2, s1 ^ s2});
                    ans = min(ans, mx - mn);
                }
            }
            return res;
        };
        for (int i = 0; i < n; ++i) {
            for (int j : g[i]) {
                s1 = dfs(i, j);
                dfs2(i, j);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumScore(nums []int, edges [][]int) int {
	n := len(nums)
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	s, s1 := 0, 0
	ans := math.MaxInt32
	for _, x := range nums {
		s ^= x
	}
	var dfs func(i, fa int) int
	dfs = func(i, fa int) int {
		res := nums[i]
		for _, j := range g[i] {
			if j != fa {
				res ^= dfs(j, i)
			}
		}
		return res
	}
	var dfs2 func(i, fa int) int
	dfs2 = func(i, fa int) int {
		res := nums[i]
		for _, j := range g[i] {
			if j != fa {
				s2 := dfs2(j, i)
				res ^= s2
				mx := max(s^s1, s2, s1^s2)
				mn := min(s^s1, s2, s1^s2)
				ans = min(ans, mx-mn)
			}
		}
		return res
	}
	for i := 0; i < n; i++ {
		for _, j := range g[i] {
			s1 = dfs(i, j)
			dfs2(i, j)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minimumScore(nums: number[], edges: number[][]): number {
    const n = nums.length;
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    const s = nums.reduce((a, b) => a ^ b, 0);
    let s1 = 0;
    let ans = Number.MAX_SAFE_INTEGER;
    function dfs(i: number, fa: number): number {
        let res = nums[i];
        for (const j of g[i]) {
            if (j !== fa) {
                res ^= dfs(j, i);
            }
        }
        return res;
    }
    function dfs2(i: number, fa: number): number {
        let res = nums[i];
        for (const j of g[i]) {
            if (j !== fa) {
                const s2 = dfs2(j, i);
                res ^= s2;
                const mx = Math.max(s ^ s1, s2, s1 ^ s2);
                const mn = Math.min(s ^ s1, s2, s1 ^ s2);
                ans = Math.min(ans, mx - mn);
            }
        }
        return res;
    }
    for (let i = 0; i < n; ++i) {
        for (const j of g[i]) {
            s1 = dfs(i, j);
            dfs2(i, j);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_score(nums: Vec<i32>, edges: Vec<Vec<i32>>) -> i32 {
        let n = nums.len();
        let mut g = vec![vec![]; n];
        for e in edges.iter() {
            let a = e[0] as usize;
            let b = e[1] as usize;
            g[a].push(b);
            g[b].push(a);
        }
        let mut s1 = 0;
        let mut ans = i32::MAX;
        let s = nums.iter().fold(0, |acc, &x| acc ^ x);

        fn dfs(i: usize, fa: usize, g: &Vec<Vec<usize>>, nums: &Vec<i32>) -> i32 {
            let mut res = nums[i];
            for &j in &g[i] {
                if j != fa {
                    res ^= dfs(j, i, g, nums);
                }
            }
            res
        }

        fn dfs2(
            i: usize,
            fa: usize,
            g: &Vec<Vec<usize>>,
            nums: &Vec<i32>,
            s: i32,
            s1: i32,
            ans: &mut i32
        ) -> i32 {
            let mut res = nums[i];
            for &j in &g[i] {
                if j != fa {
                    let s2 = dfs2(j, i, g, nums, s, s1, ans);
                    res ^= s2;
                    let mx = (s ^ s1).max(s2).max(s1 ^ s2);
                    let mn = (s ^ s1).min(s2).min(s1 ^ s2);
                    *ans = (*ans).min(mx - mn);
                }
            }
            res
        }

        for i in 0..n {
            for &j in &g[i] {
                s1 = dfs(i, j, &g, &nums);
                dfs2(i, j, &g, &nums, s, s1, &mut ans);
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int MinimumScore(int[] nums, int[][] edges) {
        int n = nums.Length;
        List<int>[] g = new List<int>[n];
        for (int i = 0; i < n; i++) {
            g[i] = new List<int>();
        }
        foreach (var e in edges) {
            int a = e[0], b = e[1];
            g[a].Add(b);
            g[b].Add(a);
        }

        int s = 0;
        foreach (int x in nums) {
            s ^= x;
        }

        int ans = int.MaxValue;
        int s1 = 0;

        int Dfs(int i, int fa) {
            int res = nums[i];
            foreach (int j in g[i]) {
                if (j != fa) {
                    res ^= Dfs(j, i);
                }
            }
            return res;
        }

        int Dfs2(int i, int fa) {
            int res = nums[i];
            foreach (int j in g[i]) {
                if (j != fa) {
                    int s2 = Dfs2(j, i);
                    res ^= s2;
                    int mx = Math.Max(Math.Max(s ^ s1, s2), s1 ^ s2);
                    int mn = Math.Min(Math.Min(s ^ s1, s2), s1 ^ s2);
                    ans = Math.Min(ans, mx - mn);
                }
            }
            return res;
        }

        for (int i = 0; i < n; ++i) {
            foreach (int j in g[i]) {
                s1 = Dfs(i, j);
                Dfs2(i, j);
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
