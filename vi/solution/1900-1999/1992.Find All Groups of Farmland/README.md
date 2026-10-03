---
comments: true
difficulty: Medium
rating: 1539
source: Biweekly Contest 60 Q2
tags:
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [1992. Find All Groups of Farmland](https://leetcode.com/problems/find-all-groups-of-farmland)

[中文文档](/solution/1900-1999/1992.Find%20All%20Groups%20of%20Farmland/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận nhị phân <strong>đánh chỉ số từ 0</strong> <code>m x n</code> là <code>land</code>, trong đó <code>0</code> biểu thị một hecta đất rừng và <code>1</code> biểu thị một hecta đất nông nghiệp.</p>

<p>Để quy hoạch đất, có những khu vực hình chữ nhật gồm các hecta <strong>hoàn toàn</strong> là đất nông nghiệp. Những khu vực hình chữ nhật này được gọi là <strong>nhóm</strong>. Không có hai nhóm nào kề nhau, nghĩa là đất nông nghiệp trong một nhóm <strong>không kề theo bốn hướng</strong> với đất nông nghiệp thuộc một nhóm khác.</p>

<p>Có thể biểu diễn <code>land</code> bằng một hệ tọa độ, trong đó góc trên bên trái của <code>land</code> là <code>(0, 0)</code> và góc dưới bên phải của <code>land</code> là <code>(m-1, n-1)</code>. Hãy tìm tọa độ góc trên bên trái và góc dưới bên phải của mỗi <strong>nhóm</strong> đất nông nghiệp. Một <strong>nhóm</strong> đất nông nghiệp có góc trên bên trái tại <code>(r<sub>1</sub>, c<sub>1</sub>)</code> và góc dưới bên phải tại <code>(r<sub>2</sub>, c<sub>2</sub>)</code> được biểu diễn bằng mảng độ dài 4 <code>[r<sub>1</sub>, c<sub>1</sub>, r<sub>2</sub>, c<sub>2</sub>].</code></p>

<p>Trả về <em>một mảng 2D chứa các mảng độ dài 4 được mô tả ở trên cho mỗi <strong>nhóm</strong> đất nông nghiệp trong </em><code>land</code><em>. Nếu không có nhóm đất nông nghiệp nào, hãy trả về một mảng rỗng. Bạn có thể trả về đáp án theo <strong>bất kỳ thứ tự nào</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1992.Find%20All%20Groups%20of%20Farmland/images/screenshot-2021-07-27-at-12-23-15-copy-of-diagram-drawio-diagrams-net.png" style="width: 300px; height: 300px;" />
<pre>
<strong>Đầu vào:</strong> land = [[1,0,0],[0,1,1],[0,1,1]]
<strong>Đầu ra:</strong> [[0,0,0,0],[1,1,2,2]]
<strong>Giải thích:</strong>
Nhóm đầu tiên có góc trên bên trái tại land[0][0] và góc dưới bên phải cũng tại land[0][0].
Nhóm thứ hai có góc trên bên trái tại land[1][1] và góc dưới bên phải tại land[2][2].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1992.Find%20All%20Groups%20of%20Farmland/images/screenshot-2021-07-27-at-12-30-26-copy-of-diagram-drawio-diagrams-net.png" style="width: 200px; height: 200px;" />
<pre>
<strong>Đầu vào:</strong> land = [[1,1],[1,1]]
<strong>Đầu ra:</strong> [[0,0,1,1]]
<strong>Giải thích:</strong>
Nhóm đầu tiên có góc trên bên trái tại land[0][0] và góc dưới bên phải tại land[1][1].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1992.Find%20All%20Groups%20of%20Farmland/images/screenshot-2021-07-27-at-12-32-24-copy-of-diagram-drawio-diagrams-net.png" style="width: 100px; height: 100px;" />
<pre>
<strong>Đầu vào:</strong> land = [[0]]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong>
Không có nhóm đất nông nghiệp nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == land.length</code></li>
	<li><code>n == land[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 300</code></li>
	<li><code>land</code> chỉ gồm các giá trị <code>0</code> và <code>1</code>.</li>
	<li>Các nhóm đất nông nghiệp có dạng <strong>hình chữ nhật</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi khu đất nông nghiệp là một hình chữ nhật đặc. Có thể dùng BFS, nhưng ta có thể xác định các góc từ đường biên.
>
> Một ô có ô phía trên và bên trái không phải đất nông nghiệp là góc trên bên trái; sau đó ta mở rộng xuống dưới và sang phải đến góc đối diện rồi ghi nhận hình chữ nhật đó đúng một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findFarmland(self, land: List[List[int]]) -> List[List[int]]:
        m, n = len(land), len(land[0])
        ans = []
        for i in range(m):
            for j in range(n):
                if (
                    land[i][j] == 0
                    or (j > 0 and land[i][j - 1] == 1)
                    or (i > 0 and land[i - 1][j] == 1)
                ):
                    continue
                x, y = i, j
                while x + 1 < m and land[x + 1][j] == 1:
                    x += 1
                while y + 1 < n and land[x][y + 1] == 1:
                    y += 1
                ans.append([i, j, x, y])
        return ans
```

#### Java

```java
class Solution {
    public int[][] findFarmland(int[][] land) {
        List<int[]> ans = new ArrayList<>();
        int m = land.length;
        int n = land[0].length;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (land[i][j] == 0 || (j > 0 && land[i][j - 1] == 1)
                    || (i > 0 && land[i - 1][j] == 1)) {
                    continue;
                }
                int x = i;
                int y = j;
                for (; x + 1 < m && land[x + 1][j] == 1; ++x)
                    ;
                for (; y + 1 < n && land[x][y + 1] == 1; ++y)
                    ;
                ans.add(new int[] {i, j, x, y});
            }
        }
        return ans.toArray(new int[ans.size()][4]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> findFarmland(vector<vector<int>>& land) {
        vector<vector<int>> ans;
        int m = land.size();
        int n = land[0].size();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (land[i][j] == 0 || (j > 0 && land[i][j - 1] == 1) || (i > 0 && land[i - 1][j] == 1)) continue;
                int x = i;
                int y = j;
                for (; x + 1 < m && land[x + 1][j] == 1; ++x)
                    ;
                for (; y + 1 < n && land[x][y + 1] == 1; ++y)
                    ;
                ans.push_back({i, j, x, y});
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findFarmland(land [][]int) [][]int {
	m, n := len(land), len(land[0])
	var ans [][]int
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if land[i][j] == 0 || (j > 0 && land[i][j-1] == 1) || (i > 0 && land[i-1][j] == 1) {
				continue
			}
			x, y := i, j
			for ; x+1 < m && land[x+1][j] == 1; x++ {
			}
			for ; y+1 < n && land[x][y+1] == 1; y++ {
			}
			ans = append(ans, []int{i, j, x, y})
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
