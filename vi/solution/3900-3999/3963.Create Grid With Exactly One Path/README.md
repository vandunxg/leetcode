---
comments: true
difficulty: Easy
rating: 1310
source: Biweekly Contest 185 Q1
---

<!-- problem:start -->

# [3963. Create Grid With Exactly One Path](https://leetcode.com/problems/create-grid-with-exactly-one-path)

[中文文档](/solution/3900-3999/3963.Create%20Grid%20With%20Exactly%20One%20Path/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <code>m</code> và <code>n</code>, lần lượt biểu thị số hàng và số cột của một lưới.</p>

<p>Hãy xây dựng <strong>bất kỳ</strong> lưới <code>m x n</code> nào chỉ gồm các ký tự <code>&#39;.&#39;</code> và <code>&#39;#&#39;</code>, trong đó:</p>

<ul>
	<li><code>&#39;.&#39;</code> biểu thị một ô trống.</li>
	<li><code>&#39;#&#39;</code> biểu thị một ô chướng ngại vật.</li>
</ul>

<p>Một <strong>đường đi hợp lệ</strong> là một dãy các ô trống:</p>

<ul>
	<li>Bắt đầu tại ô góc trên bên trái <code>(0, 0)</code>.</li>
	<li>Kết thúc tại ô góc dưới bên phải <code>(m - 1, n - 1)</code>.</li>
	<li>Chỉ di chuyển:
	<ul>
		<li>Sang phải, từ <code>(i, j)</code> đến <code>(i, j + 1)</code>, hoặc</li>
		<li>Xuống dưới, từ <code>(i, j)</code> đến <code>(i + 1, j)</code>.</li>
	</ul>
	</li>
</ul>

<p>Trả về bất kỳ lưới nào sao cho có <strong>chính xác một đường đi hợp lệ</strong> từ ô góc trên bên trái đến ô góc dưới bên phải.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 2, n = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;..#&quot;,&quot;#..&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3963.Create%20Grid%20With%20Exactly%20One%20Path/images/screenshot-2026-05-26-at-61005pm.png" style="width: 200px; height: 95px;" /></p>

<p>Đường đi hợp lệ duy nhất là: <code>(0,0) &rarr; (0,1) &rarr; (1,1) &rarr; (1,2)</code></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 3, n = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;..#&quot;,&quot;#..&quot;,&quot;##.&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3963.Create%20Grid%20With%20Exactly%20One%20Path/images/screenshot-2026-05-26-at-61129pm.png" style="width: 220px; height: 150px;" /></p>

<p>Đường đi hợp lệ duy nhất là: <code>(0,0) &rarr; (0,1) &rarr; (1,1) &rarr; (1,2) &rarr; (2,2)</code></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 1, n = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;....&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi hợp lệ duy nhất là: <code>(0,0) &rarr; (0,1) &rarr; (0,2) &rarr; (0,3)</code></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 25</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Xây dựng

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ cần có đúng một đường đi chỉ gồm các bước xuống hoặc sang phải từ góc trên bên trái đến góc dưới bên phải. Điền toàn bộ lưới bằng tường, sau đó mở hàng đầu tiên và cột cuối cùng, tạo thành đường gấp khúc duy nhất “đi ngang trên cùng rồi đi xuống bên phải”.
>
> $m,n\le 25$, nên việc xây dựng có độ phức tạp tuyến tính theo kích thước lưới.

<!-- thinking:end -->

Ta xây dựng lưới như sau:

- Trước tiên, tạo một lưới được điền hoàn toàn bằng `#`.
- Đặt tất cả phần tử trong hàng đầu tiên thành `.`.
- Đặt tất cả phần tử trong cột cuối cùng thành `.`.
- Trả về lưới đã xây dựng.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của lưới.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def createGrid(self, m: int, n: int) -> list[str]:
        g = [["#"] * n for _ in range(m)]
        g[0] = ["."] * n
        for i in range(m):
            g[i][-1] = "."
        return ["".join(row) for row in g]
```

#### Java

```java
class Solution {
    public String[] createGrid(int m, int n) {
        char[][] g = new char[m][n];
        for (int i = 0; i < m; i++) {
            Arrays.fill(g[i], '#');
        }

        Arrays.fill(g[0], '.');

        for (int i = 0; i < m; i++) {
            g[i][n - 1] = '.';
        }

        String[] ans = new String[m];
        for (int i = 0; i < m; i++) {
            ans[i] = new String(g[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> createGrid(int m, int n) {
        vector<string> g(m, string(n, '#'));

        g[0] = string(n, '.');

        for (int i = 0; i < m; i++) {
            g[i][n - 1] = '.';
        }

        return g;
    }
};
```

#### Go

```go
func createGrid(m int, n int) []string {
	g := make([][]byte, m)
	for i := range g {
		g[i] = make([]byte, n)
		for j := range g[i] {
			g[i][j] = '#'
		}
	}

	for j := 0; j < n; j++ {
		g[0][j] = '.'
	}

	for i := 0; i < m; i++ {
		g[i][n-1] = '.'
	}

	ans := make([]string, m)
	for i := range g {
		ans[i] = string(g[i])
	}
	return ans
}
```

#### TypeScript

```ts
function createGrid(m: number, n: number): string[] {
    const g: string[][] = Array.from({ length: m }, () => Array(n).fill('#'));

    g[0].fill('.');

    for (let i = 0; i < m; i++) {
        g[i][n - 1] = '.';
    }

    return g.map(row => row.join(''));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
