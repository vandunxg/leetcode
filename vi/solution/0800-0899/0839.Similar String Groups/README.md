---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [839. Similar String Groups](https://leetcode.com/problems/similar-string-groups)

[中文文档](/solution/0800-0899/0839.Similar%20String%20Groups/README.md)

## Mô tả

<!-- description:start -->

<p>Hai chuỗi <code>X</code> và <code>Y</code> được xem là tương tự nếu chúng giống hệt nhau hoặc có thể biến chuỗi <code>X</code> thành <code>Y</code> bằng cách đổi chỗ nhiều nhất hai chữ cái ở hai vị trí khác nhau.</p>

<p>Ví dụ, <code>&quot;tars&quot;</code>&nbsp;và <code>&quot;rats&quot;</code>&nbsp;tương tự nhau (đổi chỗ tại vị trí <code>0</code> và <code>2</code>), <code>&quot;rats&quot;</code> và <code>&quot;arts&quot;</code> cũng tương tự nhau, nhưng <code>&quot;star&quot;</code> không tương tự với <code>&quot;tars&quot;</code>, <code>&quot;rats&quot;</code> hay <code>&quot;arts&quot;</code>.</p>

<p>Xét quan hệ tương tự, các chuỗi này tạo thành hai nhóm liên thông: <code>{&quot;tars&quot;, &quot;rats&quot;, &quot;arts&quot;}</code> và <code>{&quot;star&quot;}</code>.&nbsp; Lưu ý <code>&quot;tars&quot;</code> và <code>&quot;arts&quot;</code> thuộc cùng một nhóm dù chúng không tương tự nhau.&nbsp; Chính xác hơn, một từ thuộc nhóm khi và chỉ khi nó tương tự với ít nhất một từ khác trong nhóm.</p>

<p>Cho danh sách chuỗi <code>strs</code>, trong đó mỗi chuỗi là hoán vị của mọi chuỗi khác trong <code>strs</code>. Có bao nhiêu nhóm?</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;tars&quot;,&quot;rats&quot;,&quot;arts&quot;,&quot;star&quot;]
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;omv&quot;,&quot;ovm&quot;]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= strs.length &lt;= 300</code></li>
	<li><code>1 &lt;= strs[i].length &lt;= 300</code></li>
	<li><code>strs[i]</code> chỉ gồm các chữ cái viết thường.</li>
	<li>Tất cả các từ trong <code>strs</code> có cùng độ dài và là hoán vị của nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Quan hệ tương tự (khác nhau ở nhiều nhất hai vị trí) có tính bắc cầu, nên đáp án là số thành phần liên thông. Với $n\le 300$, có thể kiểm tra từng cặp kết hợp với Union-Find.
>
> Các chuỗi là hoán vị của nhau nên chỉ cần đếm số vị trí khác nhau: nếu không quá hai thì có cạnh nối chúng. Mỗi lần union thành công sẽ làm giảm số thành phần đi một.

<!-- thinking:end -->

Ta có thể xét mọi cặp chuỗi $s$ và $t$ trong danh sách. Vì $s$ và $t$ là hoán vị của nhau, nếu số ký tự khác nhau tại các vị trí tương ứng không quá $2$ thì chúng tương tự nhau. Dùng cấu trúc dữ liệu Union-Find để hợp nhất $s$ và $t$. Nếu hợp nhất thành công, số nhóm chuỗi tương tự giảm đi $1$.

Số nhóm chuỗi tương tự cuối cùng chính là số thành phần liên thông trong cấu trúc Union-Find.

Độ phức tạp thời gian là $O(n^2 \times (m + \alpha(n)))$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số chuỗi trong danh sách, $m$ là độ dài của mỗi chuỗi, còn $\alpha(n)$ là hàm Ackermann ngược, có thể xem như một hằng số rất nhỏ.

<!-- tabs:start -->

#### Python3

```python
class UnionFind:
    def __init__(self, n):
        self.p = list(range(n))
        self.size = [1] * n

    def find(self, x):
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]

    def union(self, a, b):
        pa, pb = self.find(a), self.find(b)
        if pa == pb:
            return False
        if self.size[pa] > self.size[pb]:
            self.p[pb] = pa
            self.size[pa] += self.size[pb]
        else:
            self.p[pa] = pb
            self.size[pb] += self.size[pa]
        return True


class Solution:
    def numSimilarGroups(self, strs: List[str]) -> int:
        n, m = len(strs), len(strs[0])
        uf = UnionFind(n)
        for i, s in enumerate(strs):
            for j, t in enumerate(strs[:i]):
                if sum(s[k] != t[k] for k in range(m)) <= 2 and uf.union(i, j):
                    n -= 1
        return n
```

