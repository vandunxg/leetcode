---
comments: true
difficulty: Hard
rating: 2115
source: Weekly Contest 193 Q4
tags:
    - Bit Manipulation
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Design
    - Binary Search
    - Dynamic Programming
    - Binary Lifting
---

<!-- problem:start -->

# [1483. Kth Ancestor of a Tree Node](https://leetcode.com/problems/kth-ancestor-of-a-tree-node)

[中文文档](/solution/1400-1499/1483.Kth%20Ancestor%20of%20a%20Tree%20Node/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>, biểu diễn dưới dạng mảng cha <code>parent</code>, trong đó <code>parent[i]</code> là cha của node thứ <code>i</code>. Gốc của cây là node <code>0</code>. Hãy tìm tổ tiên thứ <code>k</code> của một node cho trước.</p>

<p>Tổ tiên thứ <code>k</code> của một node trong cây là node thứ <code>k</code> trên đường đi từ node đó đến node gốc.</p>

<p>Hãy triển khai class <code>TreeAncestor</code>:</p>

<ul>
	<li><code>TreeAncestor(int n, int[] parent)</code> khởi tạo đối tượng với số lượng node trong cây và mảng cha.</li>
	<li><code>int getKthAncestor(int node, int k)</code> trả về tổ tiên thứ <code>k</code> của node đã cho <code>node</code>. Nếu không tồn tại tổ tiên như vậy, trả về <code>-1</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1483.Kth%20Ancestor%20of%20a%20Tree%20Node/images/1528_ex1.png" style="width: 396px; height: 262px;" />
<pre>
<strong>Đầu vào</strong>
[&quot;TreeAncestor&quot;, &quot;getKthAncestor&quot;, &quot;getKthAncestor&quot;, &quot;getKthAncestor&quot;]
[[7, [-1, 0, 0, 1, 1, 2, 2]], [3, 1], [5, 2], [6, 3]]
<strong>Đầu ra</strong>
[null, 1, 0, -1]

<strong>Giải thích</strong>
TreeAncestor treeAncestor = new TreeAncestor(7, [-1, 0, 0, 1, 1, 2, 2]);
treeAncestor.getKthAncestor(3, 1); // returns 1 which is the parent of 3
treeAncestor.getKthAncestor(5, 2); // returns 0 which is the grandparent of 5
treeAncestor.getKthAncestor(6, 3); // returns -1 because there is no such ancestor</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>parent.length == n</code></li>
	<li><code>parent[0] == -1</code></li>
	<li><code>0 &lt;= parent[i] &lt; n</code> với mọi <code>0 &lt; i &lt; n</code></li>
	<li><code>0 &lt;= node &lt; n</code></li>
	<li>Có tối đa <code>5 * 10<sup>4</sup></code> truy vấn.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + Binary Lifting

<!-- thinking:start -->

> **Tư duy**
>
> Vì cả $n$ và số lượng truy vấn đều là $5\times 10^4$, việc lần theo $k$ node cha cho mỗi truy vấn sẽ quá chậm. Ta tiền xử lý $p[i][j]$ là tổ tiên thứ $2^j$, sử dụng $p[i][j]=p[p[i][j-1]][j-1]$.
>
> Mỗi truy vấn thực hiện các bước nhảy theo các bit của $k$ trong $O(\log n)$.

<!-- thinking:end -->

Bài toán yêu cầu tìm node tổ tiên thứ $k$ của node $node$. Nếu giải bằng brute force, ta cần đi ngược từ $node$ lên $k$ lần, có độ phức tạp thời gian là $O(k)$ và rõ ràng sẽ vượt quá giới hạn thời gian.

Ta có thể kết hợp quy hoạch động với ý tưởng binary lifting để xử lý bài toán này.

Ta định nghĩa $p[i][j]$ là node tổ tiên thứ $2^j$ của node $i$, tức là node đạt được khi đi lên từ node $i$ $2^j$ bước. Khi đó, ta có công thức chuyển trạng thái:

$$
p[i][j] = p[p[i][j-1]][j-1]
$$

Nói cách khác, để tìm node tổ tiên thứ $2^j$ của node $i$, trước tiên ta tìm node tổ tiên thứ $2^{j-1}$ của node $i$, sau đó tìm node tổ tiên thứ $2^{j-1}$ của node này. Do đó, ta cần tìm tổ tiên của mỗi node ở khoảng cách $2^j$ cho đến khi đạt đến chiều cao lớn nhất của cây.

Với mỗi truy vấn, ta phân tích $k$ thành biểu diễn nhị phân, sau đó dựa vào vị trí của các bit $1$ trong biểu diễn nhị phân để lần lượt nhảy lên, cuối cùng thu được node tổ tiên thứ $k$ của node $node$.

Về độ phức tạp thời gian, khởi tạo là $O(n \times \log n)$ và mỗi truy vấn là $O(\log n)$. Độ phức tạp không gian là $O(n \times \log n)$, trong đó $n$ là số node trong cây.

Các bài toán tương tự:

- [2836. Maximize Value of Function in a Ball Passing Game](https://github.com/doocs/leetcode/blob/main/solution/2800-2899/2836.Maximize%20Value%20of%20Function%20in%20a%20Ball%20Passing%20Game/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class TreeAncestor:
    def __init__(self, n: int, parent: List[int]):
        self.p = [[-1] * 18 for _ in range(n)]
        for i, fa in enumerate(parent):
            self.p[i][0] = fa
        for j in range(1, 18):
            for i in range(n):
                if self.p[i][j - 1] == -1:
                    continue
                self.p[i][j] = self.p[self.p[i][j - 1]][j - 1]

    def getKthAncestor(self, node: int, k: int) -> int:
        for i in range(17, -1, -1):
            if k >> i & 1:
                node = self.p[node][i]
                if node == -1:
                    break
        return node


# Your TreeAncestor object will be instantiated and called as such:
# obj = TreeAncestor(n, parent)
# param_1 = obj.getKthAncestor(node,k)
```

#### Java

```java
class TreeAncestor {
    private int[][] p;

    public TreeAncestor(int n, int[] parent) {
        p = new int[n][18];
        for (var e : p) {
            Arrays.fill(e, -1);
        }
        for (int i = 0; i < n; ++i) {
            p[i][0] = parent[i];
        }
        for (int j = 1; j < 18; ++j) {
            for (int i = 0; i < n; ++i) {
                if (p[i][j - 1] == -1) {
                    continue;
                }
                p[i][j] = p[p[i][j - 1]][j - 1];
            }
        }
    }

    public int getKthAncestor(int node, int k) {
        for (int i = 17; i >= 0; --i) {
            if (((k >> i) & 1) == 1) {
                node = p[node][i];
                if (node == -1) {
                    break;
                }
            }
        }
        return node;
    }
}

/**
 * Your TreeAncestor object will be instantiated and called as such:
 * TreeAncestor obj = new TreeAncestor(n, parent);
 * int param_1 = obj.getKthAncestor(node,k);
 */
```

#### C++

```cpp
class TreeAncestor {
public:
    TreeAncestor(int n, vector<int>& parent) {
        p = vector<vector<int>>(n, vector<int>(18, -1));
        for (int i = 0; i < n; ++i) {
            p[i][0] = parent[i];
        }
        for (int j = 1; j < 18; ++j) {
            for (int i = 0; i < n; ++i) {
                if (p[i][j - 1] == -1) {
                    continue;
                }
                p[i][j] = p[p[i][j - 1]][j - 1];
            }
        }
    }

    int getKthAncestor(int node, int k) {
        for (int i = 17; ~i; --i) {
            if (k >> i & 1) {
                node = p[node][i];
                if (node == -1) {
                    break;
                }
            }
        }
        return node;
    }

private:
    vector<vector<int>> p;
};

/**
 * Your TreeAncestor object will be instantiated and called as such:
 * TreeAncestor* obj = new TreeAncestor(n, parent);
 * int param_1 = obj->getKthAncestor(node,k);
 */
```

#### Go

```go
type TreeAncestor struct {
	p [][18]int
}

func Constructor(n int, parent []int) TreeAncestor {
	p := make([][18]int, n)
	for i, fa := range parent {
		p[i][0] = fa
		for j := 1; j < 18; j++ {
			p[i][j] = -1
		}
	}
	for j := 1; j < 18; j++ {
		for i := range p {
			if p[i][j-1] == -1 {
				continue
			}
			p[i][j] = p[p[i][j-1]][j-1]
		}
	}
	return TreeAncestor{p}
}

func (this *TreeAncestor) GetKthAncestor(node int, k int) int {
	for i := 17; i >= 0; i-- {
		if k>>i&1 == 1 {
			node = this.p[node][i]
			if node == -1 {
				break
			}
		}
	}
	return node
}

/**
 * Your TreeAncestor object will be instantiated and called as such:
 * obj := Constructor(n, parent);
 * param_1 := obj.GetKthAncestor(node,k);
 */
```

#### TypeScript

```ts
class TreeAncestor {
    private p: number[][];

    constructor(n: number, parent: number[]) {
        const p = new Array(n).fill(0).map(() => new Array(18).fill(-1));
        for (let i = 0; i < n; ++i) {
            p[i][0] = parent[i];
        }
        for (let j = 1; j < 18; ++j) {
            for (let i = 0; i < n; ++i) {
                if (p[i][j - 1] === -1) {
                    continue;
                }
                p[i][j] = p[p[i][j - 1]][j - 1];
            }
        }
        this.p = p;
    }

    getKthAncestor(node: number, k: number): number {
        for (let i = 17; i >= 0; --i) {
            if (((k >> i) & 1) === 1) {
                node = this.p[node][i];
                if (node === -1) {
                    break;
                }
            }
        }
        return node;
    }
}

/**
 * Your TreeAncestor object will be instantiated and called as such:
 * var obj = new TreeAncestor(n, parent)
 * var param_1 = obj.getKthAncestor(node,k)
 */
```

#### C#

```cs
public class TreeAncestor {
    private int[][] p;

    public TreeAncestor(int n, int[] parent) {
        p = new int[n][];
        for (int i = 0; i < n; i++) {
            p[i] = new int[18];
            for (int j = 0; j < 18; j++) {
                p[i][j] = -1;
            }
        }

        for (int i = 0; i < n; ++i) {
            p[i][0] = parent[i];
        }

        for (int j = 1; j < 18; ++j) {
            for (int i = 0; i < n; ++i) {
                if (p[i][j - 1] == -1) {
                    continue;
                }
                p[i][j] = p[p[i][j - 1]][j - 1];
            }
        }
    }

    public int GetKthAncestor(int node, int k) {
        for (int i = 17; i >= 0; --i) {
            if (((k >> i) & 1) == 1) {
                node = p[node][i];
                if (node == -1) {
                    break;
                }
            }
        }
        return node;
    }
}

/**
 * Your TreeAncestor object will be instantiated and called as such:
 * TreeAncestor obj = new TreeAncestor(n, parent);
 * int param_1 = obj.GetKthAncestor(node,k);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
