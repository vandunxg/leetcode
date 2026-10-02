---
comments: true
difficulty: Medium
rating: 1464
source: Weekly Contest 177 Q2
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
    - Binary Tree
---

<!-- problem:start -->

# [1361. Validate Binary Tree Nodes](https://leetcode.com/problems/validate-binary-tree-nodes)

[中文文档](/solution/1300-1399/1361.Validate%20Binary%20Tree%20Nodes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>n</code> node nhị phân được đánh số từ <code>0</code> đến <code>n - 1</code>, trong đó node <code>i</code> có hai node con là <code>leftChild[i]</code> và <code>rightChild[i]</code>. Trả về <code>true</code> khi và chỉ khi <strong>tất cả</strong> node đã cho tạo thành <strong>duy nhất một</strong> cây nhị phân hợp lệ.</p>

<p>Nếu node <code>i</code> không có node con trái thì <code>leftChild[i]</code> bằng <code>-1</code>; tương tự với node con phải.</p>

<p>Lưu ý rằng các node không có giá trị; trong bài này ta chỉ dùng số hiệu của node.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1361.Validate%20Binary%20Tree%20Nodes/images/1503_ex1.png" style="width: 195px; height: 287px;" />
<pre>
<strong>Input:</strong> n = 4, leftChild = [1,-1,3,-1], rightChild = [2,-1,-1,-1]
<strong>Output:</strong> true
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1361.Validate%20Binary%20Tree%20Nodes/images/1503_ex2.png" style="width: 183px; height: 272px;" />
<pre>
<strong>Input:</strong> n = 4, leftChild = [1,-1,3,-1], rightChild = [2,3,-1,-1]
<strong>Output:</strong> false
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1361.Validate%20Binary%20Tree%20Nodes/images/1503_ex3.png" style="width: 82px; height: 174px;" />
<pre>
<strong>Input:</strong> n = 2, leftChild = [1,0], rightChild = [-1,-1]
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == leftChild.length == rightChild.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>-1 &lt;= leftChild[i], rightChild[i] &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Hai mảng node con trái và phải của $n$ node phải tạo thành đúng một cây nhị phân. Các trường hợp không hợp lệ gồm node có cha thứ hai, có chu trình hoặc có nhiều hơn một component. Union-find phát hiện node con đã có cha hoặc cạnh nối hai node trong cùng component; nếu không, hợp nhất chúng và giảm số component. Cuối cùng, số component phải bằng $1$.

<!-- thinking:end -->

Ta duyệt từng node $i$ cùng node con trái $l$ và node con phải $r$ tương ứng, dùng mảng $vis$ để ghi nhận node nào đã có cha:

- Nếu node con đã có cha, tức là node đó có nhiều cha, không thỏa điều kiện nên ta trả về `false` ngay.
- Nếu node con và node cha đã nằm trong cùng một connected component, việc thêm cạnh sẽ tạo chu trình, không thỏa điều kiện nên ta trả về `false` ngay.
- Nếu không, ta thực hiện union, đặt phần tử tương ứng trong mảng $vis$ thành `true` và giảm số connected component đi $1$.

Sau khi duyệt xong, ta kiểm tra số connected component trong cấu trúc union-find có bằng $1$ hay không. Nếu có thì trả về `true`, nếu không thì trả về `false`.

Độ phức tạp thời gian là $O(n \times \alpha(n))$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số node, còn $\alpha(n)$ là hàm inverse Ackermann, có giá trị nhỏ hơn $5$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validateBinaryTreeNodes(
        self, n: int, leftChild: List[int], rightChild: List[int]
    ) -> bool:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        p = list(range(n))
        vis = [False] * n
        for i, (a, b) in enumerate(zip(leftChild, rightChild)):
            for j in (a, b):
                if j != -1:
                    if vis[j] or find(i) == find(j):
                        return False
                    p[find(i)] = find(j)
                    vis[j] = True
                    n -= 1
        return n == 1
```

#### Java

```java
class Solution {
    private int[] p;

    public boolean validateBinaryTreeNodes(int n, int[] leftChild, int[] rightChild) {
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        boolean[] vis = new boolean[n];
        for (int i = 0, m = n; i < m; ++i) {
            for (int j : new int[] {leftChild[i], rightChild[i]}) {
                if (j != -1) {
                    if (vis[j] || find(i) == find(j)) {
                        return false;
                    }
                    p[find(i)] = find(j);
                    vis[j] = true;
                    --n;
                }
            }
        }
        return n == 1;
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
    bool validateBinaryTreeNodes(int n, vector<int>& leftChild, vector<int>& rightChild) {
        int p[n];
        iota(p, p + n, 0);
        bool vis[n];
        memset(vis, 0, sizeof(vis));
        function<int(int)> find = [&](int x) {
            return p[x] == x ? x : p[x] = find(p[x]);
        };
        for (int i = 0, m = n; i < m; ++i) {
            for (int j : {leftChild[i], rightChild[i]}) {
                if (j != -1) {
                    if (vis[j] || find(i) == find(j)) {
                        return false;
                    }
                    p[find(i)] = find(j);
                    vis[j] = true;
                    --n;
                }
            }
        }
        return n == 1;
    }
};
```

#### Go

```go
func validateBinaryTreeNodes(n int, leftChild []int, rightChild []int) bool {
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
	vis := make([]bool, n)
	for i, a := range leftChild {
		for _, j := range []int{a, rightChild[i]} {
			if j != -1 {
				if vis[j] || find(i) == find(j) {
					return false
				}
				p[find(i)] = find(j)
				vis[j] = true
				n--
			}
		}
	}
	return n == 1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Đếm indegree + BFS

<!-- thinking:start -->

> **Tư duy**
>
> Union-find theo dõi quan hệ cha và các component. Đếm indegree giúp tìm root duy nhất (hoặc phát hiện không có root). BFS từ root đó sẽ loại trường hợp thăm lại node con, rồi kiểm tra mọi node đều đã được thăm mà không cần mảng lưu cha.

<!-- thinking:end -->

Trước tiên, ta đếm indegree của từng node, tức số node cha trỏ đến nó. Nếu không có node nào có indegree bằng $0$, graph có chu trình nên ta trả về `false`; nếu có thì node đó là root.

Tiếp theo, ta thực hiện breadth-first search bắt đầu từ root. Trong lúc duyệt, nếu node con đã được thăm thì node đó có nhiều cha hoặc graph có chu trình, nên ta trả về `false` ngay.

Sau khi duyệt xong, ta kiểm tra số node đã thăm có bằng $n$ hay không. Nếu bằng, tất cả node tạo thành đúng một cây nhị phân hợp lệ và ta trả về `true`; nếu không thì trả về `false`.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validateBinaryTreeNodes(
        self, n: int, leftChild: List[int], rightChild: List[int]
    ) -> bool:
        indeg = [0] * n
        for c in chain(leftChild, rightChild):
            if c != -1:
                indeg[c] += 1
        root = next((i for i, x in enumerate(indeg) if x == 0), -1)
        if root == -1:
            return False
        q = deque([root])
        vis = {root}
        while q:
            i = q.popleft()
            for j in (leftChild[i], rightChild[i]):
                if j != -1:
                    if j in vis:
                        return False
                    vis.add(j)
                    q.append(j)
        return len(vis) == n
```

#### Java

```java
class Solution {
    public boolean validateBinaryTreeNodes(int n, int[] leftChild, int[] rightChild) {
        int[] indeg = new int[n];
        for (int c : leftChild) {
            if (c != -1) {
                indeg[c]++;
            }
        }
        for (int c : rightChild) {
            if (c != -1) {
                indeg[c]++;
            }
        }

        int root = -1;
        for (int i = 0; i < n; i++) {
            if (indeg[i] == 0) {
                root = i;
                break;
            }
        }
        if (root == -1) {
            return false;
        }

        Deque<Integer> q = new ArrayDeque<>();
        q.add(root);
        Set<Integer> vis = new HashSet<>();
        vis.add(root);

        while (!q.isEmpty()) {
            int i = q.poll();
            int j = leftChild[i];
            if (j != -1) {
                if (vis.contains(j)) {
                    return false;
                }
                vis.add(j);
                q.add(j);
            }

            j = rightChild[i];
            if (j != -1) {
                if (vis.contains(j)) {
                    return false;
                }
                vis.add(j);
                q.add(j);
            }
        }

        return vis.size() == n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool validateBinaryTreeNodes(int n, vector<int>& leftChild, vector<int>& rightChild) {
        vector<int> indeg(n, 0);
        for (int c : leftChild) {
            if (c != -1) {
                indeg[c]++;
            }
        }
        for (int c : rightChild) {
            if (c != -1) {
                indeg[c]++;
            }
        }

        int root = -1;
        for (int i = 0; i < n; i++) {
            if (indeg[i] == 0) {
                root = i;
                break;
            }
        }
        if (root == -1) {
            return false;
        }

        queue<int> q;
        unordered_set<int> vis;

        q.push(root);
        vis.insert(root);

        while (!q.empty()) {
            int i = q.front();
            q.pop();

            int j = leftChild[i];
            if (j != -1) {
                if (vis.count(j)) {
                    return false;
                }
                vis.insert(j);
                q.push(j);
            }

            j = rightChild[i];
            if (j != -1) {
                if (vis.count(j)) {
                    return false;
                }
                vis.insert(j);
                q.push(j);
            }
        }

        return vis.size() == n;
    }
};
```

#### Go

```go
func validateBinaryTreeNodes(n int, leftChild []int, rightChild []int) bool {
	indeg := make([]int, n)

	for _, c := range leftChild {
		if c != -1 {
			indeg[c]++
		}
	}
	for _, c := range rightChild {
		if c != -1 {
			indeg[c]++
		}
	}

	root := -1
	for i, x := range indeg {
		if x == 0 {
			root = i
			break
		}
	}
	if root == -1 {
		return false
	}

	q := []int{root}
	vis := map[int]bool{root: true}

	for len(q) > 0 {
		i := q[0]
		q = q[1:]

		j := leftChild[i]
		if j != -1 {
			if vis[j] {
				return false
			}
			vis[j] = true
			q = append(q, j)
		}

		j = rightChild[i]
		if j != -1 {
			if vis[j] {
				return false
			}
			vis[j] = true
			q = append(q, j)
		}
	}

	return len(vis) == n
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
