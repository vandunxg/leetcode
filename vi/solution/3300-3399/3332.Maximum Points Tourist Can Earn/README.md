---
comments: true
difficulty: Medium
rating: 1827
source: Biweekly Contest 142 Q3
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [3332. Maximum Points Tourist Can Earn](https://leetcode.com/problems/maximum-points-tourist-can-earn)

[中文文档](/solution/3300-3399/3332.Maximum%20Points%20Tourist%20Can%20Earn/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>n</code> và <code>k</code>, cùng với hai mảng số nguyên 2 chiều <code>stayScore</code> và <code>travelScore</code>.</p>

<p>Một du khách đang tham quan một quốc gia có <code>n</code> thành phố, trong đó mỗi thành phố đều được kết nối <strong>trực tiếp</strong> với mọi thành phố khác. Hành trình của du khách kéo dài <strong>chính xác</strong> <code>k</code> ngày <strong>với chỉ số bắt đầu từ 0</strong>, và họ có thể chọn <strong>bất kỳ</strong> thành phố nào làm điểm xuất phát.</p>

<p>Mỗi ngày, du khách có hai lựa chọn:</p>

<ul>
    <li><strong>Ở lại thành phố hiện tại</strong>: Nếu du khách ở lại thành phố hiện tại <code>curr</code> trong ngày <code>i</code>, họ nhận được <code>stayScore[i][curr]</code> điểm.</li>
    <li><strong>Di chuyển đến thành phố khác</strong>: Nếu du khách di chuyển từ thành phố hiện tại <code>curr</code> đến thành phố <code>dest</code>, họ nhận được <code>travelScore[curr][dest]</code> điểm.</li>
</ul>

<p>Hãy trả về số điểm <strong>lớn nhất</strong> mà du khách có thể nhận được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, k = 1, stayScore = [[2,3]], travelScore = [[0,2],[1,0]]</span></p>

<p><strong>Đầu ra:</strong> 3</p>

<p><strong>Giải thích:</strong></p>

<p>Du khách nhận được số điểm lớn nhất bằng cách bắt đầu ở thành phố 1 và ở lại thành phố đó.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, k = 2, stayScore = [[3,4,2],[2,1,2]], travelScore = [[0,2,1],[2,0,4],[3,2,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p>Du khách nhận được số điểm lớn nhất bằng cách bắt đầu ở thành phố 1, ở lại thành phố đó trong ngày 0, rồi di chuyển đến thành phố 2 trong ngày 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 200</code></li>
    <li><code>1 &lt;= k &lt;= 200</code></li>
    <li><code>n == travelScore.length == travelScore[i].length == stayScore[i].length</code></li>
    <li><code>k == stayScore.length</code></li>
    <li><code>1 &lt;= stayScore[i][j] &lt;= 100</code></li>
    <li><code>0 &lt;= travelScore[i][j] &lt;= 100</code></li>
    <li><code>travelScore[i][i] == 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Trong $k$ ngày, ta có thể ở lại hoặc di chuyển, tương ứng nhận điểm ở lại hoặc điểm di chuyển. Với $n,k \le 200$, không gian trạng thái là $O(nk)$ và mỗi bước chuyển sẽ duyệt qua thành phố trước đó.
>
> $f[i][j]$ là số điểm tốt nhất sau ngày $i$ khi ở thành phố $j$. Nếu ở lại thì $h=j$ và cộng thêm $\textit{stayScore}[i-1][j]$; nếu di chuyển từ $h$ thì cộng thêm $\textit{travelScore}[h][j]$.
>
> Ngày $0$ có điểm số bằng $0$ ở mọi thành phố. Đáp án là giá trị lớn nhất của $f[k]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(
        self, n: int, k: int, stayScore: List[List[int]], travelScore: List[List[int]]
    ) -> int:
        f = [[-inf] * n for _ in range(k + 1)]
        f[0] = [0] * n
        for i in range(1, k + 1):
            for j in range(n):
                for h in range(n):
                    f[i][j] = max(
                        f[i][j],
                        f[i - 1][h]
                        + (stayScore[i - 1][j] if j == h else travelScore[h][j]),
                    )
        return max(f[k])
```

#### Java

```java
class Solution {
    public int maxScore(int n, int k, int[][] stayScore, int[][] travelScore) {
        int[][] f = new int[k + 1][n];
        for (var g : f) {
            Arrays.fill(g, Integer.MIN_VALUE);
        }
        Arrays.fill(f[0], 0);
        for (int i = 1; i <= k; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int h = 0; h < n; ++h) {
                    f[i][j] = Math.max(
                        f[i][j], f[i - 1][h] + (j == h ? stayScore[i - 1][j] : travelScore[h][j]));
                }
            }
        }
        return Arrays.stream(f[k]).max().getAsInt();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxScore(int n, int k, vector<vector<int>>& stayScore, vector<vector<int>>& travelScore) {
        int f[k + 1][n];
        memset(f, 0xc0, sizeof(f));
        memset(f[0], 0, sizeof(f[0]));
        for (int i = 1; i <= k; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int h = 0; h < n; ++h) {
                    f[i][j] = max(f[i][j], f[i - 1][h] + (j == h ? stayScore[i - 1][j] : travelScore[h][j]));
                }
            }
        }
        return *max_element(f[k], f[k] + n);
    }
};
```

#### Go

```go
func maxScore(n int, k int, stayScore [][]int, travelScore [][]int) (ans int) {
    f := make([][]int, k+1)
    for i := range f {
        f[i] = make([]int, n)
        for j := range f[i] {
            f[i][j] = math.MinInt32
        }
    }
    for j := 0; j < n; j++ {
        f[0][j] = 0
    }
    for i := 1; i <= k; i++ {
        for j := 0; j < n; j++ {
            f[i][j] = f[i-1][j] + stayScore[i-1][j]
            for h := 0; h < n; h++ {
                if h != j {
                    f[i][j] = max(f[i][j], f[i-1][h]+travelScore[h][j])
                }
            }
        }
    }
    for j := 0; j < n; j++ {
        ans = max(ans, f[k][j])
    }
    return
}
```

#### TypeScript

```ts
function maxScore(n: number, k: number, stayScore: number[][], travelScore: number[][]): number {
    const f: number[][] = Array.from({ length: k + 1 }, () => Array(n).fill(-Infinity));
    f[0].fill(0);
    for (let i = 1; i <= k; ++i) {
        for (let j = 0; j < n; ++j) {
            for (let h = 0; h < n; ++h) {
                f[i][j] = Math.max(
                    f[i][j],
                    f[i - 1][h] + (j == h ? stayScore[i - 1][j] : travelScore[h][j]),
                );
            }
        }
    }
    return Math.max(...f[k]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
