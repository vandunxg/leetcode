---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [554. Brick Wall](https://leetcode.com/problems/brick-wall)

[中文文档](/solution/0500-0599/0554.Brick%20Wall/README.md)

## Mô tả

<!-- description:start -->

<p>Trước mặt bạn là một bức tường gạch hình chữ nhật gồm <code>n</code> hàng. Ở hàng <code>i<sup>th</sup></code> có một số viên gạch cùng chiều cao (bằng một đơn vị), nhưng chiều rộng có thể khác nhau. Tổng chiều rộng của mỗi hàng bằng nhau.</p>

<p>Hãy kẻ một đường thẳng đứng từ trên xuống dưới sao cho cắt ít viên gạch nhất. Nếu đường thẳng đi qua mép viên gạch thì không tính là cắt viên đó. Không được kẻ đường trùng với một trong hai cạnh dọc của bức tường, vì khi đó hiển nhiên đường thẳng không cắt viên gạch nào.</p>

<p>Cho mảng 2D <code>wall</code> mô tả bức tường, hãy trả về <em>số viên gạch ít nhất bị đường thẳng đứng đó cắt</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0554.Brick%20Wall/images/a.png" style="width: 400px; height: 384px;" />
<pre>
<strong>Đầu vào:</strong> wall = [[1,2,2,1],[3,1,2],[1,3,2],[2,4],[3,1,2],[1,3,1,1]]
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> wall = [[1],[1],[1]]
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == wall.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= wall[i].length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= sum(wall[i].length) &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>sum(wall[i])</code> có cùng giá trị ở mọi hàng <code>i</code>.</li>
	<li><code>1 &lt;= wall[i][j] &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Đường thẳng đứng không cắt phần bên trong viên gạch sẽ cắt $(\text{số hàng} - \text{số hàng có mép gạch trùng với vị trí đó})$ viên. Đếm lại số hàng cho từng vị trí mép sẽ lặp lại công việc.
>
> Tính prefix sum cho từng hàng, không tính viên gạch cuối cùng, rồi đếm số lần xuất hiện của mỗi vị trí khe hở. Khe hở xuất hiện nhiều nhất là vị trí đường thẳng cắt ít viên gạch nhất; đáp án bằng số hàng trừ đi tần suất đó. Không xét hai cạnh ngoài của bức tường, nếu không đường thẳng sẽ không cắt viên nào.

<!-- thinking:end -->

Ta có thể dùng hash table $\textit{cnt}$ để ghi lại prefix sum của mỗi hàng, không tính viên gạch cuối cùng. Key là giá trị prefix sum, còn value là số lần prefix sum đó xuất hiện.

Duyệt từng hàng; với mỗi viên gạch ở hàng hiện tại, cộng chiều rộng của nó vào prefix sum hiện tại rồi cập nhật $\textit{cnt}$.

Cuối cùng, ta duyệt $\textit{cnt}$ để tìm prefix sum xuất hiện nhiều nhất, tương ứng với vị trí cắt ít viên gạch nhất. Đáp án là số hàng của bức tường trừ đi số viên gạch bị cắt.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số viên gạch trong bức tường.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def leastBricks(self, wall: List[List[int]]) -> int:
        cnt = Counter()
        for row in wall:
            s = 0
            for x in row[:-1]:
                s += x
                cnt[s] += 1
        return len(wall) - max(cnt.values(), default=0)
```

#### Java

```java
class Solution {
    public int leastBricks(List<List<Integer>> wall) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (var row : wall) {
            int s = 0;
            for (int i = 0; i + 1 < row.size(); ++i) {
                s += row.get(i);
                cnt.merge(s, 1, Integer::sum);
            }
        }
        int mx = 0;
        for (var x : cnt.values()) {
            mx = Math.max(mx, x);
        }
        return wall.size() - mx;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int leastBricks(vector<vector<int>>& wall) {
        unordered_map<int, int> cnt;
        for (const auto& row : wall) {
            int s = 0;
            for (int i = 0; i + 1 < row.size(); ++i) {
                s += row[i];
                cnt[s]++;
            }
        }
        int mx = 0;
        for (const auto& [_, x] : cnt) {
            mx = max(mx, x);
        }
        return wall.size() - mx;
    }
};
```

#### Go

```go
func leastBricks(wall [][]int) int {
	cnt := map[int]int{}
	for _, row := range wall {
		s := 0
		for _, x := range row[:len(row)-1] {
			s += x
			cnt[s]++
		}
	}
	mx := 0
	for _, x := range cnt {
		mx = max(mx, x)
	}
	return len(wall) - mx
}
```

#### TypeScript

```ts
function leastBricks(wall: number[][]): number {
    const cnt: Map<number, number> = new Map();
    for (const row of wall) {
        let s = 0;
        for (let i = 0; i + 1 < row.length; ++i) {
            s += row[i];
            cnt.set(s, (cnt.get(s) || 0) + 1);
        }
    }
    const mx = Math.max(...cnt.values(), 0);
    return wall.length - mx;
}
```

#### JavaScript

```js
/**
 * @param {number[][]} wall
 * @return {number}
 */
var leastBricks = function (wall) {
    const cnt = new Map();
    for (const row of wall) {
        let s = 0;
        for (let i = 0; i + 1 < row.length; ++i) {
            s += row[i];
            cnt.set(s, (cnt.get(s) || 0) + 1);
        }
    }
    const mx = Math.max(...cnt.values(), 0);
    return wall.length - mx;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
