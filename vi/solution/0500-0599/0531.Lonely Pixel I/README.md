---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Matrix
---

<!-- problem:start -->

# [531. Lonely Pixel I 🔒](https://leetcode.com/problems/lonely-pixel-i)

[中文文档](/solution/0500-0599/0531.Lonely%20Pixel%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ảnh <code>picture</code> kích thước <code>m x n</code>, gồm các pixel đen <code>&#39;B&#39;</code> và pixel trắng <code>&#39;W&#39;</code>. Hãy trả về <em>số lượng pixel đen <b>cô độc</b></em>.</p>

<p>Pixel đen cô độc là ký tự <code>&#39;B&#39;</code> nằm tại một vị trí mà trên cùng hàng và cùng cột không có <strong>bất kỳ</strong> pixel đen nào khác.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0531.Lonely%20Pixel%20I/images/pixel1.jpg" style="width: 242px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> picture = [[&quot;W&quot;,&quot;W&quot;,&quot;B&quot;],[&quot;W&quot;,&quot;B&quot;,&quot;W&quot;],[&quot;B&quot;,&quot;W&quot;,&quot;W&quot;]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Cả ba pixel &#39;B&#39; đều là pixel đen cô độc.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0531.Lonely%20Pixel%20I/images/pixel2.jpg" style="width: 242px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> picture = [[&quot;B&quot;,&quot;B&quot;,&quot;B&quot;],[&quot;B&quot;,&quot;B&quot;,&quot;W&quot;],[&quot;B&quot;,&quot;B&quot;,&quot;B&quot;]]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m ==&nbsp;picture.length</code></li>
	<li><code>n ==&nbsp;picture[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 500</code></li>
	<li><code>picture[i][j]</code> là <code>&#39;W&#39;</code> hoặc <code>&#39;B&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm và duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Pixel đen cô độc là pixel `B` duy nhất trên hàng và cột của nó. Kiểm tra lại hàng và cột cho từng `B` sẽ tốn $O(mn(m+n))$.
>
> Trước tiên, đếm số `B` trên từng hàng và cột, sau đó duyệt lại: `B` nào có cả hai số đếm bằng $1$ thì là pixel cô độc. Hai lượt duyệt, độ phức tạp thời gian $O(mn)$.

<!-- thinking:end -->

Theo đề bài, ta cần đếm số pixel đen trên từng hàng và cột, rồi lần lượt lưu vào các mảng `rows` và `cols`. Sau đó, duyệt từng pixel đen và kiểm tra xem trên hàng và cột của nó có đúng một pixel đen hay không. Nếu có, tăng đáp án thêm một.

Độ phức tạp thời gian là $O(m \times n)$, độ phức tạp không gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLonelyPixel(self, picture: List[List[str]]) -> int:
        rows = [0] * len(picture)
        cols = [0] * len(picture[0])
        for i, row in enumerate(picture):
            for j, x in enumerate(row):
                if x == "B":
                    rows[i] += 1
                    cols[j] += 1
        ans = 0
        for i, row in enumerate(picture):
            for j, x in enumerate(row):
                if x == "B" and rows[i] == 1 and cols[j] == 1:
                    ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int findLonelyPixel(char[][] picture) {
        int m = picture.length, n = picture[0].length;
        int[] rows = new int[m];
        int[] cols = new int[n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (picture[i][j] == 'B') {
                    ++rows[i];
                    ++cols[j];
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (picture[i][j] == 'B' && rows[i] == 1 && cols[j] == 1) {
                    ++ans;
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
    int findLonelyPixel(vector<vector<char>>& picture) {
        int m = picture.size(), n = picture[0].size();
        vector<int> rows(m);
        vector<int> cols(n);
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (picture[i][j] == 'B') {
                    ++rows[i];
                    ++cols[j];
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (picture[i][j] == 'B' && rows[i] == 1 && cols[j] == 1) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findLonelyPixel(picture [][]byte) (ans int) {
	rows := make([]int, len(picture))
	cols := make([]int, len(picture[0]))
	for i, row := range picture {
		for j, x := range row {
			if x == 'B' {
				rows[i]++
				cols[j]++
			}
		}
	}
	for i, row := range picture {
		for j, x := range row {
			if x == 'B' && rows[i] == 1 && cols[j] == 1 {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function findLonelyPixel(picture: string[][]): number {
    const m = picture.length;
    const n = picture[0].length;
    const rows: number[] = Array(m).fill(0);
    const cols: number[] = Array(n).fill(0);
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (picture[i][j] === 'B') {
                ++rows[i];
                ++cols[j];
            }
        }
    }
    let ans = 0;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (picture[i][j] === 'B' && rows[i] === 1 && cols[j] === 1) {
                ++ans;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
