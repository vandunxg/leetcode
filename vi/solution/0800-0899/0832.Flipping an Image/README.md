---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
    - Array
    - Two Pointers
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [832. Flipping an Image](https://leetcode.com/problems/flipping-an-image)

[中文文档](/solution/0800-0899/0832.Flipping%20an%20Image/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận nhị phân <code>image</code> kích thước <code>n x n</code>, hãy lật ảnh theo chiều <strong>ngang</strong>, sau đó đảo các bit rồi trả về <em>ảnh thu được</em>.</p>

<p>Lật ảnh theo chiều ngang nghĩa là đảo ngược thứ tự các phần tử trong từng hàng.</p>

<ul>
	<li>Ví dụ, lật <code>[1,1,0]</code> theo chiều ngang sẽ thu được <code>[0,1,1]</code>.</li>
</ul>

<p>Đảo ảnh nghĩa là thay mỗi <code>0</code> bằng <code>1</code> và mỗi <code>1</code> bằng <code>0</code>.</p>

<ul>
	<li>Ví dụ, đảo <code>[0,1,1]</code> sẽ thu được <code>[1,0,0]</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> image = [[1,1,0],[1,0,1],[0,0,0]]
<strong>Đầu ra:</strong> [[1,0,0],[0,1,0],[1,1,1]]
<strong>Giải thích:</strong> Đầu tiên, đảo ngược thứ tự các phần tử trong từng hàng: [[0,1,1],[1,0,1],[0,0,0]].
Sau đó đảo các bit trong ảnh: [[1,0,0],[0,1,0],[1,1,1]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> image = [[1,1,0,0],[1,0,0,1],[0,1,1,1],[1,0,1,0]]
<strong>Đầu ra:</strong> [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
<strong>Giải thích:</strong> Đầu tiên, đảo ngược thứ tự các phần tử trong từng hàng: [[0,0,1,1],[1,0,0,1],[1,1,1,0],[0,1,0,1]].
Sau đó đảo các bit trong ảnh: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == image.length</code></li>
	<li><code>n == image[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 20</code></li>
	<li><code>images[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi hàng được đảo ngược rồi đảo bit. Vì $n\le 20$, ta có thể dùng hai con trỏ để thực hiện cả hai bước ngay trên mảng.
>
> Nếu hai đầu hàng bằng nhau thì sau khi đảo ngược chúng vẫn bằng nhau, nên chỉ cần đảo cả hai bit. Nếu hai đầu khác nhau thì thao tác đảo ngược rồi đảo bit sẽ giữ nguyên giá trị, không cần ghi lại. Ô chính giữa được đảo riêng.

<!-- thinking:end -->

Ta duyệt ma trận; với mỗi hàng $\textit{row}$, dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến phần tử đầu và cuối hàng. Nếu $\textit{row}[i] = \textit{row}[j]$, đổi chỗ chúng sẽ không làm thay đổi giá trị, nên chỉ cần XOR để đảo $\textit{row}[i]$ và $\textit{row}[j]$, rồi dịch $i$ và $j$ mỗi con trỏ một vị trí về giữa cho đến khi $i \geq j$. Nếu $\textit{row}[i] \neq \textit{row}[j]$, đổi chỗ rồi đảo giá trị cũng giữ nguyên chúng, nên không cần thao tác.

Cuối cùng, nếu $i = j$, ta đảo trực tiếp $\textit{row}[i]$.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là số hàng hoặc số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def flipAndInvertImage(self, image: List[List[int]]) -> List[List[int]]:
        n = len(image)
        for row in image:
            i, j = 0, n - 1
            while i < j:
                if row[i] == row[j]:
                    row[i] ^= 1
                    row[j] ^= 1
                i, j = i + 1, j - 1
            if i == j:
                row[i] ^= 1
        return image
```

#### Java

```java
class Solution {
    public int[][] flipAndInvertImage(int[][] image) {
        for (var row : image) {
            int i = 0, j = row.length - 1;
            for (; i < j; ++i, --j) {
                if (row[i] == row[j]) {
                    row[i] ^= 1;
                    row[j] ^= 1;
                }
            }
            if (i == j) {
                row[i] ^= 1;
            }
        }
        return image;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> flipAndInvertImage(vector<vector<int>>& image) {
        for (auto& row : image) {
            int i = 0, j = row.size() - 1;
            for (; i < j; ++i, --j) {
                if (row[i] == row[j]) {
                    row[i] ^= 1;
                    row[j] ^= 1;
                }
            }
            if (i == j) {
                row[i] ^= 1;
            }
        }
        return image;
    }
};
```

#### Go

```go
func flipAndInvertImage(image [][]int) [][]int {
	for _, row := range image {
		i, j := 0, len(row)-1
		for ; i < j; i, j = i+1, j-1 {
			if row[i] == row[j] {
				row[i] ^= 1
				row[j] ^= 1
			}
		}
		if i == j {
			row[i] ^= 1
		}
	}
	return image
}
```

#### JavaScript

```js
/**
 * @param {number[][]} image
 * @return {number[][]}
 */
var flipAndInvertImage = function (image) {
    for (const row of image) {
        let i = 0;
        let j = row.length - 1;
        for (; i < j; ++i, --j) {
            if (row[i] == row[j]) {
                row[i] ^= 1;
                row[j] ^= 1;
            }
        }
        if (i == j) {
            row[i] ^= 1;
        }
    }
    return image;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
