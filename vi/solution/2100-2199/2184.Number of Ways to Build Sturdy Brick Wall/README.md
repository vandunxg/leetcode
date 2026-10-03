---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [2184. Number of Ways to Build Sturdy Brick Wall 🔒](https://leetcode.com/problems/number-of-ways-to-build-sturdy-brick-wall)

[中文文档](/solution/2100-2199/2184.Number%20of%20Ways%20to%20Build%20Sturdy%20Brick%20Wall/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <code>height</code> và <code>width</code> biểu thị kích thước của một bức tường gạch cần xây. Bạn cũng được cho một mảng số nguyên <strong>không trùng lặp</strong> <strong>được đánh chỉ số từ 0</strong> <code>bricks</code>, trong đó viên gạch thứ <code>i<sup>th</sup></code> có chiều cao bằng <code>1</code> và chiều rộng bằng <code>bricks[i]</code>. Bạn có nguồn cung <strong>vô hạn </strong>cho mỗi loại gạch và <strong>không được xoay</strong> gạch.</p>

<p>Mỗi hàng trong bức tường phải có độ dài đúng bằng <code>width</code> đơn vị. Để bức tường <strong>vững chắc</strong>, các hàng kề nhau <strong>không được </strong>nối các viên gạch tại cùng một vị trí, ngoại trừ hai đầu tường.</p>

<p>Trả về <em>số cách xây dựng một bức tường <strong>vững chắc</strong></em>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>chia lấy dư</strong> cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2184.Number%20of%20Ways%20to%20Build%20Sturdy%20Brick%20Wall/images/image-20220220190749-1.png" style="width: 919px; height: 250px;" />
<pre>
<strong>Đầu vào:</strong> height = 2, width = 3, bricks = [1,2]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Hai bức tường đầu tiên trong hình là hai cách duy nhất để xây một bức tường gạch vững chắc.
Lưu ý rằng bức tường thứ ba trong hình không vững chắc vì các hàng kề nhau nối các viên gạch tại vị trí cách mép trái 2 đơn vị.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> height = 1, width = 1, bricks = [5]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Không có cách nào để xây một bức tường vững chắc vì loại gạch duy nhất có sẵn dài hơn chiều rộng của bức tường.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= height &lt;= 100</code></li>
	<li><code>1 &lt;= width &lt;= 10</code></li>
	<li><code>1 &lt;= bricks.length &lt;= 10</code></li>
	<li><code>1 &lt;= bricks[i] &lt;= 10</code></li>
	<li>Tất cả các giá trị trong <code>bricks</code> đều <strong>không trùng lặp</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi hàng là một cách phân chia chiều rộng, và các vết nối dọc của hai hàng kề nhau không được trùng vị trí. Vì chiều cao và số loại gạch đều bị giới hạn, chúng ta liệt kê tất cả cách sắp xếp một hàng rồi xác định tính tương thích giữa chúng.
>
> DFS xây dựng các dãy độ dài viên gạch có tổng bằng $\textit{width}$. Hai dãy không tương thích nếu chúng có cùng một tổng tiền tố khác $0$ và $\textit{width}$. Việc chuyển từ hàng này sang hàng kế tiếp là bài toán đếm đường đi trên đồ thị đó: $\textit{dp}[i][j]$ là số cách sử dụng cách sắp xếp $j$ cho hàng $i$.
>
> Khởi tạo hàng đầu tiên bằng một, cộng các trạng thái tiền nhiệm tương thích, rồi tính tổng hàng cuối cùng theo modulo $10^9+7$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def buildWall(self, height: int, width: int, bricks: List[int]) -> int:
        def dfs(v):
            if v > width:
                return
            if v == width:
                s.append(t[:])
                return
            for x in bricks:
                t.append(x)
                dfs(v + x)
                t.pop()

        def check(a, b):
            s1, s2 = a[0], b[0]
            i = j = 1
            while i < len(a) and j < len(b):
                if s1 == s2:
                    return False
                if s1 < s2:
                    s1 += a[i]
                    i += 1
                else:
                    s2 += b[j]
                    j += 1
            return True

        mod = 10**9 + 7
        s = []
        t = []
        dfs(0)
        g = defaultdict(list)
        n = len(s)
        for i in range(n):
            if check(s[i], s[i]):
                g[i].append(i)
            for j in range(i + 1, n):
                if check(s[i], s[j]):
                    g[i].append(j)
                    g[j].append(i)
        dp = [[0] * n for _ in range(height)]
        for j in range(n):
            dp[0][j] = 1
        for i in range(1, height):
            for j in range(n):
                for k in g[j]:
                    dp[i][j] += dp[i - 1][k]
                    dp[i][j] %= mod
        return sum(dp[-1]) % mod
```

#### Java

```java
class Solution {
    private List<List<Integer>> res = new ArrayList<>();
    private List<Integer> t = new ArrayList<>();
    private static final int MOD = (int) 1e9 + 7;
    private int width;
    private int[] bricks;

    public int buildWall(int height, int width, int[] bricks) {
        this.width = width;
        this.bricks = bricks;
        dfs(0);
        int n = res.size();
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int i = 0; i < n; ++i) {
            if (check(res.get(i), res.get(i))) {
                g[i].add(i);
            }
            for (int j = i + 1; j < n; ++j) {
                if (check(res.get(i), res.get(j))) {
                    g[i].add(j);
                    g[j].add(i);
                }
            }
        }
        int[][] dp = new int[height][n];
        for (int j = 0; j < n; ++j) {
            dp[0][j] = 1;
        }
        for (int i = 1; i < height; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k : g[j]) {
                    dp[i][j] = (dp[i][j] + dp[i - 1][k]) % MOD;
                }
            }
        }
        int ans = 0;
        for (int j = 0; j < n; ++j) {
            ans = (ans + dp[height - 1][j]) % MOD;
        }
        return ans;
    }

    private boolean check(List<Integer> a, List<Integer> b) {
        int s1 = a.get(0);
        int s2 = b.get(0);
        int i = 1, j = 1;
        while (i < a.size() && j < b.size()) {
            if (s1 == s2) {
                return false;
            }
            if (s1 < s2) {
                s1 += a.get(i++);
            } else {
                s2 += b.get(j++);
            }
        }
        return true;
    }

    private void dfs(int v) {
        if (v > width) {
            return;
        }
        if (v == width) {
            res.add(new ArrayList<>(t));
            return;
        }
        for (int x : bricks) {
            t.add(x);
            dfs(v + x);
            t.remove(t.size() - 1);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> bricks;
    int width;
    int mod = 1e9 + 7;
    vector<vector<int>> res;
    vector<int> t;

    int buildWall(int height, int width, vector<int>& bricks) {
        this->width = width;
        this->bricks = bricks;
        dfs(0);
        t.resize(0);
        int n = res.size();
        vector<vector<int>> g(n);
        for (int i = 0; i < n; ++i) {
            if (check(res[i], res[i])) {
                g[i].push_back(i);
            }
            for (int j = i + 1; j < n; ++j) {
                if (check(res[i], res[j])) {
                    g[i].push_back(j);
                    g[j].push_back(i);
                }
            }
        }
        vector<vector<int>> dp(height, vector<int>(n));
        for (int j = 0; j < n; ++j) dp[0][j] = 1;
        for (int i = 1; i < height; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k : g[j]) {
                    dp[i][j] += dp[i - 1][k];
                    dp[i][j] %= mod;
                }
            }
        }
        int ans = 0;
        for (int j = 0; j < n; ++j) {
            ans += dp[height - 1][j];
            ans %= mod;
        }
        return ans;
    }

    bool check(vector<int>& a, vector<int>& b) {
        int s1 = a[0], s2 = b[0];
        int i = 1, j = 1;
        while (i < a.size() && j < b.size()) {
            if (s1 == s2) return false;
            if (s1 < s2)
                s1 += a[i++];
            else
                s2 += b[j++];
        }
        return true;
    }

    void dfs(int v) {
        if (v > width) return;
        if (v == width) {
            res.push_back(t);
            return;
        }
        for (int x : bricks) {
            t.push_back(x);
            dfs(v + x);
            t.pop_back();
        }
    }
};
```

#### Go

```go
func buildWall(height int, width int, bricks []int) int {
	mod := int(1e9) + 7
	res := [][]int{}
	t := []int{}
	var dfs func(int)
	dfs = func(v int) {
		if v > width {
			return
		}
		if v == width {
			res = append(res, slices.Clone(t))
			return
		}
		for _, x := range bricks {
			t = append(t, x)
			dfs(v + x)
			t = t[:len(t)-1]
		}
	}
	check := func(a, b []int) bool {
		s1, s2 := a[0], b[0]
		i, j := 1, 1
		for i < len(a) && j < len(b) {
			if s1 == s2 {
				return false
			}
			if s1 < s2 {
				s1 += a[i]
				i++
			} else {
				s2 += b[j]
				j++
			}
		}
		return true
	}
	dfs(0)
	n := len(res)
	g := make([][]int, n)
	for i := 0; i < n; i++ {
		if check(res[i], res[i]) {
			g[i] = append(g[i], i)
		}
		for j := i + 1; j < n; j++ {
			if check(res[i], res[j]) {
				g[i] = append(g[i], j)
				g[j] = append(g[j], i)
			}
		}
	}
	dp := make([][]int, height)
	for i := range dp {
		dp[i] = make([]int, n)
	}
	for j := 0; j < n; j++ {
		dp[0][j] = 1
	}
	for i := 1; i < height; i++ {
		for j := 0; j < n; j++ {
			for _, k := range g[j] {
				dp[i][j] += dp[i-1][k]
				dp[i][j] %= mod
			}
		}
	}
	ans := 0
	for j := 0; j < n; j++ {
		ans += dp[height-1][j]
		ans %= mod
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
