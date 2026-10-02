---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Matrix
---

<!-- problem:start -->

# [533. Lonely Pixel II 🔒](https://leetcode.com/problems/lonely-pixel-ii)

[中文文档](/solution/0500-0599/0533.Lonely%20Pixel%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <code>picture</code> kích thước <code>m x n</code> gồm các pixel đen <code>&#39;B&#39;</code>, pixel trắng <code>&#39;W&#39;</code> và số nguyên target; hãy trả về <em>số lượng pixel đen <b>cô độc</b></em>.</p>

<p>Pixel đen cô độc là ký tự <code>&#39;B&#39;</code> ở vị trí <code>(r, c)</code> thỏa mãn:</p>

<ul>
	<li>Hàng <code>r</code> và cột <code>c</code> đều có đúng <code>target</code> pixel đen.</li>
	<li>Mọi hàng có pixel đen ở cột <code>c</code> đều phải giống hệt hàng <code>r</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0533.Lonely%20Pixel%20II/images/pixel2-1-grid.jpg" style="width: 493px; height: 333px;" />
<pre>
<strong>Đầu vào:</strong> picture = [[&quot;W&quot;,&quot;B&quot;,&quot;W&quot;,&quot;B&quot;,&quot;B&quot;,&quot;W&quot;],[&quot;W&quot;,&quot;B&quot;,&quot;W&quot;,&quot;B&quot;,&quot;B&quot;,&quot;W&quot;],[&quot;W&quot;,&quot;B&quot;,&quot;W&quot;,&quot;B&quot;,&quot;B&quot;,&quot;W&quot;],[&quot;W&quot;,&quot;W&quot;,&quot;B&quot;,&quot;W&quot;,&quot;B&quot;,&quot;W&quot;]], target = 3
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Các ký tự &#39;B&#39; màu xanh là những pixel đen cần tìm (tất cả &#39;B&#39; ở cột 1 và 3).
Lấy &#39;B&#39; ở hàng r = 0, cột c = 1 làm ví dụ:
 - Quy tắc 1: hàng r = 0 và cột c = 1 đều có đúng target = 3 pixel đen. 
 - Quy tắc 2: các hàng có pixel đen ở cột c = 1 là hàng 0, hàng 1 và hàng 2. Chúng giống hệt hàng r = 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0533.Lonely%20Pixel%20II/images/pixel2-2-grid.jpg" style="width: 253px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> picture = [[&quot;W&quot;,&quot;W&quot;,&quot;B&quot;],[&quot;W&quot;,&quot;W&quot;,&quot;B&quot;],[&quot;W&quot;,&quot;W&quot;,&quot;B&quot;]], target = 1
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m ==&nbsp;picture.length</code></li>
	<li><code>n ==&nbsp;picture[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>picture[i][j]</code> là <code>&#39;W&#39;</code> hoặc <code>&#39;B&#39;</code>.</li>
	<li><code>1 &lt;= target &lt;= min(m, n)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ngoài điều kiện hàng và cột đều có đúng $target$ ký tự `B`, mọi hàng có `B` trong cột đó phải giống hệt nhau. Quét lại các hàng cho từng cột sẽ lặp công việc.
>
> Đếm pixel đen theo từng hàng và lưu các chỉ số hàng theo từng cột. Với mỗi cột, chọn một hàng làm mẫu: nếu số hàng có pixel đen ở cột đó bằng $target$ và mọi hàng được liệt kê đều giống hàng mẫu, cộng $target$ vào kết quả. So sánh cả hàng giúp tránh phải viết lại phép so sánh từng ô.

<!-- thinking:end -->

Điều kiện thứ hai của đề bài tương đương với yêu cầu: với mỗi cột có pixel đen, tất cả các hàng chứa pixel đen trong cột đó phải giống hệt nhau.

Vì vậy, ta có thể dùng adjacency list $g$ để lưu các hàng có pixel đen trong từng cột; cụ thể, $g[j]$ là tập các hàng có pixel đen ở cột thứ $j$. Ngoài ra, dùng mảng $rows$ để lưu số pixel đen trong mỗi hàng.

Tiếp theo, duyệt từng cột. Với mỗi cột, tìm hàng đầu tiên $i_1$ có pixel đen. Nếu hàng này không có đúng $target$ pixel đen thì cột đó không thể chứa pixel cô độc, nên bỏ qua. Nếu có, kiểm tra mọi hàng chứa pixel đen trong cột có giống hệt hàng thứ $i_1$ hay không. Nếu đúng, tất cả pixel đen trong cột này đều cô độc, nên cộng $target$ vào đáp án.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(m \times n^2)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findBlackPixel(self, picture: List[List[str]], target: int) -> int:
        rows = [0] * len(picture)
        g = defaultdict(list)
        for i, row in enumerate(picture):
            for j, x in enumerate(row):
                if x == "B":
                    rows[i] += 1
                    g[j].append(i)
        ans = 0
        for j in g:
            i1 = g[j][0]
            if rows[i1] != target:
                continue
            if len(g[j]) == rows[i1] and all(picture[i2] == picture[i1] for i2 in g[j]):
                ans += target
        return ans
```

#### Java

```java
class Solution {
    public int findBlackPixel(char[][] picture, int target) {
        int m = picture.length;
        int n = picture[0].length;
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        int[] rows = new int[m];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (picture[i][j] == 'B') {
                    ++rows[i];
                    g[j].add(i);
                }
            }
        }
        int ans = 0;
        for (int j = 0; j < n; ++j) {
            if (g[j].isEmpty() || (rows[g[j].get(0)] != target)) {
                continue;
            }
            int i1 = g[j].get(0);
            int ok = 0;
            if (g[j].size() == rows[i1]) {
                ok = target;
                for (int i2 : g[j]) {
                    if (!Arrays.equals(picture[i1], picture[i2])) {
                        ok = 0;
                        break;
                    }
                }
            }
            ans += ok;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findBlackPixel(vector<vector<char>>& picture, int target) {
        int m = picture.size();
        int n = picture[0].size();
        vector<int> g[n];
        vector<int> rows(m);
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (picture[i][j] == 'B') {
                    ++rows[i];
                    g[j].push_back(i);
                }
            }
        }

        int ans = 0;
        for (int j = 0; j < n; ++j) {
            if (g[j].empty() || (rows[g[j][0]] != target)) {
                continue;
            }
            int i1 = g[j][0];
            int ok = 0;
            if (g[j].size() == rows[i1]) {
                ok = target;
                for (int i2 : g[j]) {
                    if (picture[i1] != picture[i2]) {
                        ok = 0;
                        break;
                    }
                }
            }
            ans += ok;
        }
        return ans;
    }
};
```

#### Go

```go
func findBlackPixel(picture [][]byte, target int) (ans int) {
	m := len(picture)
	n := len(picture[0])
	g := make([][]int, n)
	rows := make([]int, m)
	for i, row := range picture {
		for j, x := range row {
			if x == 'B' {
				rows[i]++
				g[j] = append(g[j], i)
			}
		}
	}
	for j := 0; j < n; j++ {
		if len(g[j]) == 0 || rows[g[j][0]] != target {
			continue
		}
		i1 := g[j][0]
		ok := 0
		if len(g[j]) == rows[i1] {
			ok = target
			for _, i2 := range g[j] {
				if !bytes.Equal(picture[i1], picture[i2]) {
					ok = 0
					break
				}
			}
		}
		ans += ok
	}
	return
}
```

#### TypeScript

```ts
function findBlackPixel(picture: string[][], target: number): number {
    const m: number = picture.length;
    const n: number = picture[0].length;
    const g: number[][] = Array.from({ length: n }, () => []);
    const rows: number[] = Array(m).fill(0);

    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (picture[i][j] === 'B') {
                ++rows[i];
                g[j].push(i);
            }
        }
    }

    let ans: number = 0;
    for (let j = 0; j < n; ++j) {
        if (g[j].length === 0 || rows[g[j][0]] !== target) {
            continue;
        }
        const i1: number = g[j][0];
        let ok: number = 0;
        if (g[j].length === rows[i1]) {
            ok = target;
            for (const i2 of g[j]) {
                if (picture[i1].join('') !== picture[i2].join('')) {
                    ok = 0;
                    break;
                }
            }
        }
        ans += ok;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