#### Java

```java
class UnionFind {
    private final int[] p;
    private final int[] size;

    public UnionFind(int n) {
        p = new int[n];
        size = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
            size[i] = 1;
        }
    }

    public int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    public boolean union(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa == pb) {
            return false;
        }
        if (size[pa] > size[pb]) {
            p[pb] = pa;
            size[pa] += size[pb];
        } else {
            p[pa] = pb;
            size[pb] += size[pa];
        }
        return true;
    }
}

class Solution {
    public int numSimilarGroups(String[] strs) {
        int n = strs.length, m = strs[0].length();
        UnionFind uf = new UnionFind(n);
        int cnt = n;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                int diff = 0;
                for (int k = 0; k < m; ++k) {
                    if (strs[i].charAt(k) != strs[j].charAt(k)) {
                        ++diff;
                    }
                }
                if (diff <= 2 && uf.union(i, j)) {
                    --cnt;
                }
            }
        }
        return cnt;
    }
}
```

#### C++

```cpp
class UnionFind {
public:
    UnionFind(int n) {
        p = vector<int>(n);
        size = vector<int>(n, 1);
        iota(p.begin(), p.end(), 0);
    }

    bool unite(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa == pb) {
            return false;
        }
        if (size[pa] > size[pb]) {
            p[pb] = pa;
            size[pa] += size[pb];
        } else {
            p[pa] = pb;
            size[pb] += size[pa];
        }
        return true;
    }

    int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

private:
    vector<int> p, size;
};

class Solution {
public:
    int numSimilarGroups(vector<string>& strs) {
        int n = strs.size(), m = strs[0].size();
        int cnt = n;
        UnionFind uf(n);
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                int diff = 0;
                for (int k = 0; k < m; ++k) {
                    diff += strs[i][k] != strs[j][k];
                }
                if (diff <= 2 && uf.unite(i, j)) {
                    --cnt;
                }
            }
        }
        return cnt;
    }
};
```

#### Go

```go
type unionFind struct {
	p, size []int
}

func newUnionFind(n int) *unionFind {
	p := make([]int, n)
	size := make([]int, n)
	for i := range p {
		p[i] = i
		size[i] = 1
	}
	return &unionFind{p, size}
}

func (uf *unionFind) find(x int) int {
	if uf.p[x] != x {
		uf.p[x] = uf.find(uf.p[x])
	}
	return uf.p[x]
}

func (uf *unionFind) union(a, b int) bool {
	pa, pb := uf.find(a), uf.find(b)
	if pa == pb {
		return false
	}
	if uf.size[pa] > uf.size[pb] {
		uf.p[pb] = pa
		uf.size[pa] += uf.size[pb]
	} else {
		uf.p[pa] = pb
		uf.size[pb] += uf.size[pa]
	}
	return true
}

func numSimilarGroups(strs []string) int {
	n := len(strs)
	uf := newUnionFind(n)
	for i, s := range strs {
		for j, t := range strs[:i] {
			diff := 0
			for k := range s {
				if s[k] != t[k] {
					diff++
				}
			}
			if diff <= 2 && uf.union(i, j) {
				n--
			}
		}
	}
	return n
}
```

#### TypeScript

```ts
class UnionFind {
    private p: number[];
    private size: number[];

    constructor(n: number) {
        this.p = Array.from({ length: n }, (_, i) => i);
        this.size = Array(n).fill(1);
    }

    union(a: number, b: number): boolean {
        const pa = this.find(a);
        const pb = this.find(b);
        if (pa === pb) {
            return false;
        }
        if (this.size[pa] > this.size[pb]) {
            this.p[pb] = pa;
            this.size[pa] += this.size[pb];
        } else {
            this.p[pa] = pb;
            this.size[pb] += this.size[pa];
        }
        return true;
    }

    find(x: number): number {
        if (this.p[x] !== x) {
            this.p[x] = this.find(this.p[x]);
        }
        return this.p[x];
    }
}

function numSimilarGroups(strs: string[]): number {
    const n = strs.length;
    const m = strs[0].length;
    const uf = new UnionFind(n);
    let cnt = n;
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < i; ++j) {
            let diff = 0;
            for (let k = 0; k < m; ++k) {
                if (strs[i][k] !== strs[j][k]) {
                    diff++;
                }
            }
            if (diff <= 2 && uf.union(i, j)) {
                cnt--;
            }
        }
    }
    return cnt;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
