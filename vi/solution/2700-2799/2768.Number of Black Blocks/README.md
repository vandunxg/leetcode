---
comments: true
difficulty: Medium
rating: 2175
source: Biweekly Contest 108 Q4
tags:
    - Array
    - Hash Table
    - Enumeration
---

<!-- problem:start -->

# [2768. Number of Black Blocks](https://leetcode.com/problems/number-of-black-blocks)

[Tài liệu tiếng Trung](/solution/2700-2799/2768.Number%20of%20Black%20Blocks/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <code>m</code> và <code>n</code> biểu diễn kích thước của một lưới <code>m x n</code> được đánh chỉ số từ <strong>0</strong>.</p>

<p>Bạn cũng được cho một ma trận số nguyên 2D <code>coordinates</code> được đánh chỉ số từ <strong>0</strong>, trong đó <code>coordinates[i] = [x, y]</code> cho biết ô có tọa độ <code>[x, y]</code> được tô <strong>đen</strong>. Tất cả các ô trong lưới không xuất hiện trong <code>coordinates</code> đều có màu <strong>trắng</strong>.</p>

<p>Một khối được định nghĩa là một ma trận con <code>2 x 2</code> của lưới. Cụ thể hơn, một khối có ô <code>[x, y]</code> là ô trên cùng bên trái, với <code>0 &lt;= x &lt; m - 1</code> và <code>0 &lt;= y &lt; n - 1</code>, sẽ chứa các tọa độ <code>[x, y]</code>, <code>[x + 1, y]</code>, <code>[x, y + 1]</code> và <code>[x + 1, y + 1]</code>.</p>

<p>Trả về <em>một mảng số nguyên được đánh chỉ số từ <strong>0</strong></em> <code>arr</code> <em>có kích thước</em> <code>5</code> <em>sao cho</em> <code>arr[i]</code> <em>là số khối chứa chính xác</em> <code>i</code> <em>ô <strong>đen</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> m = 3, n = 3, coordinates = [[0,0]]
<strong>Đầu ra:</strong> [3,1,0,0,0]
<strong>Giải thích:</strong> Lưới trông như sau:
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2768.Number%20of%20Black%20Blocks/images/screen-shot-2023-06-18-at-44656-am.png" style="width: 150px; height: 128px;" />
Chỉ có 1 khối chứa một ô đen, đó là khối bắt đầu tại ô [0,0].
3 khối còn lại bắt đầu tại các ô [0,1], [1,0] và [1,1]. Tất cả đều không có ô đen.
Vì vậy, ta trả về [3,1,0,0,0].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> m = 3, n = 3, coordinates = [[0,0],[1,1],[0,2]]
<strong>Đầu ra:</strong> [0,2,2,0,0]
<strong>Giải thích:</strong> Lưới trông như sau:
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2768.Number%20of%20Black%20Blocks/images/screen-shot-2023-06-18-at-45018-am.png" style="width: 150px; height: 128px;" />
Có 2 khối chứa hai ô đen, đó là các khối bắt đầu tại tọa độ [0,0] và [0,1].
2 khối còn lại bắt đầu tại các tọa độ [1,0] và [1,1]. Cả hai đều có 1 ô đen.
Do đó, ta trả về [0,2,2,0,0].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= m &lt;= 10<sup>5</sup></code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= coordinates.length &lt;= 10<sup>4</sup></code></li>
	<li><code>coordinates[i].length == 2</code></li>
	<li><code>0 &lt;= coordinates[i][0] &lt; m</code></li>
	<li><code>0 &lt;= coordinates[i][1] &lt; n</code></li>
	<li>Đảm bảo rằng <code>coordinates</code> chứa các tọa độ đôi một khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số khối $2\times 2$ chứa từ $0$ đến $4$ ô đen. Có $(m-1)(n-1)$ khối và mỗi chiều của lưới có thể lên đến $10^5$, nên không thể duyệt qua tất cả các khối.
>
> Một ô đen thuộc tối đa bốn khối. Tăng số đếm của các khối đó trong một hash map, điền $ans[1..4]$ từ các giá trị trong map, rồi đặt $ans[0]$ bằng tổng số khối trừ đi kích thước của map.

<!-- thinking:end -->

Với mỗi ma trận con $2 \times 2$, ta có thể dùng tọa độ ô trên cùng bên trái $(x, y)$ để biểu diễn nó.

Mỗi ô đen $(x, y)$ đóng góp $1$ vào 4 ma trận con, cụ thể là các ma trận $(x - 1, y - 1)$, $(x - 1, y)$, $(x, y - 1)$ và $(x, y)$.

Vì vậy, ta duyệt qua tất cả các ô đen, sau đó cộng dồn số ô đen trong mỗi ma trận con và lưu trong hash table $cnt$.

Cuối cùng, ta duyệt qua tất cả các giá trị trong $cnt$ (lớn hơn $0$), đếm số lần mỗi giá trị xuất hiện và lưu vào mảng kết quả $ans$. Trong đó, $ans[0]$ biểu diễn số ma trận con không có ô đen, có giá trị là $(m - 1) \times (n - 1) - \sum_{i = 1}^4 ans[i]$.

Độ phức tạp thời gian là $O(l)$, độ phức tạp không gian là $O(l)$, trong đó $l$ là độ dài của $coordinates$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countBlackBlocks(
        self, m: int, n: int, coordinates: List[List[int]]
    ) -> List[int]:
        cnt = Counter()
        for x, y in coordinates:
            for a, b in pairwise((0, 0, -1, -1, 0)):
                i, j = x + a, y + b
                if 0 <= i < m - 1 and 0 <= j < n - 1:
                    cnt[(i, j)] += 1
        ans = [0] * 5
        for x in cnt.values():
            ans[x] += 1
        ans[0] = (m - 1) * (n - 1) - len(cnt.values())
        return ans
```

#### Java

```java
class Solution {
    public long[] countBlackBlocks(int m, int n, int[][] coordinates) {
        Map<Long, Integer> cnt = new HashMap<>(coordinates.length);
        int[] dirs = {0, 0, -1, -1, 0};
        for (var e : coordinates) {
            int x = e[0], y = e[1];
            for (int k = 0; k < 4; ++k) {
                int i = x + dirs[k], j = y + dirs[k + 1];
                if (i >= 0 && i < m - 1 && j >= 0 && j < n - 1) {
                    cnt.merge(1L * i * n + j, 1, Integer::sum);
                }
            }
        }
        long[] ans = new long[5];
        ans[0] = (m - 1L) * (n - 1);
        for (int x : cnt.values()) {
            ++ans[x];
            --ans[0];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> countBlackBlocks(int m, int n, vector<vector<int>>& coordinates) {
        unordered_map<long long, int> cnt;
        int dirs[5] = {0, 0, -1, -1, 0};
        for (auto& e : coordinates) {
            int x = e[0], y = e[1];
            for (int k = 0; k < 4; ++k) {
                int i = x + dirs[k], j = y + dirs[k + 1];
                if (i >= 0 && i < m - 1 && j >= 0 && j < n - 1) {
                    ++cnt[1LL * i * n + j];
                }
            }
        }
        vector<long long> ans(5);
        ans[0] = (m - 1LL) * (n - 1);
        for (auto& [_, x] : cnt) {
            ++ans[x];
            --ans[0];
        }
        return ans;
    }
};
```

#### Go

```go
func countBlackBlocks(m int, n int, coordinates [][]int) []int64 {
	cnt := map[int64]int{}
	dirs := [5]int{0, 0, -1, -1, 0}
	for _, e := range coordinates {
		x, y := e[0], e[1]
		for k := 0; k < 4; k++ {
			i, j := x+dirs[k], y+dirs[k+1]
			if i >= 0 && i < m-1 && j >= 0 && j < n-1 {
				cnt[int64(i*n+j)]++
			}
		}
	}
	ans := make([]int64, 5)
	ans[0] = int64((m - 1) * (n - 1))
	for _, x := range cnt {
		ans[x]++
		ans[0]--
	}
	return ans
}
```

#### TypeScript

```ts
function countBlackBlocks(m: number, n: number, coordinates: number[][]): number[] {
    const cnt: Map<number, number> = new Map();
    const dirs: number[] = [0, 0, -1, -1, 0];
    for (const [x, y] of coordinates) {
        for (let k = 0; k < 4; ++k) {
            const [i, j] = [x + dirs[k], y + dirs[k + 1]];
            if (i >= 0 && i < m - 1 && j >= 0 && j < n - 1) {
                const key = i * n + j;
                cnt.set(key, (cnt.get(key) || 0) + 1);
            }
        }
    }
    const ans: number[] = Array(5).fill(0);
    ans[0] = (m - 1) * (n - 1);
    for (const [_, x] of cnt) {
        ++ans[x];
        --ans[0];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
