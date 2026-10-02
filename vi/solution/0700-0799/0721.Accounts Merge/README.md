---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [721. Accounts Merge](https://leetcode.com/problems/accounts-merge)

[中文文档](/solution/0700-0799/0721.Accounts%20Merge/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách <code>accounts</code>, trong đó mỗi phần tử <code>accounts[i]</code> là danh sách chuỗi: phần tử đầu tiên <code>accounts[i][0]</code> là tên, các phần tử còn lại là <strong>email</strong> của tài khoản đó.</p>

<p>Hãy gộp các tài khoản này. Hai tài khoản chắc chắn thuộc cùng một người nếu chúng có chung ít nhất một email. Lưu ý rằng hai tài khoản trùng tên vẫn có thể thuộc về hai người khác nhau vì nhiều người có thể cùng tên. Ban đầu, một người có thể có bất kỳ số lượng tài khoản nào, nhưng tất cả tài khoản của người đó chắc chắn có cùng tên.</p>

<p>Sau khi gộp, hãy trả về các tài khoản theo định dạng sau: phần tử đầu tiên của mỗi tài khoản là tên, các phần tử còn lại là email được <strong>sắp xếp</strong>. Có thể trả về các tài khoản theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> accounts = [[&quot;John&quot;,&quot;johnsmith@mail.com&quot;,&quot;john_newyork@mail.com&quot;],[&quot;John&quot;,&quot;johnsmith@mail.com&quot;,&quot;john00@mail.com&quot;],[&quot;Mary&quot;,&quot;mary@mail.com&quot;],[&quot;John&quot;,&quot;johnnybravo@mail.com&quot;]]
<strong>Đầu ra:</strong> [[&quot;John&quot;,&quot;john00@mail.com&quot;,&quot;john_newyork@mail.com&quot;,&quot;johnsmith@mail.com&quot;],[&quot;Mary&quot;,&quot;mary@mail.com&quot;],[&quot;John&quot;,&quot;johnnybravo@mail.com&quot;]]
<strong>Giải thích:</strong>
John ở tài khoản thứ nhất và thứ hai là cùng một người vì họ có chung email &quot;johnsmith@mail.com&quot;.
John ở tài khoản thứ ba và Mary là hai người khác nhau vì không email nào của họ được dùng trong tài khoản khác.
Ta có thể trả về các danh sách theo bất kỳ thứ tự nào. Ví dụ, đáp án [[&#39;Mary&#39;, &#39;mary@mail.com&#39;], [&#39;John&#39;, &#39;johnnybravo@mail.com&#39;], 
[&#39;John&#39;, &#39;john00@mail.com&#39;, &#39;john_newyork@mail.com&#39;, &#39;johnsmith@mail.com&#39;]] vẫn được chấp nhận.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> accounts = [[&quot;Gabe&quot;,&quot;Gabe0@m.co&quot;,&quot;Gabe3@m.co&quot;,&quot;Gabe1@m.co&quot;],[&quot;Kevin&quot;,&quot;Kevin3@m.co&quot;,&quot;Kevin5@m.co&quot;,&quot;Kevin0@m.co&quot;],[&quot;Ethan&quot;,&quot;Ethan5@m.co&quot;,&quot;Ethan4@m.co&quot;,&quot;Ethan0@m.co&quot;],[&quot;Hanzo&quot;,&quot;Hanzo3@m.co&quot;,&quot;Hanzo1@m.co&quot;,&quot;Hanzo0@m.co&quot;],[&quot;Fern&quot;,&quot;Fern5@m.co&quot;,&quot;Fern1@m.co&quot;,&quot;Fern0@m.co&quot;]]
<strong>Đầu ra:</strong> [[&quot;Ethan&quot;,&quot;Ethan0@m.co&quot;,&quot;Ethan4@m.co&quot;,&quot;Ethan5@m.co&quot;],[&quot;Gabe&quot;,&quot;Gabe0@m.co&quot;,&quot;Gabe1@m.co&quot;,&quot;Gabe3@m.co&quot;],[&quot;Hanzo&quot;,&quot;Hanzo0@m.co&quot;,&quot;Hanzo1@m.co&quot;,&quot;Hanzo3@m.co&quot;],[&quot;Kevin&quot;,&quot;Kevin0@m.co&quot;,&quot;Kevin3@m.co&quot;,&quot;Kevin5@m.co&quot;],[&quot;Fern&quot;,&quot;Fern0@m.co&quot;,&quot;Fern1@m.co&quot;,&quot;Fern5@m.co&quot;]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= accounts.length &lt;= 1000</code></li>
	<li><code>2 &lt;= accounts[i].length &lt;= 10</code></li>
	<li><code>1 &lt;= accounts[i][j].length &lt;= 30</code></li>
	<li><code>accounts[i][0]</code> chỉ gồm các chữ cái tiếng Anh.</li>
	<li><code>accounts[i][j] (for j &gt; 0)</code> là email hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Các tài khoản của cùng một người có email chung, cần được gộp lại rồi sắp xếp. Với khoảng $1000$ tài khoản, so sánh từng cặp tập hợp khá bất tiện; đồng thời phải giữ tên gắn với tài khoản.
>
> Quan hệ liên thông nằm giữa các tài khoản: hai chỉ số được nối nếu tài khoản tương ứng có email chung. Dùng Union-Find trên id tài khoản cùng map ánh xạ email về tài khoản đầu tiên sử dụng email đó để tìm các cạnh nối.
>
> Nhóm email theo root, lấy tên từ tài khoản root rồi sắp xếp email. Path compression giúp độ phức tạp gần tuyến tính-logarit.

<!-- thinking:end -->

Theo đề bài, ta có thể dùng cấu trúc dữ liệu Union-Find để gộp các tài khoản có cùng địa chỉ email. Các bước cụ thể như sau:

Đầu tiên, ta duyệt tất cả tài khoản. Với tài khoản thứ $i$, ta duyệt từng địa chỉ email. Nếu email đã có trong hash table $\textit{d}$, ta dùng Union-Find để hợp nhất chỉ số $i$ của tài khoản với chỉ số tài khoản đã dùng email này trước đó; nếu chưa, ta ánh xạ email về chỉ số tài khoản $i$.

Tiếp theo, ta duyệt lại tất cả tài khoản. Với tài khoản thứ $i$, ta dùng Union-Find tìm root node của nó, rồi thêm mọi địa chỉ email của tài khoản vào hash table $\textit{g}$, trong đó key là root node và value là các địa chỉ email của tài khoản.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số tài khoản.

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
    def accountsMerge(self, accounts: List[List[str]]) -> List[List[str]]:
        uf = UnionFind(len(accounts))
        d = {}
        for i, (_, *emails) in enumerate(accounts):
            for email in emails:
                if email in d:
                    uf.union(i, d[email])
                else:
                    d[email] = i
        g = defaultdict(set)
        for i, (_, *emails) in enumerate(accounts):
            root = uf.find(i)
            g[root].update(emails)
        return [[accounts[root][0]] + sorted(emails) for root, emails in g.items()]
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
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        int n = accounts.size();
        UnionFind uf = new UnionFind(n);
        Map<String, Integer> d = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            for (int j = 1; j < accounts.get(i).size(); ++j) {
                String email = accounts.get(i).get(j);
                if (d.containsKey(email)) {
                    uf.union(i, d.get(email));
                } else {
                    d.put(email, i);
                }
            }
        }
        Map<Integer, Set<String>> g = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            int root = uf.find(i);
            g.computeIfAbsent(root, k -> new HashSet<>())
                .addAll(accounts.get(i).subList(1, accounts.get(i).size()));
        }
        List<List<String>> ans = new ArrayList<>();
        for (var e : g.entrySet()) {
            List<String> emails = new ArrayList<>(e.getValue());
            Collections.sort(emails);
            ans.add(new ArrayList<>());
            ans.get(ans.size() - 1).add(accounts.get(e.getKey()).get(0));
            ans.get(ans.size() - 1).addAll(emails);
        }
        return ans;
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
    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
        int n = accounts.size();
        UnionFind uf(n);
        unordered_map<string, int> d;
        for (int i = 0; i < n; ++i) {
            for (int j = 1; j < accounts[i].size(); ++j) {
                const string& email = accounts[i][j];
                if (d.find(email) != d.end()) {
                    uf.unite(i, d[email]);
                } else {
                    d[email] = i;
                }
            }
        }
        unordered_map<int, set<string>> g;
        for (int i = 0; i < n; ++i) {
            int root = uf.find(i);
            g[root].insert(accounts[i].begin() + 1, accounts[i].end());
        }
        vector<vector<string>> ans;
        for (const auto& [root, s] : g) {
            vector<string> emails(s.begin(), s.end());
            emails.insert(emails.begin(), accounts[root][0]);
            ans.push_back(emails);
        }
        return ans;
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

func accountsMerge(accounts [][]string) (ans [][]string) {
	n := len(accounts)
	uf := newUnionFind(n)
	d := make(map[string]int)
	for i := 0; i < n; i++ {
		for _, email := range accounts[i][1:] {
			if j, ok := d[email]; ok {
				uf.union(i, j)
			} else {
				d[email] = i
			}
		}
	}
	g := make(map[int]map[string]struct{})
	for i := 0; i < n; i++ {
		root := uf.find(i)
		if _, ok := g[root]; !ok {
			g[root] = make(map[string]struct{})
		}
		for _, email := range accounts[i][1:] {
			g[root][email] = struct{}{}
		}
	}
	for root, s := range g {
		emails := []string{}
		for email := range s {
			emails = append(emails, email)
		}
		sort.Strings(emails)
		account := append([]string{accounts[root][0]}, emails...)
		ans = append(ans, account)
	}
	return
}
```

#### TypeScript

```ts
class UnionFind {
    private p: number[];
    private size: number[];

    constructor(n: number) {
        this.p = new Array(n);
        this.size = new Array(n);
        for (let i = 0; i < n; ++i) {
            this.p[i] = i;
            this.size[i] = 1;
        }
    }

    find(x: number): number {
        if (this.p[x] !== x) {
            this.p[x] = this.find(this.p[x]);
        }
        return this.p[x];
    }

    union(a: number, b: number): boolean {
        let pa = this.find(a),
            pb = this.find(b);
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
}

function accountsMerge(accounts: string[][]): string[][] {
    const n = accounts.length;
    const uf = new UnionFind(n);
    const d = new Map<string, number>();

    for (let i = 0; i < n; ++i) {
        for (let j = 1; j < accounts[i].length; ++j) {
            const email = accounts[i][j];
            if (d.has(email)) {
                uf.union(i, d.get(email)!);
            } else {
                d.set(email, i);
            }
        }
    }

    const g = new Map<number, Set<string>>();
    for (let i = 0; i < n; ++i) {
        const root = uf.find(i);
        if (!g.has(root)) {
            g.set(root, new Set<string>());
        }
        const emailSet = g.get(root)!;
        for (let j = 1; j < accounts[i].length; ++j) {
            emailSet.add(accounts[i][j]);
        }
    }

    const ans: string[][] = [];
    for (const [root, emails] of g.entries()) {
        const emailList = Array.from(emails).sort();
        const mergedAccount = [accounts[root][0], ...emailList];
        ans.push(mergedAccount);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
