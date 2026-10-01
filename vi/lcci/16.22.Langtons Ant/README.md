---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [16.22. Langtons Ant](https://leetcode.cn/problems/langtons-ant-lcci)

[中文文档](/lcci/16.22.Langtons%20Ant/README.md)

## Mô tả

<!-- description:start -->

<p>Một con kiến đang đứng trên một lưới vô hạn gồm các ô trắng và đen. Ban đầu nó hướng sang phải. Ban đầu tất cả các ô đều màu trắng.</p>
<p>Ở mỗi bước, nó thực hiện các thao tác sau:</p>
<p>(1) Ở một ô trắng, đổi màu ô, quay 90 độ sang phải (theo chiều kim đồng hồ), rồi tiến về phía trước một đơn vị.</p>
<p>(2) Ở một ô đen, đổi màu ô, quay 90 độ sang trái (ngược chiều kim đồng hồ), rồi tiến về phía trước một đơn vị.</p>
<p>Hãy viết chương trình mô phỏng K bước di chuyển đầu tiên của con kiến và in bàn cờ cuối cùng dưới dạng một lưới.</p>
<p>Lưới được biểu diễn dưới dạng một mảng các chuỗi, trong đó mỗi phần tử biểu diễn một hàng của lưới. Ô đen được biểu diễn bằng <code>&#39;X&#39;</code>, ô trắng được biểu diễn bằng <code>&#39;_&#39;</code>, còn ô mà con kiến đang đứng được biểu diễn bằng <code>&#39;L&#39;</code>, <code>&#39;U&#39;</code>, <code>&#39;R&#39;</code>, <code>&#39;D&#39;</code>, lần lượt biểu thị hướng sang trái, lên trên, sang phải và xuống dưới. Bạn chỉ cần trả về ma trận nhỏ nhất có thể chứa tất cả các ô mà con kiến đã đi qua.</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong>Đầu vào:</strong> 0

<strong>Đầu ra: </strong>[&quot;R&quot;]

</pre>
<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong>Đầu vào:</strong> 2

<strong>Đầu ra:

</strong>[

&nbsp; &quot;\_X&quot;,

&nbsp; &quot;LX&quot;

]

</pre>
<p><strong>Ví dụ 3:</strong></p>
<pre>

<strong>Đầu vào:</strong> 5

<strong>Đầu ra:

</strong>[

&nbsp; &quot;\_U&quot;,

&nbsp; &quot;X\_&quot;,

&nbsp; &quot;XX&quot;

]

</pre>
<p><strong>Lưu ý: </strong></p>
<ul>
	<li><code>K &lt;= 100000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Kiến Langton di chuyển $K$ bước trên một lưới trắng vô hạn. Một mảng cố định không thể biết được khung bao.
>
> Chỉ các ô đen cần được lưu trữ, do đó dùng một set, cùng với các tọa độ min/max được cập nhật để cắt vùng cuối cùng.
>
> Ô trắng chuyển thành đen và quay phải; ô đen chuyển thành trắng và quay trái; sau đó di chuyển theo $dirs$. Tô `X` lên các ô đen và ghi chữ cái chỉ hướng lên con kiến.

<!-- thinking:end -->

Chúng ta dùng một hash table `black` để ghi lại vị trí của tất cả các ô đen, và một hash table `dirs` để ghi lại bốn hướng của con kiến. Chúng ta dùng các biến $x$, $y$ để ghi lại vị trí của con kiến, và biến $p$ để ghi lại hướng của con kiến. Chúng ta dùng các biến $x_1$, $y_1$, $x_2$, $y_2$ để ghi lại tọa độ x nhỏ nhất, tọa độ y nhỏ nhất, tọa độ x lớn nhất và tọa độ y lớn nhất của tất cả các ô đen.

Chúng ta mô phỏng quá trình di chuyển của con kiến. Nếu ô mà con kiến đang đứng có màu trắng, con kiến quay phải $90$ độ, tô ô đó thành màu đen rồi tiến về phía trước một đơn vị. Nếu ô mà con kiến đang đứng có màu đen, con kiến quay trái $90$ độ, tô ô đó thành màu trắng rồi tiến về phía trước một đơn vị. Trong quá trình mô phỏng, chúng ta liên tục cập nhật các giá trị của $x_1$, $y_1$, $x_2$, $y_2$ để chúng bao phủ tất cả các ô mà con kiến đã đi qua.

Sau khi mô phỏng, chúng ta xây dựng ma trận đáp án $g$ dựa trên các giá trị $x_1$, $y_1$, $x_2$, $y_2$. Sau đó, chúng ta ghi hướng của con kiến tại vị trí của nó, ghi $X$ lên tất cả các ô đen, và cuối cùng trả về ma trận đáp án.

Độ phức tạp thời gian là $O(K)$, và độ phức tạp không gian là $O(K)$. Trong đó, $K$ là số bước mà con kiến di chuyển.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def printKMoves(self, K: int) -> List[str]:
        x1 = y1 = x2 = y2 = 0
        dirs = (0, 1, 0, -1, 0)
        d = "RDLU"
        x = y = 0
        p = 0
        black = set()
        for _ in range(K):
            if (x, y) in black:
                black.remove((x, y))
                p = (p + 3) % 4
            else:
                black.add((x, y))
                p = (p + 1) % 4
            x += dirs[p]
            y += dirs[p + 1]
            x1 = min(x1, x)
            y1 = min(y1, y)
            x2 = max(x2, x)
            y2 = max(y2, y)
        m, n = x2 - x1 + 1, y2 - y1 + 1
        g = [["_"] * n for _ in range(m)]
        for i, j in black:
            g[i - x1][j - y1] = "X"
        g[x - x1][y - y1] = d[p]
        return ["".join(row) for row in g]
```

#### Java

```java
class Solution {
    public List<String> printKMoves(int K) {
        int x1 = 0, y1 = 0, x2 = 0, y2 = 0;
        int[] dirs = {0, 1, 0, -1, 0};
        String d = "RDLU";
        int x = 0, y = 0, p = 0;
        Set<List<Integer>> black = new HashSet<>();
        while (K-- > 0) {
            List<Integer> t = List.of(x, y);
            if (black.add(t)) {
                p = (p + 1) % 4;
            } else {
                black.remove(t);
                p = (p + 3) % 4;
            }
            x += dirs[p];
            y += dirs[p + 1];
            x1 = Math.min(x1, x);
            y1 = Math.min(y1, y);
            x2 = Math.max(x2, x);
            y2 = Math.max(y2, y);
        }
        int m = x2 - x1 + 1;
        int n = y2 - y1 + 1;
        char[][] g = new char[m][n];
        for (char[] row : g) {
            Arrays.fill(row, '_');
        }
        for (List<Integer> t : black) {
            int i = t.get(0) - x1;
            int j = t.get(1) - y1;
            g[i][j] = 'X';
        }
        g[x - x1][y - y1] = d.charAt(p);
        List<String> ans = new ArrayList<>();
        for (char[] row : g) {
            ans.add(String.valueOf(row));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> printKMoves(int K) {
        int x1 = 0, y1 = 0, x2 = 0, y2 = 0;
        int dirs[5] = {0, 1, 0, -1, 0};
        string d = "RDLU";
        int x = 0, y = 0, p = 0;
        set<pair<int, int>> black;
        while (K--) {
            auto t = make_pair(x, y);
            if (black.count(t)) {
                black.erase(t);
                p = (p + 3) % 4;
            } else {
                black.insert(t);
                p = (p + 1) % 4;
            }
            x += dirs[p];
            y += dirs[p + 1];
            x1 = min(x1, x);
            y1 = min(y1, y);
            x2 = max(x2, x);
            y2 = max(y2, y);
        }
        int m = x2 - x1 + 1, n = y2 - y1 + 1;
        vector<string> g(m, string(n, '_'));
        for (auto& [i, j] : black) {
            g[i - x1][j - y1] = 'X';
        }
        g[x - x1][y - y1] = d[p];
        return g;
    }
};
```

#### Go

```go
func printKMoves(K int) []string {
	var x1, y1, x2, y2, x, y, p int
	dirs := [5]int{0, 1, 0, -1, 0}
	d := "RDLU"
	type pair struct{ x, y int }
	black := map[pair]bool{}
	for K > 0 {
		t := pair{x, y}
		if black[t] {
			delete(black, t)
			p = (p + 3) % 4
		} else {
			black[t] = true
			p = (p + 1) % 4
		}
		x += dirs[p]
		y += dirs[p+1]
		x1 = min(x1, x)
		y1 = min(y1, y)
		x2 = max(x2, x)
		y2 = max(y2, y)
		K--
	}
	m, n := x2-x1+1, y2-y1+1
	g := make([][]byte, m)
	for i := range g {
		g[i] = make([]byte, n)
		for j := range g[i] {
			g[i][j] = '_'
		}
	}
	for t := range black {
		i, j := t.x-x1, t.y-y1
		g[i][j] = 'X'
	}
	g[x-x1][y-y1] = d[p]
	ans := make([]string, m)
	for i := range ans {
		ans[i] = string(g[i])
	}
	return ans
}
```

#### Swift

```swift
class Solution {
    func printKMoves(_ K: Int) -> [String] {
        var x1 = 0, y1 = 0, x2 = 0, y2 = 0
        let dirs = [0, 1, 0, -1, 0]
        let d = "RDLU"
        var x = 0, y = 0, p = 0
        var black = Set<[Int]>()
        var K = K

        while K > 0 {
            let t = [x, y]
            if black.insert(t).inserted {
                p = (p + 1) % 4
            } else {
                black.remove(t)
                p = (p + 3) % 4
            }
            x += dirs[p]
            y += dirs[p + 1]
            x1 = min(x1, x)
            y1 = min(y1, y)
            x2 = max(x2, x)
            y2 = max(y2, y)
            K -= 1
        }

        let m = x2 - x1 + 1
        let n = y2 - y1 + 1
        var g = Array(repeating: Array(repeating: "_", count: n), count: m)

        for t in black {
            let i = t[0] - x1
            let j = t[1] - y1
            g[i][j] = "X"
        }

        g[x - x1][y - y1] = String(d[d.index(d.startIndex, offsetBy: p)])

        return g.map { $0.joined() }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
