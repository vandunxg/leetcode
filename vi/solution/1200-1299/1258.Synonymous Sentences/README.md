---
comments: true
difficulty: Medium
rating: 1847
source: Biweekly Contest 13 Q3
tags:
    - Sort
    - Union Find
    - Array
    - Hash Table
    - String
    - Backtracking
---

<!-- problem:start -->

# [1258. Synonymous Sentences 🔒](https://leetcode.com/problems/synonymous-sentences)

[中文文档](/solution/1200-1299/1258.Synonymous%20Sentences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách các cặp chuỗi tương đương <code>synonyms</code>, trong đó <code>synonyms[i] = [s<sub>i</sub>, t<sub>i</sub>]</code> cho biết <code>s<sub>i</sub></code> và <code>t<sub>i</sub></code> là hai chuỗi tương đương. Bạn cũng được cho một câu <code>text</code>.</p>

<p>Trả về <em>tất cả các câu có thể tạo thành bằng cách thay từ đồng nghĩa, được <strong>sắp xếp theo thứ tự từ điển</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> synonyms = [[&quot;happy&quot;,&quot;joy&quot;],[&quot;sad&quot;,&quot;sorrow&quot;],[&quot;joy&quot;,&quot;cheerful&quot;]], text = &quot;I am happy today but was sad yesterday&quot;
<strong>Output:</strong> [&quot;I am cheerful today but was sad yesterday&quot;,&quot;I am cheerful today but was sorrow yesterday&quot;,&quot;I am happy today but was sad yesterday&quot;,&quot;I am happy today but was sorrow yesterday&quot;,&quot;I am joy today but was sad yesterday&quot;,&quot;I am joy today but was sorrow yesterday&quot;]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> synonyms = [[&quot;happy&quot;,&quot;joy&quot;],[&quot;cheerful&quot;,&quot;glad&quot;]], text = &quot;I am happy today but was sad yesterday&quot;
<strong>Output:</strong> [&quot;I am happy today but was sad yesterday&quot;,&quot;I am joy today but was sad yesterday&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= synonyms.length &lt;= 10</code></li>
	<li><code>synonyms[i].length == 2</code></li>
	<li><code>1 &lt;= s<sub>i</sub>.length,<sub> </sub>t<sub>i</sub>.length &lt;= 10</code></li>
	<li><code>s<sub>i</sub> != t<sub>i</sub></code></li>
	<li><code>text</code> gồm không quá <code>10</code> từ.</li>
	<li>Tất cả các cặp trong&nbsp;<code>synonyms</code> đều <strong>khác nhau</strong>.</li>
	<li>Các từ trong <code>text</code> được phân tách bằng một dấu cách.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Quan hệ đồng nghĩa có tính bắc cầu, nên mỗi nhóm từ tạo thành một component. Số từ và số cặp đều nhỏ: dùng union-find để hợp nhất chúng, sau đó nhóm theo root và sắp xếp theo thứ tự từ điển.
>
> Khi tạo câu, từ không thuộc nhóm nào được giữ nguyên; nếu không, ta DFS qua tất cả từ trong nhóm của nó. Các component cung cấp những từ thay thế; DFS lần lượt mở rộng tích Descartes tại từng vị trí.

<!-- thinking:end -->

Ta nhận thấy quan hệ đồng nghĩa trong bài có tính bắc cầu: nếu `a` và `b` đồng nghĩa, đồng thời `b` và `c` đồng nghĩa, thì `a` và `c` cũng đồng nghĩa. Vì vậy, ta có thể dùng union-find để tìm các connected component của những từ đồng nghĩa. Trong mỗi component, mọi từ đều đồng nghĩa với nhau và được sắp xếp theo thứ tự từ điển.

Tiếp theo, ta tách chuỗi `text` theo dấu cách thành mảng từ `sentence`. Với mỗi từ `sentence[i]`, nếu từ đó có trong các cặp đồng nghĩa, ta thay nó lần lượt bằng mọi từ trong connected component tương ứng; nếu không thì giữ nguyên. Nhờ vậy, ta có thể tạo ra tất cả các câu bằng DFS.

Ta định nghĩa hàm $dfs(i)$, bắt đầu từ từ thứ $i$ trong `sentence`, thay từ đó bằng tất cả từ trong connected component tương ứng, rồi đệ quy xử lý các từ tiếp theo.

Nếu $i$ lớn hơn hoặc bằng độ dài của `sentence`, nghĩa là ta đã xử lý hết các từ và thêm câu hiện tại vào mảng kết quả. Nếu chưa, khi `sentence[i]` không thuộc nhóm từ đồng nghĩa nào, ta giữ nguyên từ đó, thêm vào câu hiện tại rồi đệ quy xử lý các từ tiếp theo. Ngược lại, ta thay `sentence[i]` bằng từng từ trong connected component tương ứng và tiếp tục đệ quy.

Cuối cùng, trả về mảng kết quả.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số từ.

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
        if pa != pb:
            if self.size[pa] > self.size[pb]:
                self.p[pb] = pa
                self.size[pa] += self.size[pb]
            else:
                self.p[pa] = pb
                self.size[pb] += self.size[pa]


class Solution:
    def generateSentences(self, synonyms: List[List[str]], text: str) -> List[str]:
        def dfs(i):
            if i >= len(sentence):
                ans.append(' '.join(t))
                return
            if sentence[i] not in d:
                t.append(sentence[i])
                dfs(i + 1)
                t.pop()
            else:
                root = uf.find(d[sentence[i]])
                for j in g[root]:
                    t.append(words[j])
                    dfs(i + 1)
                    t.pop()

        words = list(set(chain.from_iterable(synonyms)))
        d = {w: i for i, w in enumerate(words)}
        uf = UnionFind(len(d))
        for a, b in synonyms:
            uf.union(d[a], d[b])
        g = defaultdict(list)
        for i in range(len(words)):
            g[uf.find(i)].append(i)
        for k in g.keys():
            g[k].sort(key=lambda i: words[i])
        sentence = text.split()
        ans = []
        t = []
        dfs(0)
        return ans
```

#### Java

```java
class UnionFind {
    private int[] p;
    private int[] size;

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

    public void union(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa != pb) {
            if (size[pa] > size[pb]) {
                p[pb] = pa;
                size[pa] += size[pb];
            } else {
                p[pa] = pb;
                size[pb] += size[pa];
            }
        }
    }
}

class Solution {
    private List<String> ans = new ArrayList<>();
    private List<String> t = new ArrayList<>();
    private List<String> words;
    private Map<String, Integer> d;
    private UnionFind uf;
    private List<Integer>[] g;
    private String[] sentence;

    public List<String> generateSentences(List<List<String>> synonyms, String text) {
        Set<String> ss = new HashSet<>();
        for (List<String> pairs : synonyms) {
            ss.addAll(pairs);
        }
        words = new ArrayList<>(ss);
        int n = words.size();
        d = new HashMap<>(n);
        for (int i = 0; i < words.size(); ++i) {
            d.put(words.get(i), i);
        }
        uf = new UnionFind(n);
        for (List<String> pairs : synonyms) {
            uf.union(d.get(pairs.get(0)), d.get(pairs.get(1)));
        }
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int i = 0; i < n; ++i) {
            g[uf.find(i)].add(i);
        }
        for (List<Integer> e : g) {
            e.sort((i, j) -> words.get(i).compareTo(words.get(j)));
        }
        sentence = text.split(" ");
        dfs(0);
        return ans;
    }

    private void dfs(int i) {
        if (i >= sentence.length) {
            ans.add(String.join(" ", t));
            return;
        }
        if (!d.containsKey(sentence[i])) {
            t.add(sentence[i]);
            dfs(i + 1);
            t.remove(t.size() - 1);
        } else {
            for (int j : g[uf.find(d.get(sentence[i]))]) {
                t.add(words.get(j));
                dfs(i + 1);
                t.remove(t.size() - 1);
            }
        }
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

    void unite(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa != pb) {
            if (size[pa] > size[pb]) {
                p[pb] = pa;
                size[pa] += size[pb];
            } else {
                p[pa] = pb;
                size[pb] += size[pa];
            }
        }
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
    vector<string> generateSentences(vector<vector<string>>& synonyms, string text) {
        unordered_set<string> ss;
        for (auto& pairs : synonyms) {
            ss.insert(pairs[0]);
            ss.insert(pairs[1]);
        }
        vector<string> words{ss.begin(), ss.end()};
        unordered_map<string, int> d;
        int n = words.size();
        for (int i = 0; i < n; ++i) {
            d[words[i]] = i;
        }
        UnionFind uf(n);
        for (auto& pairs : synonyms) {
            uf.unite(d[pairs[0]], d[pairs[1]]);
        }
        vector<vector<int>> g(n);
        for (int i = 0; i < n; ++i) {
            g[uf.find(i)].push_back(i);
        }
        for (int i = 0; i < n; ++i) {
            sort(g[i].begin(), g[i].end(), [&](int a, int b) {
                return words[a] < words[b];
            });
        }
        vector<string> sentence;
        string s;
        istringstream iss(text);
        while (iss >> s) {
            sentence.emplace_back(s);
        }
        vector<string> ans;
        vector<string> t;
        function<void(int)> dfs = [&](int i) {
            if (i >= sentence.size()) {
                string s;
                for (int j = 0; j < t.size(); ++j) {
                    if (j) {
                        s += " ";
                    }
                    s += t[j];
                }
                ans.emplace_back(s);
                return;
            }
            if (!d.count(sentence[i])) {
                t.emplace_back(sentence[i]);
                dfs(i + 1);
                t.pop_back();
            } else {
                for (int j : g[uf.find(d[sentence[i]])]) {
                    t.emplace_back(words[j]);
                    dfs(i + 1);
                    t.pop_back();
                }
            }
        };
        dfs(0);
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

func (uf *unionFind) union(a, b int) {
	pa, pb := uf.find(a), uf.find(b)
	if pa != pb {
		if uf.size[pa] > uf.size[pb] {
			uf.p[pb] = pa
			uf.size[pa] += uf.size[pb]
		} else {
			uf.p[pa] = pb
			uf.size[pb] += uf.size[pa]
		}
	}
}

func generateSentences(synonyms [][]string, text string) (ans []string) {
	ss := map[string]bool{}
	for _, pairs := range synonyms {
		ss[pairs[0]] = true
		ss[pairs[1]] = true
	}
	words := []string{}
	for w := range ss {
		words = append(words, w)
	}
	d := map[string]int{}
	for i, w := range words {
		d[w] = i
	}
	uf := newUnionFind(len(words))
	for _, pairs := range synonyms {
		uf.union(d[pairs[0]], d[pairs[1]])
	}
	g := make([][]int, len(words))
	for i := range g {
		g[uf.find(i)] = append(g[uf.find(i)], i)
	}
	for i := range g {
		sort.Slice(g[i], func(a, b int) bool { return words[g[i][a]] < words[g[i][b]] })
	}
	t := []string{}
	sentences := strings.Split(text, " ")
	var dfs func(int)
	dfs = func(i int) {
		if i >= len(sentences) {
			ans = append(ans, strings.Join(t, " "))
			return
		}
		if _, ok := ss[sentences[i]]; !ok {
			t = append(t, sentences[i])
			dfs(i + 1)
			t = t[:len(t)-1]
			return
		} else {
			for _, j := range g[uf.find(d[sentences[i]])] {
				t = append(t, words[j])
				dfs(i + 1)
				t = t[:len(t)-1]
			}
		}
	}
	dfs(0)
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
