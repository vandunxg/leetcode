---
comments: true
difficulty: Easy
rating: 1285
source: Biweekly Contest 136 Q1
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [3238. Find the Number of Winning Players](https://leetcode.com/problems/find-the-number-of-winning-players)

[中文文档](/solution/3200-3299/3238.Find%20the%20Number%20of%20Winning%20Players/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu thị số người chơi trong một trò chơi và một mảng 2D <code>pick</code>, trong đó <code>pick[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu thị người chơi <code>x<sub>i</sub></code> đã chọn một quả bóng có màu <code>y<sub>i</sub></code>.</p>

<p>Người chơi <code>i</code> <strong>thắng</strong> trò chơi nếu họ chọn <strong>nhiều hơn</strong> <code>i</code> quả bóng có <strong>cùng</strong> màu. Nói cách khác,</p>

<ul>
	<li>Người chơi 0 thắng nếu họ chọn bất kỳ quả bóng nào.</li>
	<li>Người chơi 1 thắng nếu họ chọn ít nhất hai quả bóng có <em>cùng</em> màu.</li>
	<li>...</li>
	<li>Người chơi <code>i</code> thắng nếu họ chọn ít nhất <code>i + 1</code> quả bóng có <em>cùng</em> màu.</li>
</ul>

<p>Trả về số người chơi <strong>thắng</strong> trò chơi.</p>

<p><strong>Lưu ý</strong> rằng <em>nhiều</em> người chơi có thể thắng trò chơi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, pick = [[0,0],[1,0],[1,0],[2,1],[2,1],[2,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Người chơi 0 và người chơi 1 thắng trò chơi, còn người chơi 2 và người chơi 3 thì không.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, pick = [[1,1],[1,2],[1,3],[1,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có người chơi nào thắng trò chơi.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, pick = [[1,1],[2,4],[2,4],[2,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Người chơi 2 thắng trò chơi vì đã chọn 3 quả bóng màu 4.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10</code></li>
	<li><code>1 &lt;= pick.length &lt;= 100</code></li>
	<li><code>pick[i].length == 2</code></li>
	<li><code>0 &lt;= x<sub>i</sub> &lt;= n - 1 </code></li>
	<li><code>0 &lt;= y<sub>i</sub> &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Người chơi $x$ thắng nếu số lượng bóng của một màu nào đó vượt quá $x$. Vì $n\le 10$ và $\textit{pick}$ có độ dài không quá $100$, đếm trực tiếp là đủ.
>
> Một bảng lưu số lượng bóng của mỗi người chơi theo từng màu; khi đạt ngưỡng, ta đưa người chơi vào một set để không đếm trùng. Kích thước của set là số người thắng.

<!-- thinking:end -->

Ta có thể sử dụng một mảng 2D $\textit{cnt}$ để ghi lại số bóng mỗi màu mà mỗi người chơi nhận được, và một hash table $\textit{s}$ để ghi lại ID của những người chơi thắng.

Duyệt qua mảng $\textit{pick}$, với mỗi phần tử $[x, y]$, ta tăng $\textit{cnt}[x][y]$ lên một. Nếu $\textit{cnt}[x][y]$ lớn hơn $x$, ta thêm $x$ vào hash table $\textit{s}$.

Cuối cùng, trả về kích thước của hash table $\textit{s}$.

Độ phức tạp thời gian là $O(m + n \times M)$, và độ phức tạp không gian là $O(n \times M)$. Trong đó, $m$ là độ dài của mảng $\textit{pick}$, còn $n$ và $M$ lần lượt là số người chơi và số màu.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def winningPlayerCount(self, n: int, pick: List[List[int]]) -> int:
        cnt = [[0] * 11 for _ in range(n)]
        s = set()
        for x, y in pick:
            cnt[x][y] += 1
            if cnt[x][y] > x:
                s.add(x)
        return len(s)
```

#### Java

```java
class Solution {
    public int winningPlayerCount(int n, int[][] pick) {
        int[][] cnt = new int[n][11];
        Set<Integer> s = new HashSet<>();
        for (var p : pick) {
            int x = p[0], y = p[1];
            if (++cnt[x][y] > x) {
                s.add(x);
            }
        }
        return s.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int winningPlayerCount(int n, vector<vector<int>>& pick) {
        int cnt[10][11]{};
        unordered_set<int> s;
        for (const auto& p : pick) {
            int x = p[0], y = p[1];
            if (++cnt[x][y] > x) {
                s.insert(x);
            }
        }
        return s.size();
    }
};
```

#### Go

```go
func winningPlayerCount(n int, pick [][]int) int {
	cnt := make([][11]int, n)
	s := map[int]struct{}{}
	for _, p := range pick {
		x, y := p[0], p[1]
		cnt[x][y]++
		if cnt[x][y] > x {
			s[x] = struct{}{}
		}
	}
	return len(s)
}
```

#### TypeScript

```ts
function winningPlayerCount(n: number, pick: number[][]): number {
    const cnt: number[][] = Array.from({ length: n }, () => Array(11).fill(0));
    const s = new Set<number>();
    for (const [x, y] of pick) {
        if (++cnt[x][y] > x) {
            s.add(x);
        }
    }
    return s.size;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
