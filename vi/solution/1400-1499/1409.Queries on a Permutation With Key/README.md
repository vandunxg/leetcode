---
comments: true
difficulty: Medium
rating: 1334
source: Weekly Contest 184 Q2
tags:
    - Binary Indexed Tree
    - Array
    - Simulation
    - Sqrt Decomposition
---

<!-- problem:start -->

# [1409. Queries on a Permutation With Key](https://leetcode.com/problems/queries-on-a-permutation-with-key)

[中文文档](/solution/1400-1499/1409.Queries%20on%20a%20Permutation%20With%20Key/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>queries</code> gồm các số nguyên dương từ <code>1</code> đến <code>m</code>, hãy xử lý tất cả <code>queries[i]</code> (từ <code>i=0</code> đến <code>i=queries.length-1</code>) theo các quy tắc sau:</p>

<ul>
	<li>Ban đầu, ta có hoán vị <code>P=[1,2,3,...,m]</code>.</li>
	<li>Với <code>i</code> hiện tại, tìm vị trí của <code>queries[i]</code> trong hoán vị <code>P</code> (<strong>đánh chỉ số từ 0</strong>), sau đó chuyển phần tử này lên đầu hoán vị <code>P</code>. Lưu ý rằng vị trí của <code>queries[i]</code> trong <code>P</code> chính là kết quả tương ứng với <code>queries[i]</code>.</li>
</ul>

<p>Trả về một mảng chứa kết quả cho <code>queries</code> đã cho.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> queries = [3,1,2,1], m = 5
<strong>Đầu ra:</strong> [2,1,2,1] 
<strong>Giải thích:</strong> Các query được xử lý như sau: 
Với i=0: queries[i]=3, P=[1,2,3,4,5], vị trí của 3 trong P là <strong>2</strong>, sau đó chuyển 3 lên đầu P, thu được P=[3,1,2,4,5]. 
Với i=1: queries[i]=1, P=[3,1,2,4,5], vị trí của 1 trong P là <strong>1</strong>, sau đó chuyển 1 lên đầu P, thu được P=[1,3,2,4,5]. 
Với i=2: queries[i]=2, P=[1,3,2,4,5], vị trí của 2 trong P là <strong>2</strong>, sau đó chuyển 2 lên đầu P, thu được P=[2,1,3,4,5]. 
Với i=3: queries[i]=1, P=[2,1,3,4,5], vị trí của 1 trong P là <strong>1</strong>, sau đó chuyển 1 lên đầu P, thu được P=[1,2,3,4,5]. 
Vì vậy, mảng chứa kết quả là [2,1,2,1].  
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> queries = [4,1,2,2], m = 4
<strong>Đầu ra:</strong> [3,1,2,0]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> queries = [7,5,5,8,3], m = 8
<strong>Đầu ra:</strong> [6,5,0,7,5]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m &lt;= 10^3</code></li>
	<li><code>1 &lt;= queries.length &lt;= m</code></li>
	<li><code>1 &lt;= queries[i] &lt;= m</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> $m\le 10^3$. Tìm một giá trị trong hoán vị độ dài $m$ rồi chuyển giá trị đó lên đầu mất $O(m)$ cho mỗi query, nên tổng $O(m^2)$ là chấp nhận được.
>
> Lưu hoán vị trong một list, ghi lại `index`, sau đó pop và chèn phần tử vào đầu.

<!-- thinking:end -->

Kích thước dữ liệu của bài toán không lớn, nên ta có thể mô phỏng trực tiếp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def processQueries(self, queries: List[int], m: int) -> List[int]:
        p = list(range(1, m + 1))
        ans = []
        for v in queries:
            j = p.index(v)
            ans.append(j)
            p.pop(j)
            p.insert(0, v)
        return ans
```

#### Java

```java
class Solution {
    public int[] processQueries(int[] queries, int m) {
        List<Integer> p = new LinkedList<>();
        for (int i = 1; i <= m; ++i) {
            p.add(i);
        }
        int[] ans = new int[queries.length];
        int i = 0;
        for (int v : queries) {
            int j = p.indexOf(v);
            ans[i++] = j;
            p.remove(j);
            p.add(0, v);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> processQueries(vector<int>& queries, int m) {
        vector<int> p(m);
        iota(p.begin(), p.end(), 1);
        vector<int> ans;
        for (int v : queries) {
            int j = 0;
            for (int i = 0; i < m; ++i) {
                if (p[i] == v) {
                    j = i;
                    break;
                }
            }
            ans.push_back(j);
            p.erase(p.begin() + j);
            p.insert(p.begin(), v);
        }
        return ans;
    }
};
```

#### Go

```go
func processQueries(queries []int, m int) []int {
	p := make([]int, m)
	for i := range p {
		p[i] = i + 1
	}
	ans := []int{}
	for _, v := range queries {
		j := 0
		for i := range p {
			if p[i] == v {
				j = i
				break
			}
		}
		ans = append(ans, j)
		p = append(p[:j], p[j+1:]...)
		p = append([]int{v}, p...)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 quét danh sách ở mỗi lần. Nếu có thể hỏi “có bao nhiêu phần tử nằm bên trái giá trị này” trong $O(\log(m+n))$, ta sẽ xử lý nhanh hơn.
>
> Đặt hoán vị ban đầu tại các chỉ số $[n+1,n+m]$ và chuyển mỗi giá trị được query đến một ô trống ở bên trái. Fenwick tree lưu trạng thái có phần tử; prefix sum chính là chỉ số hiện tại.

<!-- thinking:end -->

Binary Indexed Tree (BIT), còn được gọi là Fenwick Tree, hỗ trợ hiệu quả hai thao tác sau:

1. **Point Update** `update(x, delta)`: Cộng giá trị `delta` vào phần tử tại vị trí `x` trong dãy.
2. **Prefix Sum Query** `query(x)`: Tính tổng của dãy trên đoạn `[1,...,x]`, tức prefix sum tại vị trí `x`.

Cả hai thao tác đều có độ phức tạp thời gian là $O(\log n)$.

Chức năng cốt lõi của Binary Indexed Tree là đếm số phần tử nhỏ hơn một phần tử cho trước `x`. Việc so sánh này mang tính trừu tượng và có thể áp dụng cho kích thước, tọa độ, khối lượng, v.v.

Ví dụ, cho mảng `a[5] = {2, 5, 3, 4, 1}`, ta cần tính `b[i] = the number of elements to the left of position i that are less than or equal to a[i]`. Với ví dụ này, `b[5] = {0, 1, 1, 2, 0}`.

Lời giải là duyệt mảng, trước tiên tính `query(a[i])` cho mỗi vị trí, sau đó cập nhật Binary Indexed Tree bằng `update(a[i], 1)`. Khi miền giá trị lớn, cần rời rạc hóa, bao gồm loại bỏ phần tử trùng lặp, sắp xếp, rồi gán chỉ số cho mỗi giá trị.

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n):
        self.n = n
        self.c = [0] * (n + 1)

    @staticmethod
    def lowbit(x):
        return x & -x

    def update(self, x, delta):
        while x <= self.n:
            self.c[x] += delta
            x += BinaryIndexedTree.lowbit(x)

    def query(self, x):
        s = 0
        while x > 0:
            s += self.c[x]
            x -= BinaryIndexedTree.lowbit(x)
        return s


class Solution:
    def processQueries(self, queries: List[int], m: int) -> List[int]:
        n = len(queries)
        pos = [0] * (m + 1)
        tree = BinaryIndexedTree(m + n)
        for i in range(1, m + 1):
            pos[i] = n + i
            tree.update(n + i, 1)

        ans = []
        for i, v in enumerate(queries):
            j = pos[v]
            tree.update(j, -1)
            ans.append(tree.query(j))
            pos[v] = n - i
            tree.update(n - i, 1)
        return ans
```

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
    }

    public void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += lowbit(x);
        }
    }

    public int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= lowbit(x);
        }
        return s;
    }

    public static int lowbit(int x) {
        return x & -x;
    }
}

class Solution {
    public int[] processQueries(int[] queries, int m) {
        int n = queries.length;
        BinaryIndexedTree tree = new BinaryIndexedTree(m + n);
        int[] pos = new int[m + 1];
        for (int i = 1; i <= m; ++i) {
            pos[i] = n + i;
            tree.update(n + i, 1);
        }
        int[] ans = new int[n];
        int k = 0;
        for (int i = 0; i < n; ++i) {
            int v = queries[i];
            int j = pos[v];
            tree.update(j, -1);
            ans[k++] = tree.query(j);
            pos[v] = n - i;
            tree.update(n - i, 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
public:
    int n;
    vector<int> c;

    BinaryIndexedTree(int _n)
        : n(_n)
        , c(_n + 1) {}

    void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += lowbit(x);
        }
    }

    int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= lowbit(x);
        }
        return s;
    }

    int lowbit(int x) {
        return x & -x;
    }
};

class Solution {
public:
    vector<int> processQueries(vector<int>& queries, int m) {
        int n = queries.size();
        vector<int> pos(m + 1);
        BinaryIndexedTree* tree = new BinaryIndexedTree(m + n);
        for (int i = 1; i <= m; ++i) {
            pos[i] = n + i;
            tree->update(n + i, 1);
        }
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            int v = queries[i];
            int j = pos[v];
            tree->update(j, -1);
            ans.push_back(tree->query(j));
            pos[v] = n - i;
            tree->update(n - i, 1);
        }
        return ans;
    }
};
```

#### Go

```go
type BinaryIndexedTree struct {
	n int
	c []int
}

func newBinaryIndexedTree(n int) *BinaryIndexedTree {
	c := make([]int, n+1)
	return &BinaryIndexedTree{n, c}
}

func (this *BinaryIndexedTree) lowbit(x int) int {
	return x & -x
}

func (this *BinaryIndexedTree) update(x, delta int) {
	for x <= this.n {
		this.c[x] += delta
		x += this.lowbit(x)
	}
}

func (this *BinaryIndexedTree) query(x int) int {
	s := 0
	for x > 0 {
		s += this.c[x]
		x -= this.lowbit(x)
	}
	return s
}

func processQueries(queries []int, m int) []int {
	n := len(queries)
	pos := make([]int, m+1)
	tree := newBinaryIndexedTree(m + n)
	for i := 1; i <= m; i++ {
		pos[i] = n + i
		tree.update(n+i, 1)
	}
	ans := []int{}
	for i, v := range queries {
		j := pos[v]
		tree.update(j, -1)
		ans = append(ans, tree.query(j))
		pos[v] = n - i
		tree.update(n-i, 1)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
