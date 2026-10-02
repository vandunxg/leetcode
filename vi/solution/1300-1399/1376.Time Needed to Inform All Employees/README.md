---
comments: true
difficulty: Medium
rating: 1561
source: Weekly Contest 179 Q3
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
---

<!-- problem:start -->

# [1376. Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees)

[中文文档](/solution/1300-1399/1376.Time%20Needed%20to%20Inform%20All%20Employees/README.md)

## Mô tả

<!-- description:start -->

<p>Công ty có <code>n</code> nhân viên, mỗi người có một ID duy nhất từ <code>0</code> đến <code>n - 1</code>. Người đứng đầu công ty có ID là <code>headID</code>.</p>

<p>Mỗi nhân viên có một quản lý trực tiếp được xác định trong mảng <code>manager</code>, trong đó <code>manager[i]</code> là quản lý trực tiếp của nhân viên thứ <code>i-th</code>, còn <code>manager[headID] = -1</code>. Đảm bảo các quan hệ cấp trên - cấp dưới tạo thành cấu trúc cây.</p>

<p>Người đứng đầu công ty muốn thông báo một tin khẩn cấp cho toàn bộ nhân viên. Người này sẽ thông báo cho các cấp dưới trực tiếp, rồi họ tiếp tục thông báo cho cấp dưới của mình, cho đến khi tất cả nhân viên đều biết tin.</p>

<p>Nhân viên thứ <code>i-th</code> cần <code>informTime[i]</code> phút để thông báo cho tất cả cấp dưới trực tiếp (tức là sau <code>informTime[i]</code> phút, các cấp dưới trực tiếp mới có thể bắt đầu truyền tin).</p>

<p>Trả về <em>số phút</em> cần thiết để thông báo tin khẩn cấp cho toàn bộ nhân viên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, headID = 0, manager = [-1], informTime = [0]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Người đứng đầu công ty là nhân viên duy nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1376.Time%20Needed%20to%20Inform%20All%20Employees/images/graph.png" style="width: 404px; height: 174px;" />
<pre>
<strong>Đầu vào:</strong> n = 6, headID = 2, manager = [2,2,-1,2,2,2], informTime = [0,0,1,0,0,0]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Người đứng đầu công ty có id = 2 là quản lý trực tiếp của tất cả nhân viên và cần 1 phút để thông báo cho họ.
Hình minh họa cấu trúc cây của các nhân viên trong công ty.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= headID &lt; n</code></li>
	<li><code>manager.length == n</code></li>
	<li><code>0 &lt;= manager[i] &lt; n</code></li>
	<li><code>manager[headID] == -1</code></li>
	<li><code>informTime.length == n</code></li>
	<li><code>0 &lt;= informTime[i] &lt;= 1000</code></li>
	<li><code>informTime[i] == 0</code> nếu nhân viên <code>i</code> không có cấp dưới.</li>
	<li><strong>Đảm bảo</strong> có thể thông báo tin cho toàn bộ nhân viên.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Trong cây quản lý, tin bắt đầu từ người đứng đầu; mỗi cạnh tốn thời gian thông báo của nhân viên tương ứng. Vì $n \le 10^5$, không thể tính riêng tổng thời gian trên đường đi đến từng lá. Sau khi tạo danh sách cấp dưới, $dfs(i)$ là thời gian để nhân viên $i$ thông báo cho toàn bộ cây con của mình: $\textit{informTime}[i]$ cộng với thời gian lâu nhất $dfs(j)$ trong số các cấp dưới trực tiếp. Đáp án là $dfs(\textit{headID})$.

<!-- thinking:end -->

Đầu tiên, ta xây dựng danh sách kề $g$ dựa trên mảng $manager$, trong đó $g[i]$ chứa tất cả cấp dưới trực tiếp của nhân viên $i$.

Tiếp theo, ta định nghĩa hàm $dfs(i)$ là thời gian nhân viên $i$ cần để thông báo cho tất cả cấp dưới (cả trực tiếp lẫn gián tiếp). Đáp án là $dfs(headID)$.

Trong hàm $dfs(i)$, ta duyệt tất cả cấp dưới trực tiếp $j$ của $i$. Với mỗi cấp dưới, nhân viên $i$ cần $informTime[i]$ thời gian để thông báo cho họ, sau đó họ cần $dfs(j)$ thời gian để truyền tin tiếp cho cấp dưới của mình. Giá trị trả về của $dfs(i)$ là giá trị lớn nhất của $informTime[i] + dfs(j)$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số nhân viên.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numOfMinutes(
        self, n: int, headID: int, manager: List[int], informTime: List[int]
    ) -> int:
        def dfs(i: int) -> int:
            ans = 0
            for j in g[i]:
                ans = max(ans, dfs(j) + informTime[i])
            return ans

        g = defaultdict(list)
        for i, x in enumerate(manager):
            g[x].append(i)
        return dfs(headID)
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int[] informTime;

    public int numOfMinutes(int n, int headID, int[] manager, int[] informTime) {
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        this.informTime = informTime;
        for (int i = 0; i < n; ++i) {
            if (manager[i] >= 0) {
                g[manager[i]].add(i);
            }
        }
        return dfs(headID);
    }

    private int dfs(int i) {
        int ans = 0;
        for (int j : g[i]) {
            ans = Math.max(ans, dfs(j) + informTime[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numOfMinutes(int n, int headID, vector<int>& manager, vector<int>& informTime) {
        vector<vector<int>> g(n);
        for (int i = 0; i < n; ++i) {
            if (manager[i] >= 0) {
                g[manager[i]].push_back(i);
            }
        }
        function<int(int)> dfs = [&](int i) -> int {
            int ans = 0;
            for (int j : g[i]) {
                ans = max(ans, dfs(j) + informTime[i]);
            }
            return ans;
        };
        return dfs(headID);
    }
};
```

#### Go

```go
func numOfMinutes(n int, headID int, manager []int, informTime []int) int {
	g := make([][]int, n)
	for i, x := range manager {
		if x != -1 {
			g[x] = append(g[x], i)
		}
	}
	var dfs func(int) int
	dfs = func(i int) (ans int) {
		for _, j := range g[i] {
			ans = max(ans, dfs(j)+informTime[i])
		}
		return
	}
	return dfs(headID)
}
```

#### TypeScript

```ts
function numOfMinutes(n: number, headID: number, manager: number[], informTime: number[]): number {
    const g: number[][] = new Array(n).fill(0).map(() => []);
    for (let i = 0; i < n; ++i) {
        if (manager[i] !== -1) {
            g[manager[i]].push(i);
        }
    }
    const dfs = (i: number): number => {
        let ans = 0;
        for (const j of g[i]) {
            ans = Math.max(ans, dfs(j) + informTime[i]);
        }
        return ans;
    };
    return dfs(headID);
}
```

#### C#

```cs
public class Solution {
    private List<int>[] g;
    private int[] informTime;

    public int NumOfMinutes(int n, int headID, int[] manager, int[] informTime) {
        g = new List<int>[n];
        for (int i = 0; i < n; ++i) {
            g[i] = new List<int>();
        }
        this.informTime = informTime;
        for (int i = 0; i < n; ++i) {
            if (manager[i] != -1) {
                g[manager[i]].Add(i);
            }
        }
        return dfs(headID);
    }

    private int dfs(int i) {
        int ans = 0;
        foreach (int j in g[i]) {
            ans = Math.Max(ans, dfs(j) + informTime[i]);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
