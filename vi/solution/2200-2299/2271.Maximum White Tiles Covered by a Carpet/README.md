---
comments: true
difficulty: Medium
rating: 2021
source: Biweekly Contest 78 Q3
tags:
    - Greedy
    - Array
    - Binary Search
    - Prefix Sum
    - Sorting
    - Sliding Window
---

<!-- problem:start -->

# [2271. Maximum White Tiles Covered by a Carpet](https://leetcode.com/problems/maximum-white-tiles-covered-by-a-carpet)

[中文文档](/solution/2200-2299/2271.Maximum%20White%20Tiles%20Covered%20by%20a%20Carpet/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <code>tiles</code>, trong đó <code>tiles[i] = [l<sub>i</sub>, r<sub>i</sub>]</code> biểu diễn rằng mọi ô <code>j</code> trong đoạn <code>l<sub>i</sub> &lt;= j &lt;= r<sub>i</sub></code> đều được tô màu trắng.</p>

<p>Đồng thời, cho một số nguyên <code>carpetLen</code> là độ dài của một tấm thảm có thể được đặt <strong>ở bất kỳ vị trí nào</strong>.</p>

<p>Hãy trả về <em>số ô trắng <strong>lớn nhất</strong> có thể được tấm thảm phủ lên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2271.Maximum%20White%20Tiles%20Covered%20by%20a%20Carpet/images/example1drawio3.png" style="width: 644px; height: 158px;" />
<pre>
<strong>Đầu vào:</strong> tiles = [[1,5],[10,11],[12,18],[20,25],[30,32]], carpetLen = 10
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Đặt tấm thảm bắt đầu tại ô 10.
Tấm thảm phủ được 9 ô trắng, nên ta trả về 9.
Lưu ý rằng có thể có những vị trí khác mà tấm thảm cũng phủ được 9 ô trắng.
Có thể chứng minh rằng tấm thảm không thể phủ được nhiều hơn 9 ô trắng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2271.Maximum%20White%20Tiles%20Covered%20by%20a%20Carpet/images/example2drawio.png" style="width: 231px; height: 168px;" />
<pre>
<strong>Đầu vào:</strong> tiles = [[10,11],[1,1]], carpetLen = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Đặt tấm thảm bắt đầu tại ô 10.
Tấm thảm phủ được 2 ô trắng, nên ta trả về 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= tiles.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>tiles[i].length == 2</code></li>
	<li><code>1 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= carpetLen &lt;= 10<sup>9</sup></code></li>
	<li>Các <code>tiles</code> <strong>không chồng lấn</strong> lên nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một tấm thảm có độ dài $\textit{carpetLen}$ có thể phủ lên nhiều đoạn ô không giao nhau. Đặt đầu trái của tấm thảm bên trong một đoạn ô không bao giờ tốt hơn việc căn nó với đầu trái của một đoạn ô, vì vậy ta chỉ cần thử các vị trí đó.
>
> Sắp xếp các đoạn tiles theo đầu trái. Một con trỏ $j$ giữ đoạn ô xa nhất đã được phủ hoàn toàn, còn $s$ là tổng độ dài của chúng. Đoạn ô tiếp theo nếu chỉ được phủ một phần sẽ đóng góp $li+\textit{carpetLen}-tiles[j][0]$. $j$ chỉ di chuyển về phía trước.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumWhiteTiles(self, tiles: List[List[int]], carpetLen: int) -> int:
        tiles.sort()
        n = len(tiles)
        s = ans = j = 0
        for i, (li, ri) in enumerate(tiles):
            while j < n and tiles[j][1] - li + 1 <= carpetLen:
                s += tiles[j][1] - tiles[j][0] + 1
                j += 1
            if j < n and li + carpetLen > tiles[j][0]:
                ans = max(ans, s + li + carpetLen - tiles[j][0])
            else:
                ans = max(ans, s)
            s -= ri - li + 1
        return ans
```

#### Java

```java
class Solution {
    public int maximumWhiteTiles(int[][] tiles, int carpetLen) {
        Arrays.sort(tiles, (a, b) -> a[0] - b[0]);
        int n = tiles.length;
        int s = 0, ans = 0;
        for (int i = 0, j = 0; i < n; ++i) {
            while (j < n && tiles[j][1] - tiles[i][0] + 1 <= carpetLen) {
                s += tiles[j][1] - tiles[j][0] + 1;
                ++j;
            }
            if (j < n && tiles[i][0] + carpetLen > tiles[j][0]) {
                ans = Math.max(ans, s + tiles[i][0] + carpetLen - tiles[j][0]);
            } else {
                ans = Math.max(ans, s);
            }
            s -= (tiles[i][1] - tiles[i][0] + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumWhiteTiles(vector<vector<int>>& tiles, int carpetLen) {
        sort(tiles.begin(), tiles.end());
        int s = 0, ans = 0, n = tiles.size();
        for (int i = 0, j = 0; i < n; ++i) {
            while (j < n && tiles[j][1] - tiles[i][0] + 1 <= carpetLen) {
                s += tiles[j][1] - tiles[j][0] + 1;
                ++j;
            }
            if (j < n && tiles[i][0] + carpetLen > tiles[j][0]) {
                ans = max(ans, s + tiles[i][0] + carpetLen - tiles[j][0]);
            } else {
                ans = max(ans, s);
            }
            s -= (tiles[i][1] - tiles[i][0] + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumWhiteTiles(tiles [][]int, carpetLen int) int {
	sort.Slice(tiles, func(i, j int) bool { return tiles[i][0] < tiles[j][0] })
	n := len(tiles)
	s, ans := 0, 0
	for i, j := 0, 0; i < n; i++ {
		for j < n && tiles[j][1]-tiles[i][0]+1 <= carpetLen {
			s += tiles[j][1] - tiles[j][0] + 1
			j++
		}
		if j < n && tiles[i][0]+carpetLen > tiles[j][0] {
			ans = max(ans, s+tiles[i][0]+carpetLen-tiles[j][0])
		} else {
			ans = max(ans, s)
		}
		s -= (tiles[i][1] - tiles[i][0] + 1)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
