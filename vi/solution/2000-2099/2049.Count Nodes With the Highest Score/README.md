---
comments: true
difficulty: Medium
rating: 1911
source: Weekly Contest 264 Q3
tags:
    - Tree
    - Depth-First Search
    - Array
    - Binary Tree
    - Tree DP
---

<!-- problem:start -->

# [2049. Count Nodes With the Highest Score](https://leetcode.com/problems/count-nodes-with-the-highest-score)

[中文文档](/solution/2000-2099/2049.Count%20Nodes%20With%20the%20Highest%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây <strong>nhị phân</strong> gốc tại <code>0</code> gồm <code>n</code> node. Các node được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>parents</code> biểu diễn cây, trong đó <code>parents[i]</code> là node cha của node <code>i</code>. Vì node <code>0</code> là gốc nên <code>parents[0] == -1</code>.</p>

<p>Mỗi node có một <strong>score</strong>. Để tính score của một node, hãy xét trường hợp node đó và các cạnh nối với nó bị <strong>xóa</strong>. Cây sẽ trở thành một hoặc nhiều cây con <strong>không rỗng</strong>. <strong>Kích thước</strong> của một cây con là số node trong cây con đó. <strong>Score</strong> của node là <strong>tích kích thước</strong> của tất cả các cây con đó.</p>

<p>Trả về <em><strong>số lượng</strong> node có <strong>score cao nhất</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="example-1" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2049.Count%20Nodes%20With%20the%20Highest%20Score/images/example-1.png" style="width: 604px; height: 266px;" />
<pre>
<strong>Đầu vào:</strong> parents = [-1,2,0,2,0]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
- Score của node 0 là: 3 * 1 = 3
- Score của node 1 là: 4 = 4
- Score của node 2 là: 1 * 1 * 2 = 2
- Score của node 3 là: 4 = 4
- Score của node 4 là: 4 = 4
Score cao nhất là 4, và có ba node (node 1, node 3 và node 4) đạt score cao nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="example-2" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2049.Count%20Nodes%20With%20the%20Highest%20Score/images/example-2.png" style="width: 95px; height: 143px;" />
<pre>
<strong>Đầu vào:</strong> parents = [-1,2,0]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
- Score của node 0 là: 2 = 2
- Score của node 1 là: 2 = 2
- Score của node 2 là: 1 * 1 = 1
Score cao nhất là 2, và có hai node (node 0 và node 1) đạt score cao nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == parents.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>parents[0] == -1</code></li>
	<li><code>0 &lt;= parents[i] &lt;= n - 1</code> với <code>i != 0</code></li>
	<li><code>parents</code> biểu diễn một cây nhị phân hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Khi xóa một node, score là tích kích thước của các thành phần liên thông. Với $n \le 10^5$, ta cần tính mọi score trong một lần duyệt. Kích thước cây con xuất hiện khi DFS quay lui; phần bù là $n-cnt$.
>
> Xây dựng danh sách các node con và DFS từ node gốc: nhân kích thước các cây con, sau đó nhân thêm $n-cnt$ nếu giá trị này khác không. Theo dõi score lớn nhất và số node đạt score đó.

<!-- thinking:end -->

Trước hết, ta xây dựng một graph $g$ dựa trên mảng parent đã cho `parents`, trong đó $g[i]$ chứa tất cả node con của node $i$. Ta định nghĩa biến $ans$ để biểu diễn số lượng node có score cao nhất, và biến $mx$ để biểu diễn score cao nhất.

Sau đó, ta thiết kế hàm `dfs(i, fa)` để tính score của node $i$ và trả về số node trong cây con có gốc là node $i$.

Quy trình tính toán của hàm `dfs(i, fa)` như sau:

Trước tiên, ta khởi tạo biến $cnt = 1$, biểu diễn số node trong cây con có gốc là node $i$; và biến $score = 1$, biểu diễn score ban đầu của node $i$.

Tiếp theo, ta duyệt qua tất cả node con $j$ của node $i$. Nếu $j$ không phải là node cha $fa$ của node $i$, ta gọi đệ quy `dfs(j, i)`, nhân giá trị trả về vào $score$, đồng thời cộng giá trị trả về vào $cnt$.

Sau khi duyệt qua các node con, nếu $n - cnt > 0$, ta nhân $n - cnt$ vào $score$.

Sau đó, ta kiểm tra xem $mx$ có nhỏ hơn $score$ hay không. Nếu nhỏ hơn, ta cập nhật $mx$ thành $score$, đồng thời cập nhật $ans$ thành $1$; nếu bằng nhau, ta cập nhật $ans$ thành $ans + 1$.

Cuối cùng, ta trả về $cnt$.

Kết thúc, ta gọi `dfs(0, -1)` và trả về $ans$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countHighestScoreNodes(self, parents: List[int]) -> int:
        def dfs(i: int, fa: int):
            cnt = score = 1
            for j in g[i]:
                if j != fa:
                    t = dfs(j, i)
                    score *= t
                    cnt += t
            if n - cnt:
                score *= n - cnt
            nonlocal ans, mx
            if mx < score:
                mx = score
                ans = 1
            elif mx == score:
                ans += 1
            return cnt

        n = len(parents)
        g = [[] for _ in range(n)]
        for i in range(1, n):
            g[parents[i]].append(i)
        ans = mx = 0
        dfs(0, -1)
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int ans;
    private long mx;
    private int n;

    public int countHighestScoreNodes(int[] parents) {
        n = parents.length;
        g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (int i = 1; i < n; ++i) {
            g[parents[i]].add(i);
        }
        dfs(0, -1);
        return ans;
    }

    private int dfs(int i, int fa) {
        int cnt = 1;
        long score = 1;
        for (int j : g[i]) {
            if (j != fa) {
                int t = dfs(j, i);
                cnt += t;
                score *= t;
            }
        }
        if (n - cnt > 0) {
            score *= n - cnt;
        }
        if (mx < score) {
            mx = score;
            ans = 1;
        } else if (mx == score) {
            ++ans;
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countHighestScoreNodes(vector<int>& parents) {
        int n = parents.size();
        vector<int> g[n];
        for (int i = 1; i < n; ++i) {
            g[parents[i]].push_back(i);
        }
        int ans = 0;
        long long mx = 0;
        function<int(int, int)> dfs = [&](int i, int fa) {
            long long score = 1;
            int cnt = 1;
            for (int j : g[i]) {
                if (j != fa) {
                    int t = dfs(j, i);
                    cnt += t;
                    score *= t;
                }
            }
            if (n - cnt) {
                score *= n - cnt;
            }
            if (mx < score) {
                mx = score;
                ans = 1;
            } else if (mx == score) {
                ++ans;
            }
            return cnt;
        };
        dfs(0, -1);
        return ans;
    }
};
```

#### Go

```go
func countHighestScoreNodes(parents []int) (ans int) {
	n := len(parents)
	g := make([][]int, n)
	for i := 1; i < n; i++ {
		g[parents[i]] = append(g[parents[i]], i)
	}
	mx := 0
	var dfs func(i, fa int) int
	dfs = func(i, fa int) int {
		cnt, score := 1, 1
		for _, j := range g[i] {
			if j != fa {
				t := dfs(j, i)
				cnt += t
				score *= t
			}
		}
		if n-cnt > 0 {
			score *= n - cnt
		}
		if mx < score {
			mx = score
			ans = 1
		} else if mx == score {
			ans++
		}
		return cnt
	}
	dfs(0, -1)
	return
}
```

#### TypeScript

```ts
function countHighestScoreNodes(parents: number[]): number {
    const n = parents.length;
    const g: number[][] = Array.from({ length: n }, () => []);
    for (let i = 1; i < n; i++) {
        g[parents[i]].push(i);
    }
    let [ans, mx] = [0, 0];
    const dfs = (i: number, fa: number): number => {
        let [cnt, score] = [1, 1];
        for (const j of g[i]) {
            if (j !== fa) {
                const t = dfs(j, i);
                cnt += t;
                score *= t;
            }
        }
        if (n - cnt) {
            score *= n - cnt;
        }
        if (mx < score) {
            mx = score;
            ans = 1;
        } else if (mx === score) {
            ans++;
        }
        return cnt;
    };
    dfs(0, -1);
    return ans;
}
```

#### C#

```cs
public class Solution {
    private List<int>[] g;
    private int ans;
    private long mx;
    private int n;

    public int CountHighestScoreNodes(int[] parents) {
        n = parents.Length;
        g = new List<int>[n];
        for (int i = 0; i < n; ++i) {
            g[i] = new List<int>();
        }
        for (int i = 1; i < n; ++i) {
            g[parents[i]].Add(i);
        }

        Dfs(0, -1);
        return ans;
    }

    private int Dfs(int i, int fa) {
        int cnt = 1;
        long score = 1;

        foreach (int j in g[i]) {
            if (j != fa) {
                int t = Dfs(j, i);
                cnt += t;
                score *= t;
            }
        }

        if (n - cnt > 0) {
            score *= n - cnt;
        }

        if (mx < score) {
            mx = score;
            ans = 1;
        } else if (mx == score) {
            ++ans;
        }

        return cnt;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
