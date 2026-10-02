---
comments: true
difficulty: Medium
rating: 1658
source: Weekly Contest 206 Q2
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [1583. Count Unhappy Friends](https://leetcode.com/problems/count-unhappy-friends)

[中文文档](/solution/1500-1599/1583.Count%20Unhappy%20Friends/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách <code>preferences</code> của <code>n</code> người bạn, trong đó <code>n</code> luôn là số <strong>chẵn</strong>.</p>

<p>Với mỗi người <code>i</code>, <code>preferences[i]</code> chứa danh sách bạn bè được <strong>sắp xếp</strong> theo <strong>mức độ yêu thích</strong>. Nói cách khác, người đứng trước được thích hơn người đứng sau. Bạn bè trong mỗi danh sách được biểu diễn bằng các số nguyên từ <code>0</code> đến <code>n-1</code>.</p>

<p>Tất cả bạn bè được chia thành các cặp. Các cặp được cho trong danh sách <code>pairs</code>, trong đó <code>pairs[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> nghĩa là <code>x<sub>i</sub></code> ghép cặp với <code>y<sub>i</sub></code> và ngược lại.</p>
Hai đầu mút của cặp là <code>x<sub>i</sub></code> và <code>y<sub>i</sub></code>.

<p>Tuy nhiên, cách ghép cặp này có thể khiến một số người không vui. Một người <code>x</code> không vui nếu <code>x</code> ghép với <code>y</code> và tồn tại người <code>u</code> ghép với <code>v</code> sao cho:</p>

<ul>
	<li><code>x</code> thích <code>u</code> hơn <code>y</code>, và</li>
	<li><code>u</code> thích <code>x</code> hơn <code>v</code>.</li>
</ul>

<p>Trả về <em>số người không vui</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, preferences = [[1, 2, 3], [3, 2, 0], [3, 1, 0], [1, 2, 0]], pairs = [[0, 1], [2, 3]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Friend 1 is unhappy because:
- 1 ghép với 0 nhưng thích 3 hơn 0, và
- 3 thích 1 hơn 2.
Friend 3 is unhappy because:
- 3 ghép với 2 nhưng thích 1 hơn 2, và
- 1 thích 3 hơn 0.
Hai người 0 và 2 vui.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, preferences = [[1], [0]], pairs = [[1, 0]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Cả hai người 0 và 1 đều vui.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, preferences = [[1, 3, 2], [2, 3, 0], [1, 3, 0], [0, 2, 1]], pairs = [[1, 3], [0, 2]]
<strong>Đầu ra:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 500</code></li>
	<li><code>n</code> là số chẵn.</li>
	<li><code>preferences.length&nbsp;== n</code></li>
	<li><code>preferences[i].length&nbsp;== n - 1</code></li>
	<li><code>0 &lt;= preferences[i][j] &lt;= n - 1</code></li>
	<li><code>preferences[i]</code>&nbsp;does not contain <code>i</code>.</li>
	<li>Mọi giá trị trong <code>preferences[i]</code> đều khác nhau.</li>
	<li><code>pairs.length&nbsp;== n/2</code></li>
	<li><code>pairs[i].length&nbsp;== 2</code></li>
	<li><code>x<sub>i</sub> != y<sub>i</sub></code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub>&nbsp;&lt;= n - 1</code></li>
	<li>Mỗi người thuộc <strong>đúng một</strong> cặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi ghép cặp, $x$ không vui nếu có người $u$ được $x$ thích hơn người ghép cặp và $u$ cũng thích $x$ hơn người ghép cặp của mình. Vì $n\le 500$, ta có thể duyệt từng $x$ và những người được $x$ thích hơn.
>
> Xây dựng bảng thứ hạng và bảng ghép cặp. Với mỗi x, chỉ xét những người được xếp trước người ghép cặp y. Người $u$ đầu tiên xếp $x$ trước người ghép cặp của mình sẽ khiến $x$ không vui, khi đó có thể dừng vòng lặp trong.

<!-- thinking:end -->

Ta dùng mảng $\textit{d}$ để ghi lại thứ hạng giữa từng cặp bạn bè, trong đó $\textit{d}[i][j]$ biểu diễn mức độ thân thiết của người $i$ với người $j$ (giá trị càng nhỏ thì càng được ưu tiên). Ngoài ra, ta dùng mảng $\textit{p}$ để ghi lại người ghép cặp với mỗi người.

Ta liệt kê từng người $x$. Với người ghép cặp $y$ của $x$, ta tìm thứ hạng $\textit{d}[x][y]$ của $y$ trong danh sách của $x$. Sau đó, ta xét các người $u$ được ưu tiên hơn $y$. Nếu tồn tại $u$ sao cho $u$ xếp $x$ trước người ghép cặp của mình, thì $x$ không vui và ta tăng kết quả lên một.

Sau khi liệt kê, ta thu được số người không vui.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là số người bạn.

Trong phép kiểm tra, ta dùng người $u$ và các thứ hạng $\textit{d}[x][y]$, $\textit{d}[u][x]$, $\textit{d}[u][y]$; nếu tồn tại người $u$ phù hợp thì người x không vui.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def unhappyFriends(
        self, n: int, preferences: List[List[int]], pairs: List[List[int]]
    ) -> int:
        d = [{x: j for j, x in enumerate(p)} for p in preferences]
        p = {}
        for x, y in pairs:
            p[x] = y
            p[y] = x
        ans = 0
        for x in range(n):
            y = p[x]
            for i in range(d[x][y]):
                u = preferences[x][i]
                v = p[u]
                if d[u][x] < d[u][v]:
                    ans += 1
                    break
        return ans
```

#### Java

```java
class Solution {
    public int unhappyFriends(int n, int[][] preferences, int[][] pairs) {
        int[][] d = new int[n][n];
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n - 1; ++j) {
                d[i][preferences[i][j]] = j;
            }
        }
        int[] p = new int[n];
        for (var e : pairs) {
            int x = e[0], y = e[1];
            p[x] = y;
            p[y] = x;
        }
        int ans = 0;
        for (int x = 0; x < n; ++x) {
            int y = p[x];
            for (int i = 0; i < d[x][y]; ++i) {
                int u = preferences[x][i];
                int v = p[u];
                if (d[u][x] < d[u][v]) {
                    ++ans;
                    break;
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int unhappyFriends(int n, vector<vector<int>>& preferences, vector<vector<int>>& pairs) {
        vector<vector<int>> d(n, vector<int>(n));
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n - 1; ++j) {
                d[i][preferences[i][j]] = j;
            }
        }
        vector<int> p(n, 0);
        for (auto& e : pairs) {
            int x = e[0], y = e[1];
            p[x] = y;
            p[y] = x;
        }
        int ans = 0;
        for (int x = 0; x < n; ++x) {
            int y = p[x];
            for (int i = 0; i < d[x][y]; ++i) {
                int u = preferences[x][i];
                int v = p[u];
                if (d[u][x] < d[u][v]) {
                    ++ans;
                    break;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func unhappyFriends(n int, preferences [][]int, pairs [][]int) (ans int) {
	d := make([][]int, n)
	for i := range d {
		d[i] = make([]int, n)
	}

	for i := 0; i < n; i++ {
		for j := 0; j < n-1; j++ {
			d[i][preferences[i][j]] = j
		}
	}

	p := make([]int, n)
	for _, e := range pairs {
		x, y := e[0], e[1]
		p[x] = y
		p[y] = x
	}

	for x := 0; x < n; x++ {
		y := p[x]
		for i := 0; i < d[x][y]; i++ {
			u := preferences[x][i]
			v := p[u]
			if d[u][x] < d[u][v] {
				ans++
				break
			}
		}
	}

	return
}
```

#### TypeScript

```ts
function unhappyFriends(n: number, preferences: number[][], pairs: number[][]): number {
    const d: number[][] = Array.from({ length: n }, () => Array(n).fill(0));
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n - 1; ++j) {
            d[i][preferences[i][j]] = j;
        }
    }
    const p: number[] = Array(n).fill(0);
    for (const [x, y] of pairs) {
        p[x] = y;
        p[y] = x;
    }
    let ans = 0;
    for (let x = 0; x < n; ++x) {
        const y = p[x];
        for (let i = 0; i < d[x][y]; ++i) {
            const u = preferences[x][i];
            const v = p[u];
            if (d[u][x] < d[u][v]) {
                ++ans;
                break;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn unhappy_friends(n: i32, preferences: Vec<Vec<i32>>, pairs: Vec<Vec<i32>>) -> i32 {
        let n = n as usize;
        let mut d = vec![vec![0usize; n]; n];
        for i in 0..n {
            for j in 0..(n - 1) {
                let friend = preferences[i][j] as usize;
                d[i][friend] = j;
            }
        }

        let mut p = vec![0usize; n];
        for e in pairs {
            let x = e[0] as usize;
            let y = e[1] as usize;
            p[x] = y;
            p[y] = x;
        }

        let mut ans = 0;

        for x in 0..n {
            let y = p[x];
            for i in 0..d[x][y] {
                let u = preferences[x][i] as usize;
                let v = p[u];
                if d[u][x] < d[u][v] {
                    ans += 1;
                    break;
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
