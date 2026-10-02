---
comments: true
difficulty: Medium
tags:
    - Union Find
    - Graph
    - Array
    - String
---

<!-- problem:start -->

# [990. Satisfiability of Equality Equations](https://leetcode.com/problems/satisfiability-of-equality-equations)

[中文文档](/solution/0900-0999/0990.Satisfiability%20of%20Equality%20Equations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>equations</code> biểu diễn các quan hệ giữa các biến. Mỗi chuỗi <code>equations[i]</code> có độ dài <code>4</code> và thuộc một trong hai dạng: <code>&quot;x<sub>i</sub>==y<sub>i</sub>&quot;</code> hoặc <code>&quot;x<sub>i</sub>!=y<sub>i</sub>&quot;</code>. Trong đó, <code>x<sub>i</sub></code> và <code>y<sub>i</sub></code> là các chữ cái viết thường (có thể giống nhau), đại diện cho tên biến gồm một chữ cái.</p>

<p>Trả về <code>true</code><em> nếu có thể gán số nguyên cho các tên biến sao cho tất cả phương trình đã cho đều đúng, nếu không thì trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> equations = [&quot;a==b&quot;,&quot;b!=a&quot;]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Chẳng hạn, nếu gán a = 1 và b = 1 thì phương trình đầu tiên đúng, nhưng phương trình thứ hai không đúng.
Không có cách gán giá trị cho các biến để cả hai phương trình cùng đúng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> equations = [&quot;b==a&quot;,&quot;a==b&quot;]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta có thể gán a = 1 và b = 1 để cả hai phương trình đều đúng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= equations.length &lt;= 500</code></li>
	<li><code>equations[i].length == 4</code></li>
	<li><code>equations[i][0]</code> là một chữ cái viết thường.</li>
	<li><code>equations[i][1]</code> là <code>&#39;=&#39;</code> hoặc <code>&#39;!&#39;</code>.</li>
	<li><code>equations[i][2]</code> là <code>&#39;=&#39;</code>.</li>
	<li><code>equations[i][3]</code> là một chữ cái viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một tập phương trình dạng $a==b$ / $a!=b$ phải có thể đồng thời đúng. Quan hệ bằng nhau có tính bắc cầu, nên cần gộp các biến bằng nhau trước. Union-find gộp mọi cặp `==`, sau đó kiểm tra từng cặp `!=`; nếu hai biến thuộc cùng một component thì hệ phương trình vô nghiệm.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def equationsPossible(self, equations: List[str]) -> bool:
        def find(x):
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        p = list(range(26))
        for e in equations:
            a, b = ord(e[0]) - ord('a'), ord(e[-1]) - ord('a')
            if e[1] == '=':
                p[find(a)] = find(b)
        for e in equations:
            a, b = ord(e[0]) - ord('a'), ord(e[-1]) - ord('a')
            if e[1] == '!' and find(a) == find(b):
                return False
        return True
```

#### Java

```java
class Solution {
    private int[] p;

    public boolean equationsPossible(String[] equations) {
        p = new int[26];
        for (int i = 0; i < 26; ++i) {
            p[i] = i;
        }
        for (String e : equations) {
            int a = e.charAt(0) - 'a', b = e.charAt(3) - 'a';
            if (e.charAt(1) == '=') {
                p[find(a)] = find(b);
            }
        }
        for (String e : equations) {
            int a = e.charAt(0) - 'a', b = e.charAt(3) - 'a';
            if (e.charAt(1) == '!' && find(a) == find(b)) {
                return false;
            }
        }
        return true;
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
    vector<int> p;

    bool equationsPossible(vector<string>& equations) {
        p.resize(26);
        for (int i = 0; i < 26; ++i) p[i] = i;
        for (auto& e : equations) {
            int a = e[0] - 'a', b = e[3] - 'a';
            if (e[1] == '=') p[find(a)] = find(b);
        }
        for (auto& e : equations) {
            int a = e[0] - 'a', b = e[3] - 'a';
            if (e[1] == '!' && find(a) == find(b)) return false;
        }
        return true;
    }

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }
};
```

#### Go

```go
func equationsPossible(equations []string) bool {
	p := make([]int, 26)
	for i := 1; i < 26; i++ {
		p[i] = i
	}
	var find func(x int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	for _, e := range equations {
		a, b := int(e[0]-'a'), int(e[3]-'a')
		if e[1] == '=' {
			p[find(a)] = find(b)
		}
	}
	for _, e := range equations {
		a, b := int(e[0]-'a'), int(e[3]-'a')
		if e[1] == '!' && find(a) == find(b) {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
class UnionFind {
    private parent: number[];

    constructor() {
        this.parent = Array.from({ length: 26 }).map((_, i) => i);
    }

    find(index: number) {
        if (this.parent[index] === index) {
            return index;
        }
        this.parent[index] = this.find(this.parent[index]);
        return this.parent[index];
    }

    union(index1: number, index2: number) {
        this.parent[this.find(index1)] = this.find(index2);
    }
}

function equationsPossible(equations: string[]): boolean {
    const uf = new UnionFind();
    for (const [a, s, _, b] of equations) {
        if (s === '=') {
            const index1 = a.charCodeAt(0) - 'a'.charCodeAt(0);
            const index2 = b.charCodeAt(0) - 'a'.charCodeAt(0);
            uf.union(index1, index2);
        }
    }
    for (const [a, s, _, b] of equations) {
        if (s === '!') {
            const index1 = a.charCodeAt(0) - 'a'.charCodeAt(0);
            const index2 = b.charCodeAt(0) - 'a'.charCodeAt(0);
            if (uf.find(index1) === uf.find(index2)) {
                return false;
            }
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
