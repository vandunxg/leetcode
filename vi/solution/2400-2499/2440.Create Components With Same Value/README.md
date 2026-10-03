---
comments: true
difficulty: Hard
rating: 2460
source: Biweekly Contest 89 Q4
tags:
    - Tree
    - Depth-First Search
    - Array
    - Math
    - Enumeration
---

<!-- problem:start -->

# [2440. Create Components With Same Value](https://leetcode.com/problems/create-components-with-same-value)

[中文文档](/solution/2400-2499/2440.Create%20Components%20With%20Same%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây vô hướng gồm <code>n</code> nút được đánh số từ <code>0</code> đến <code>n - 1</code>.</p>

<p>Cho mảng số nguyên <strong>0-indexed</strong> <code><font face="monospace">nums</font></code> có độ dài <code>n</code>, trong đó <code>nums[i]</code> biểu diễn giá trị của nút thứ <code>i<sup>th</sup></code>. Bạn cũng được cho một mảng số nguyên 2D <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu diễn một cạnh nối nút <code>a<sub>i</sub></code> và nút <code>b<sub>i</sub></code> trong cây.</p>

<p>Bạn có thể <strong>xóa</strong> một số cạnh để chia cây thành nhiều thành phần liên thông. <strong>Giá trị</strong> của một thành phần là tổng của <strong>tất cả</strong> <code>nums[i]</code> với các nút <code>i</code> thuộc thành phần đó.</p>

<p>Trả về <em>số lượng <strong>lớn nhất</strong> các cạnh có thể xóa sao cho mọi thành phần liên thông trong cây có cùng giá trị.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2440.Create%20Components%20With%20Same%20Value/images/diagramdrawio.png" style="width: 441px; height: 351px;" />
<pre>
<strong>Đầu vào:</strong> nums = [6,2,2,2,6], edges = [[0,1],[1,2],[1,3],[3,4]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hình trên minh họa cách xóa các cạnh [0,1] và [3,4]. Các thành phần tạo thành là các nút [0], [1,2,3] và [4]. Tổng các giá trị trong mỗi thành phần bằng 6. Có thể chứng minh rằng không thể xóa nhiều cạnh hơn, nên đáp án là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2], edges = []
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có cạnh nào để xóa.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>nums.length == n</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 50</code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= edges[i][0], edges[i][1] &lt;= n - 1</code></li>
	<li><code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê các thành phần liên thông

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi xóa các cạnh, mọi thành phần phải có cùng tổng, nên số thành phần $k$ phải là ước của tổng $s$ và giá trị mục tiêu $t=s/k$ không nhỏ hơn giá trị lớn nhất của một nút. Ta liệt kê các giá trị $k$ khả thi từ lớn đến nhỏ; $n\le 2\times 10^4$.
>
> Dùng DFS để tính tổng của một subtree: nếu tổng đúng bằng $t$ thì cắt subtree đó và trả về $0$; nếu lớn hơn $t$ thì thất bại. Nếu root trả về $0$, ta có thể xóa $k-1$ cạnh.

<!-- thinking:end -->

Giả sử số thành phần liên thông là $k$, khi đó số cạnh cần xóa là $k-1$, và giá trị của mỗi thành phần là $\frac{s}{k}$, trong đó $s$ là tổng giá trị của tất cả các nút trong $nums$.

Ta liệt kê $k$ từ lớn đến nhỏ. Nếu tồn tại một $k$ sao cho $\frac{s}{k}$ là số nguyên và giá trị của mọi thành phần liên thông tạo thành đều bằng nhau, ta trả về trực tiếp $k-1$. Giá trị ban đầu của $k$ là $\min(n, \frac{s}{mx})$, trong đó $mx$ là giá trị lớn nhất trong $nums$.

Điểm mấu chốt là kiểm tra xem với một $\frac{s}{k}$ cho trước, ta có thể chia một số subtree sao cho giá trị của mỗi subtree bằng $\frac{s}{k}$ hay không.

Ở đây, ta dùng hàm `dfs` để kiểm tra. Ta đệ quy duyệt từ trên xuống dưới để tính giá trị của mỗi subtree. Nếu tổng giá trị của subtree đúng bằng $\frac{s}{k}$, điều đó có nghĩa là việc chia tách tại thời điểm này thành công. Ta gán giá trị bằng $0$ và trả về cho cấp trên, cho biết subtree này có thể được ngắt khỏi nút cha. Nếu tổng giá trị của subtree lớn hơn $\frac{s}{k}$, việc chia tách thất bại. Ta trả về $-1$, cho biết không thể chia tách được.

Độ phức tạp thời gian là $O(n \times \sqrt{s})$, trong đó $n$ và $s$ lần lượt là độ dài của $nums$ và tổng giá trị của tất cả các nút trong $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def componentValue(self, nums: List[int], edges: List[List[int]]) -> int:
        def dfs(i, fa):
            x = nums[i]
            for j in g[i]:
                if j != fa:
                    y = dfs(j, i)
                    if y == -1:
                        return -1
                    x += y
            if x > t:
                return -1
            return x if x < t else 0

        n = len(nums)
        g = defaultdict(list)
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        s = sum(nums)
        mx = max(nums)
        for k in range(min(n, s // mx), 1, -1):
            if s % k == 0:
                t = s // k
                if dfs(0, -1) == 0:
                    return k - 1
        return 0
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int[] nums;
    private int t;

    public int componentValue(int[] nums, int[][] edges) {
        int n = nums.length;
        g = new List[n];
        this.nums = nums;
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        int s = sum(nums), mx = max(nums);
        for (int k = Math.min(n, s / mx); k > 1; --k) {
            if (s % k == 0) {
                t = s / k;
                if (dfs(0, -1) == 0) {
                    return k - 1;
                }
            }
        }
        return 0;
    }

    private int dfs(int i, int fa) {
        int x = nums[i];
        for (int j : g[i]) {
            if (j != fa) {
                int y = dfs(j, i);
                if (y == -1) {
                    return -1;
                }
                x += y;
            }
        }
        if (x > t) {
            return -1;
        }
        return x < t ? x : 0;
    }

    private int sum(int[] arr) {
        int x = 0;
        for (int v : arr) {
            x += v;
        }
        return x;
    }

    private int max(int[] arr) {
        int x = arr[0];
        for (int v : arr) {
            x = Math.max(x, v);
        }
        return x;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int componentValue(vector<int>& nums, vector<vector<int>>& edges) {
        int n = nums.size();
        int s = accumulate(nums.begin(), nums.end(), 0);
        int mx = *max_element(nums.begin(), nums.end());
        int t = 0;
        unordered_map<int, vector<int>> g;
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        function<int(int, int)> dfs = [&](int i, int fa) -> int {
            int x = nums[i];
            for (int j : g[i]) {
                if (j != fa) {
                    int y = dfs(j, i);
                    if (y == -1) return -1;
                    x += y;
                }
            }
            if (x > t) return -1;
            return x < t ? x : 0;
        };
        for (int k = min(n, s / mx); k > 1; --k) {
            if (s % k == 0) {
                t = s / k;
                if (dfs(0, -1) == 0) {
                    return k - 1;
                }
            }
        }
        return 0;
    }
};
```

#### Go

```go
func componentValue(nums []int, edges [][]int) int {
	s, mx := 0, slices.Max(nums)
	for _, x := range nums {
		s += x
	}
	n := len(nums)
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	t := 0
	var dfs func(int, int) int
	dfs = func(i, fa int) int {
		x := nums[i]
		for _, j := range g[i] {
			if j != fa {
				y := dfs(j, i)
				if y == -1 {
					return -1
				}
				x += y
			}
		}
		if x > t {
			return -1
		}
		if x < t {
			return x
		}
		return 0
	}
	for k := min(n, s/mx); k > 1; k-- {
		if s%k == 0 {
			t = s / k
			if dfs(0, -1) == 0 {
				return k - 1
			}
		}
	}
	return 0
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
