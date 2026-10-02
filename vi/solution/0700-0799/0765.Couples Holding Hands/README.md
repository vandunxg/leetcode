---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
---

<!-- problem:start -->

# [765. Couples Holding Hands](https://leetcode.com/problems/couples-holding-hands)

[中文文档](/solution/0700-0799/0765.Couples%20Holding%20Hands/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> cặp đôi ngồi trên <code>2n</code> ghế xếp thành một hàng và họ muốn ngồi cạnh nhau để nắm tay.</p>

<p>Người ngồi trên các ghế được biểu diễn bằng mảng số nguyên <code>row</code>, trong đó <code>row[i]</code> là ID của người ngồi ở ghế thứ <code>i<sup>th</sup></code>. Các cặp đôi được đánh số theo thứ tự: cặp đầu tiên là <code>(0, 1)</code>, cặp thứ hai là <code>(2, 3)</code>, và cứ tiếp tục như vậy đến cặp cuối cùng <code>(2n - 2, 2n - 1)</code>.</p>

<p>Hãy trả về <em>số lần hoán đổi ít nhất để mọi cặp đôi đều ngồi cạnh nhau</em>. Mỗi lần hoán đổi, ta chọn hai người bất kỳ, cho họ đứng dậy rồi đổi chỗ ngồi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> row = [0,2,1,3]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta chỉ cần đổi chỗ người thứ hai (row[1]) và người thứ ba (row[2]).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> row = [3,2,0,1]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Tất cả các cặp đôi đã ngồi cạnh nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2n == row.length</code></li>
	<li><code>2 &lt;= n &lt;= 30</code>​​​​​​​</li>
	<li><code>0 &lt;= row[i] &lt; 2n</code></li>
	<li>Tất cả phần tử trong <code>row</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp các cặp đôi ngồi cạnh nhau với ít lần hoán đổi nhất. Bài toán có thể quy về các chu trình trong hoán vị ID của các cặp đôi.
>
> Mỗi cặp ghế ánh xạ hai người sang số hiệu cặp đôi; các cặp đôi đó được union với nhau. Một chu trình gồm $y$ cặp đôi cần $y-1$ lần hoán đổi.
>
> Dùng union-find trên $n$ ID cặp đôi; đáp án bằng $n$ trừ đi số root.

<!-- thinking:end -->

Ta có thể gán một số hiệu cho mỗi cặp đôi. Người mang số $0$ và $1$ thuộc cặp đôi $0$, người mang số $2$ và $3$ thuộc cặp đôi $1$, và cứ tiếp tục như vậy. Nói cách khác, người có ID $row[i]$ thuộc cặp đôi số $\lfloor \frac{row[i]}{2} \rfloor$.

Nếu có $k$ cặp đôi đang ngồi sai vị trí tương đối với nhau, tức là có $k$ cặp đôi nằm trong cùng một chu trình hoán vị, cần $k-1$ lần hoán đổi để tất cả ngồi đúng chỗ.

Vì sao? Trước tiên, ta đổi chỗ để đưa một cặp đôi về đúng vị trí. Khi đó, bài toán giảm từ $k$ cặp đôi xuống còn $k-1$ cặp. Quá trình tiếp tục cho đến khi $k = 1$, lúc này không cần hoán đổi nữa. Vì vậy, nếu có $k$ cặp đôi ngồi sai vị trí, cần $k-1$ lần hoán đổi.

Do đó, ta chỉ cần duyệt mảng một lần và dùng union-find để xác định số chu trình hoán vị. Giả sử có $x$ chu trình, kích thước mỗi chu trình (tính theo số cặp đôi) lần lượt là $y_1, y_2, \cdots, y_x$. Số lần hoán đổi cần thiết là $y_1-1 + y_2-1 + \cdots + y_x-1 = y_1 + y_2 + \cdots + y_x - x = n - x$.

Độ phức tạp thời gian là $O(n \times \alpha(n))$ và độ phức tạp không gian là $O(n)$, trong đó $\alpha(n)$ là hàm Ackermann ngược, có thể xem như một hằng số rất nhỏ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSwapsCouples(self, row: List[int]) -> int:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        n = len(row) >> 1
        p = list(range(n))
        for i in range(0, len(row), 2):
            a, b = row[i] >> 1, row[i + 1] >> 1
            p[find(a)] = find(b)
        return n - sum(i == find(i) for i in range(n))
```

#### Java

```java
class Solution {
    private int[] p;

    public int minSwapsCouples(int[] row) {
        int n = row.length >> 1;
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        for (int i = 0; i < n << 1; i += 2) {
            int a = row[i] >> 1, b = row[i + 1] >> 1;
            p[find(a)] = find(b);
        }
        int ans = n;
        for (int i = 0; i < n; ++i) {
            if (i == find(i)) {
                --ans;
            }
        }
        return ans;
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSwapsCouples(vector<int>& row) {
        int n = row.size() / 2;
        int p[n];
        iota(p, p + n, 0);
        function<int(int)> find = [&](int x) -> int {
            if (p[x] != x) {
                p[x] = find(p[x]);
            }
            return p[x];
        };
        for (int i = 0; i < n << 1; i += 2) {
            int a = row[i] >> 1, b = row[i + 1] >> 1;
            p[find(a)] = find(b);
        }
        int ans = n;
        for (int i = 0; i < n; ++i) {
            ans -= i == find(i);
        }
        return ans;
    }
};
```

#### Go

```go
func minSwapsCouples(row []int) int {
	n := len(row) >> 1
	p := make([]int, n)
	for i := range p {
		p[i] = i
	}
	var find func(int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	for i := 0; i < n<<1; i += 2 {
		a, b := row[i]>>1, row[i+1]>>1
		p[find(a)] = find(b)
	}
	ans := n
	for i := range p {
		if find(i) == i {
			ans--
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minSwapsCouples(row: number[]): number {
    const n = row.length >> 1;
    const p: number[] = Array(n)
        .fill(0)
        .map((_, i) => i);
    const find = (x: number): number => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    for (let i = 0; i < n << 1; i += 2) {
        const a = row[i] >> 1;
        const b = row[i + 1] >> 1;
        p[find(a)] = find(b);
    }
    let ans = n;
    for (let i = 0; i < n; ++i) {
        if (i === find(i)) {
            --ans;
        }
    }
    return ans;
}
```

#### C#

```cs
public class Solution {
    private int[] p;

    public int MinSwapsCouples(int[] row) {
        int n = row.Length >> 1;
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        for (int i = 0; i < n << 1; i += 2) {
            int a = row[i] >> 1;
            int b = row[i + 1] >> 1;
            p[find(a)] = find(b);
        }
        int ans = n;
        for (int i = 0; i < n; ++i) {
            if (p[i] == i) {
                --ans;
            }
        }
        return ans;
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
