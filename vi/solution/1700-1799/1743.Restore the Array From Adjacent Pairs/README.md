---
comments: true
difficulty: Medium
rating: 1579
source: Weekly Contest 226 Q2
tags:
    - Depth-First Search
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1743. Restore the Array From Adjacent Pairs](https://leetcode.com/problems/restore-the-array-from-adjacent-pairs)

[中文文档](/solution/1700-1799/1743.Restore%20the%20Array%20From%20Adjacent%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Có một mảng số nguyên <code>nums</code> gồm <code>n</code> phần tử <strong>khác nhau</strong>, nhưng bạn đã quên mảng đó. Tuy nhiên, bạn vẫn nhớ mọi cặp phần tử kề nhau trong <code>nums</code>.</p>

<p>Cho một mảng số nguyên 2 chiều <code>adjacentPairs</code> có kích thước <code>n - 1</code>, trong đó mỗi <code>adjacentPairs[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> cho biết hai phần tử <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code> kề nhau trong <code>nums</code>.</p>

<p>Đảm bảo mọi cặp phần tử kề nhau <code>nums[i]</code> và <code>nums[i+1]</code> đều xuất hiện trong <code>adjacentPairs</code>, dưới dạng <code>[nums[i], nums[i+1]]</code> hoặc <code>[nums[i+1], nums[i]]</code>. Các cặp có thể xuất hiện theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Trả về <em>mảng ban đầu </em><code>nums</code><em>. Nếu có nhiều đáp án, hãy trả về <strong>bất kỳ đáp án nào</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> adjacentPairs = [[2,1],[3,4],[3,2]]
<strong>Đầu ra:</strong> [1,2,3,4]
<strong>Giải thích:</strong> Mảng này có tất cả các cặp phần tử kề nhau trong adjacentPairs.
Lưu ý rằng adjacentPairs[i] có thể không theo thứ tự từ trái sang phải.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> adjacentPairs = [[4,-2],[1,4],[-3,1]]
<strong>Đầu ra:</strong> [-2,4,1,-3]
<strong>Giải thích:</strong> Các số có thể là số âm.
Một đáp án khác là [-3,1,4,-2], cũng được chấp nhận.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> adjacentPairs = [[100000,-100000]]
<strong>Đầu ra:</strong> [100000,-100000]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>nums.length == n</code></li>
	<li><code>adjacentPairs.length == n - 1</code></li>
	<li><code>adjacentPairs[i].length == 2</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i], u<sub>i</sub>, v<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li>Tồn tại một <code>nums</code> có <code>adjacentPairs</code> là các cặp kề nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Các cặp kề nhau tạo thành một đường đi; hai đầu mút có bậc bằng $1$. Duyệt từ một trong hai đầu mút sẽ khôi phục được mảng.
>
> Xây dựng danh sách kề vô hướng, chọn node bậc $1$ làm $ans[0]$ và hàng xóm của nó làm $ans[1]$. Mỗi giá trị tiếp theo là hàng xóm không phải giá trị ngay trước đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def restoreArray(self, adjacentPairs: List[List[int]]) -> List[int]:
        g = defaultdict(list)
        for a, b in adjacentPairs:
            g[a].append(b)
            g[b].append(a)
        n = len(adjacentPairs) + 1
        ans = [0] * n
        for i, v in g.items():
            if len(v) == 1:
                ans[0] = i
                ans[1] = v[0]
                break
        for i in range(2, n):
            v = g[ans[i - 1]]
            ans[i] = v[0] if v[1] == ans[i - 2] else v[1]
        return ans
```

#### Java

```java
class Solution {
    public int[] restoreArray(int[][] adjacentPairs) {
        int n = adjacentPairs.length + 1;
        Map<Integer, List<Integer>> g = new HashMap<>();
        for (int[] e : adjacentPairs) {
            int a = e[0], b = e[1];
            g.computeIfAbsent(a, k -> new ArrayList<>()).add(b);
            g.computeIfAbsent(b, k -> new ArrayList<>()).add(a);
        }
        int[] ans = new int[n];
        for (Map.Entry<Integer, List<Integer>> entry : g.entrySet()) {
            if (entry.getValue().size() == 1) {
                ans[0] = entry.getKey();
                ans[1] = entry.getValue().get(0);
                break;
            }
        }
        for (int i = 2; i < n; ++i) {
            List<Integer> v = g.get(ans[i - 1]);
            ans[i] = v.get(1) == ans[i - 2] ? v.get(0) : v.get(1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> restoreArray(vector<vector<int>>& adjacentPairs) {
        int n = adjacentPairs.size() + 1;
        unordered_map<int, vector<int>> g;
        for (auto& e : adjacentPairs) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        vector<int> ans(n);
        for (auto& [k, v] : g) {
            if (v.size() == 1) {
                ans[0] = k;
                ans[1] = v[0];
                break;
            }
        }
        for (int i = 2; i < n; ++i) {
            auto v = g[ans[i - 1]];
            ans[i] = v[0] == ans[i - 2] ? v[1] : v[0];
        }
        return ans;
    }
};
```

#### Go

```go
func restoreArray(adjacentPairs [][]int) []int {
	n := len(adjacentPairs) + 1
	g := map[int][]int{}
	for _, e := range adjacentPairs {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	ans := make([]int, n)
	for k, v := range g {
		if len(v) == 1 {
			ans[0] = k
			ans[1] = v[0]
			break
		}
	}
	for i := 2; i < n; i++ {
		v := g[ans[i-1]]
		ans[i] = v[0]
		if v[0] == ans[i-2] {
			ans[i] = v[1]
		}
	}
	return ans
}
```

#### C#

```cs
public class Solution {
    public int[] RestoreArray(int[][] adjacentPairs) {
        int n = adjacentPairs.Length + 1;
        Dictionary<int, List<int>> g = new Dictionary<int, List<int>>();

        foreach (int[] e in adjacentPairs) {
            int a = e[0], b = e[1];
            if (!g.ContainsKey(a)) {
                g[a] = new List<int>();
            }
            if (!g.ContainsKey(b)) {
                g[b] = new List<int>();
            }
            g[a].Add(b);
            g[b].Add(a);
        }

        int[] ans = new int[n];

        foreach (var entry in g) {
            if (entry.Value.Count == 1) {
                ans[0] = entry.Key;
                ans[1] = entry.Value[0];
                break;
            }
        }

        for (int i = 2; i < n; ++i) {
            List<int> v = g[ans[i - 1]];
            ans[i] = v[1] == ans[i - 2] ? v[0] : v[1];
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 duyệt các hàng xóm theo cách lặp. DFS từ một đầu mút bậc $1$ cho cùng thứ tự và thuận tiện khi cài đặt đệ quy.

<!-- thinking:end -->

Bắt đầu từ một đầu mút bậc 1 và DFS trên danh sách kề.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def restoreArray(self, adjacentPairs: List[List[int]]) -> List[int]:
        def dfs(i, fa):
            ans.append(i)
            for j in g[i]:
                if j != fa:
                    dfs(j, i)

        g = defaultdict(list)
        for a, b in adjacentPairs:
            g[a].append(b)
            g[b].append(a)
        i = next(i for i, v in g.items() if len(v) == 1)
        ans = []
        dfs(i, 1e6)
        return ans
```

#### Java

```java
class Solution {
    private Map<Integer, List<Integer>> g = new HashMap<>();
    private int[] ans;

    public int[] restoreArray(int[][] adjacentPairs) {
        for (var e : adjacentPairs) {
            int a = e[0], b = e[1];
            g.computeIfAbsent(a, k -> new ArrayList<>()).add(b);
            g.computeIfAbsent(b, k -> new ArrayList<>()).add(a);
        }
        int n = adjacentPairs.length + 1;
        ans = new int[n];
        for (var e : g.entrySet()) {
            if (e.getValue().size() == 1) {
                dfs(e.getKey(), 1000000, 0);
                break;
            }
        }
        return ans;
    }

    private void dfs(int i, int fa, int k) {
        ans[k++] = i;
        for (int j : g.get(i)) {
            if (j != fa) {
                dfs(j, i, k);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> restoreArray(vector<vector<int>>& adjacentPairs) {
        unordered_map<int, vector<int>> g;
        for (auto& e : adjacentPairs) {
            int a = e[0], b = e[1];
            g[a].emplace_back(b);
            g[b].emplace_back(a);
        }
        int n = adjacentPairs.size() + 1;
        vector<int> ans;
        function<void(int, int)> dfs = [&](int i, int fa) {
            ans.emplace_back(i);
            for (int& j : g[i]) {
                if (j != fa) {
                    dfs(j, i);
                }
            }
        };
        for (auto& [i, v] : g) {
            if (v.size() == 1) {
                dfs(i, 1e6);
                break;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func restoreArray(adjacentPairs [][]int) []int {
	g := map[int][]int{}
	for _, e := range adjacentPairs {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	ans := []int{}
	var dfs func(i, fa int)
	dfs = func(i, fa int) {
		ans = append(ans, i)
		for _, j := range g[i] {
			if j != fa {
				dfs(j, i)
			}
		}
	}
	for i, v := range g {
		if len(v) == 1 {
			dfs(i, 1000000)
			break
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
