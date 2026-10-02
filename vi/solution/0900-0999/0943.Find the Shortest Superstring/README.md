---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Array
    - String
    - Dynamic Programming
    - Bitmask
    - Hamiltonian Path
---

<!-- problem:start -->

# [943. Find the Shortest Superstring](https://leetcode.com/problems/find-the-shortest-superstring)

[中文文档](/solution/0900-0999/0943.Find%20the%20Shortest%20Superstring/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>words</code>, hãy trả về <em>chuỗi ngắn nhất chứa mọi chuỗi trong </em><code>words</code><em> dưới dạng chuỗi con</em>. Nếu có nhiều chuỗi hợp lệ cùng độ dài ngắn nhất, trả về <strong>bất kỳ chuỗi nào trong số đó</strong>.</p>

<p>Có thể giả sử không có chuỗi nào trong <code>words</code> là chuỗi con của một chuỗi khác trong <code>words</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;alex&quot;,&quot;loves&quot;,&quot;leetcode&quot;]
<strong>Đầu ra:</strong> &quot;alexlovesleetcode&quot;
<strong>Giải thích:</strong> Mọi hoán vị của &quot;alex&quot;,&quot;loves&quot;,&quot;leetcode&quot; đều được chấp nhận.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;catg&quot;,&quot;ctaagt&quot;,&quot;gcta&quot;,&quot;ttca&quot;,&quot;atgcatc&quot;]
<strong>Đầu ra:</strong> &quot;gctaagttcatgcatc&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 12</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 20</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Tất cả chuỗi trong <code>words</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Siêu chuỗi ngắn nhất tương ứng với đường đi Hamilton trên các chuỗi, tối đa hóa phần chồng lấp. Vì $n\le 12$, có thể dùng tập con. Tiền tính $g[i][j]$, độ dài chồng lấp lớn nhất giữa hậu tố của chuỗi $i$ và tiền tố của chuỗi $j$, rồi dùng DP: $dp[S][j]$ là tổng độ chồng lấp lớn nhất sau khi đi qua tập $S$ và kết thúc tại $j$. Khôi phục thứ tự qua các trạng thái trước đó rồi nối thêm mọi chuỗi còn lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestSuperstring(self, words: List[str]) -> str:
        n = len(words)
        g = [[0] * n for _ in range(n)]
        for i, a in enumerate(words):
            for j, b in enumerate(words):
                if i != j:
                    for k in range(min(len(a), len(b)), 0, -1):
                        if a[-k:] == b[:k]:
                            g[i][j] = k
                            break
        dp = [[0] * n for _ in range(1 << n)]
        p = [[-1] * n for _ in range(1 << n)]
        for i in range(1 << n):
            for j in range(n):
                if (i >> j) & 1:
                    pi = i ^ (1 << j)
                    for k in range(n):
                        if (pi >> k) & 1:
                            v = dp[pi][k] + g[k][j]
                            if v > dp[i][j]:
                                dp[i][j] = v
                                p[i][j] = k
        j = 0
        for i in range(n):
            if dp[-1][i] > dp[-1][j]:
                j = i
        arr = [j]
        i = (1 << n) - 1
        while p[i][j] != -1:
            i, j = i ^ (1 << j), p[i][j]
            arr.append(j)
        arr = arr[::-1]
        vis = set(arr)
        arr.extend([j for j in range(n) if j not in vis])
        ans = [words[arr[0]]] + [words[j][g[i][j] :] for i, j in pairwise(arr)]
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String shortestSuperstring(String[] words) {
        int n = words.length;
        int[][] g = new int[n][n];
        for (int i = 0; i < n; ++i) {
            String a = words[i];
            for (int j = 0; j < n; ++j) {
                String b = words[j];
                if (i != j) {
                    for (int k = Math.min(a.length(), b.length()); k > 0; --k) {
                        if (a.substring(a.length() - k).equals(b.substring(0, k))) {
                            g[i][j] = k;
                            break;
                        }
                    }
                }
            }
        }
        int[][] dp = new int[1 << n][n];
        int[][] p = new int[1 << n][n];
        for (int i = 0; i < 1 << n; ++i) {
            Arrays.fill(p[i], -1);
            for (int j = 0; j < n; ++j) {
                if (((i >> j) & 1) == 1) {
                    int pi = i ^ (1 << j);
                    for (int k = 0; k < n; ++k) {
                        if (((pi >> k) & 1) == 1) {
                            int v = dp[pi][k] + g[k][j];
                            if (v > dp[i][j]) {
                                dp[i][j] = v;
                                p[i][j] = k;
                            }
                        }
                    }
                }
            }
        }
        int j = 0;
        for (int i = 0; i < n; ++i) {
            if (dp[(1 << n) - 1][i] > dp[(1 << n) - 1][j]) {
                j = i;
            }
        }
        List<Integer> arr = new ArrayList<>();
        arr.add(j);
        for (int i = (1 << n) - 1; p[i][j] != -1;) {
            int k = i;
            i ^= (1 << j);
            j = p[k][j];
            arr.add(j);
        }
        Set<Integer> vis = new HashSet<>(arr);
        for (int i = 0; i < n; ++i) {
            if (!vis.contains(i)) {
                arr.add(i);
            }
        }
        Collections.reverse(arr);
        StringBuilder ans = new StringBuilder(words[arr.get(0)]);
        for (int i = 1; i < n; ++i) {
            int k = g[arr.get(i - 1)][arr.get(i)];
            ans.append(words[arr.get(i)].substring(k));
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string shortestSuperstring(vector<string>& words) {
        int n = words.size();
        vector<vector<int>> g(n, vector<int>(n));
        for (int i = 0; i < n; ++i) {
            auto a = words[i];
            for (int j = 0; j < n; ++j) {
                auto b = words[j];
                if (i != j) {
                    for (int k = min(a.size(), b.size()); k > 0; --k) {
                        if (a.substr(a.size() - k) == b.substr(0, k)) {
                            g[i][j] = k;
                            break;
                        }
                    }
                }
            }
        }
        vector<vector<int>> dp(1 << n, vector<int>(n));
        vector<vector<int>> p(1 << n, vector<int>(n, -1));
        for (int i = 0; i < 1 << n; ++i) {
            for (int j = 0; j < n; ++j) {
                if ((i >> j) & 1) {
                    int pi = i ^ (1 << j);
                    for (int k = 0; k < n; ++k) {
                        if ((pi >> k) & 1) {
                            int v = dp[pi][k] + g[k][j];
                            if (v > dp[i][j]) {
                                dp[i][j] = v;
                                p[i][j] = k;
                            }
                        }
                    }
                }
            }
        }
        int j = 0;
        for (int i = 0; i < n; ++i) {
            if (dp[(1 << n) - 1][i] > dp[(1 << n) - 1][j]) {
                j = i;
            }
        }
        vector<int> arr = {j};
        for (int i = (1 << n) - 1; p[i][j] != -1;) {
            int k = i;
            i ^= (1 << j);
            j = p[k][j];
            arr.push_back(j);
        }
        unordered_set<int> vis(arr.begin(), arr.end());
        for (int i = 0; i < n; ++i) {
            if (!vis.count(i)) {
                arr.push_back(i);
            }
        }
        reverse(arr.begin(), arr.end());
        string ans = words[arr[0]];
        for (int i = 1; i < n; ++i) {
            int k = g[arr[i - 1]][arr[i]];
            ans += words[arr[i]].substr(k);
        }
        return ans;
    }
};
```

#### Go

```go
func shortestSuperstring(words []string) string {
	n := len(words)
	g := make([][]int, n)
	for i, a := range words {
		g[i] = make([]int, n)
		for j, b := range words {
			if i != j {
				for k := min(len(a), len(b)); k > 0; k-- {
					if a[len(a)-k:] == b[:k] {
						g[i][j] = k
						break
					}
				}
			}
		}
	}
	dp := make([][]int, 1<<n)
	p := make([][]int, 1<<n)
	for i := 0; i < 1<<n; i++ {
		dp[i] = make([]int, n)
		p[i] = make([]int, n)
		for j := 0; j < n; j++ {
			p[i][j] = -1
			if ((i >> j) & 1) == 1 {
				pi := i ^ (1 << j)
				for k := 0; k < n; k++ {
					if ((pi >> k) & 1) == 1 {
						v := dp[pi][k] + g[k][j]
						if v > dp[i][j] {
							dp[i][j] = v
							p[i][j] = k
						}
					}
				}
			}
		}
	}
	j := 0
	for i := 0; i < n; i++ {
		if dp[(1<<n)-1][i] > dp[(1<<n)-1][j] {
			j = i
		}
	}
	arr := []int{j}
	vis := make([]bool, n)
	vis[j] = true
	for i := (1 << n) - 1; p[i][j] != -1; {
		k := i
		i ^= (1 << j)
		j = p[k][j]
		arr = append(arr, j)
		vis[j] = true
	}
	for i := 0; i < n; i++ {
		if !vis[i] {
			arr = append(arr, i)
		}
	}
	ans := &strings.Builder{}
	ans.WriteString(words[arr[n-1]])
	for i := n - 2; i >= 0; i-- {
		k := g[arr[i+1]][arr[i]]
		ans.WriteString(words[arr[i]][k:])
	}
	return ans.String()
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
