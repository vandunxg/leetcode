---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [17.07. Baby Names](https://leetcode.cn/problems/baby-names-lcci)

[中文文档](/lcci/17.07.Baby%20Names/README.md)

## Mô tả

<!-- description:start -->

<p>Mỗi năm, chính phủ công bố danh sách 10000 tên em bé phổ biến nhất và tần suất của chúng (số em bé có tên đó). Vấn đề duy nhất là một số tên có nhiều cách viết. Ví dụ, &quot;John&quot; và &#39;&#39;Jon&quot; về cơ bản là cùng một tên nhưng lại được liệt kê riêng trong danh sách. Cho hai danh sách, một danh sách gồm tên/tần suất và danh sách còn lại gồm các cặp tên tương đương, hãy viết một thuật toán để in ra danh sách mới chứa tần suất thực của từng tên. Lưu ý rằng nếu John và Jon là từ đồng nghĩa, còn Jon và Johnny là từ đồng nghĩa, thì John và Johnny cũng là từ đồng nghĩa. (Quan hệ này có tính bắc cầu và đối xứng.) Trong danh sách cuối cùng, chọn tên có <strong>thứ tự từ điển nhỏ nhất</strong> làm tên &quot;thực&quot;.</p>

<p><strong>Ví dụ: </strong></p>

<pre>

<strong>Đầu vào: </strong>names = [&quot;John(15)&quot;,&quot;Jon(12)&quot;,&quot;Chris(13)&quot;,&quot;Kris(4)&quot;,&quot;Christopher(19)&quot;], synonyms = [&quot;(Jon,John)&quot;,&quot;(John,Johnny)&quot;,&quot;(Chris,Kris)&quot;,&quot;(Chris,Christopher)&quot;]

<strong>Đầu ra: </strong>[&quot;John(27)&quot;,&quot;Chris(36)&quot;]</pre>

<p>Lưu ý:</p>

<ul>
	<li><code>names.length &lt;= 100000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Các tên đồng nghĩa được nối với nhau; tần suất được cộng trong mỗi thành phần và tên có thứ tự từ điển nhỏ nhất đại diện cho thành phần đó. Có thể dùng union-find; DFS cũng vậy.
>
> Xây dựng một đồ thị vô hướng và một bảng tần suất, sau đó tìm kiếm từng thành phần chưa được duyệt.
>
> $dfs$ trả về tên nhỏ nhất và tổng tần suất. Sau khi phân tích `Name(freq)` và `(a,b)`, chạy nó một lần cho mỗi tên chưa được duyệt trong $s$.

<!-- thinking:end -->

Với mỗi cặp tên đồng nghĩa, ta thiết lập các cạnh hai chiều giữa hai tên và lưu chúng trong danh sách kề $g$. Sau đó, ta duyệt qua tất cả tên, lưu chúng vào tập hợp $s$ và lưu tần suất của chúng trong hash table $cnt$.

Tiếp theo, ta duyệt qua từng tên trong tập hợp $s$. Nếu tên đó chưa được duyệt, ta thực hiện tìm kiếm theo chiều sâu để tìm tất cả tên trong thành phần liên thông chứa tên đó. Ta sử dụng tên có thứ tự từ điển nhỏ nhất làm tên thực, còn tổng tần suất của chúng là tần suất của tên thực. Sau đó, ta lưu tên và tần suất này vào mảng kết quả.

Sau khi duyệt qua tất cả tên, mảng kết quả chính là đáp án cần tìm.

Độ phức tạp thời gian là $O(n + m)$, và độ phức tạp không gian là $O(n + m)$. Trong đó $n$ và $m$ lần lượt là độ dài của mảng tên và mảng từ đồng nghĩa.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def trulyMostPopular(self, names: List[str], synonyms: List[str]) -> List[str]:
        def dfs(a):
            vis.add(a)
            mi, x = a, cnt[a]
            for b in g[a]:
                if b not in vis:
                    t, y = dfs(b)
                    if mi > t:
                        mi = t
                    x += y
            return mi, x

        g = defaultdict(list)
        for e in synonyms:
            a, b = e[1:-1].split(',')
            g[a].append(b)
            g[b].append(a)
        s = set()
        cnt = defaultdict(int)
        for x in names:
            name, freq = x[:-1].split("(")
            s.add(name)
            cnt[name] = int(freq)
        vis = set()
        ans = []
        for name in s:
            if name not in vis:
                name, freq = dfs(name)
                ans.append(f"{name}({freq})")
        return ans
```

#### Java

```java
class Solution {
    private Map<String, List<String>> g = new HashMap<>();
    private Map<String, Integer> cnt = new HashMap<>();
    private Set<String> vis = new HashSet<>();
    private int freq;

    public String[] trulyMostPopular(String[] names, String[] synonyms) {
        for (String pairs : synonyms) {
            String[] pair = pairs.substring(1, pairs.length() - 1).split(",");
            String a = pair[0], b = pair[1];
            g.computeIfAbsent(a, k -> new ArrayList<>()).add(b);
            g.computeIfAbsent(b, k -> new ArrayList<>()).add(a);
        }
        Set<String> s = new HashSet<>();
        for (String x : names) {
            int i = x.indexOf('(');
            String name = x.substring(0, i);
            s.add(name);
            cnt.put(name, Integer.parseInt(x.substring(i + 1, x.length() - 1)));
        }
        List<String> res = new ArrayList<>();
        for (String name : s) {
            if (!vis.contains(name)) {
                freq = 0;
                name = dfs(name);
                res.add(name + "(" + freq + ")");
            }
        }
        return res.toArray(new String[0]);
    }

    private String dfs(String a) {
        String mi = a;
        vis.add(a);
        freq += cnt.getOrDefault(a, 0);
        for (String b : g.getOrDefault(a, new ArrayList<>())) {
            if (!vis.contains(b)) {
                String t = dfs(b);
                if (t.compareTo(mi) < 0) {
                    mi = t;
                }
            }
        }
        return mi;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> trulyMostPopular(vector<string>& names, vector<string>& synonyms) {
        unordered_map<string, vector<string>> g;
        unordered_map<string, int> cnt;
        for (auto& e : synonyms) {
            int i = e.find(',');
            string a = e.substr(1, i - 1);
            string b = e.substr(i + 1, e.size() - i - 2);
            g[a].emplace_back(b);
            g[b].emplace_back(a);
        }
        unordered_set<string> s;
        for (auto& e : names) {
            int i = e.find('(');
            string name = e.substr(0, i);
            s.insert(name);
            cnt[name] += stoi(e.substr(i + 1, e.size() - i - 2));
        }
        unordered_set<string> vis;
        int freq = 0;

        function<string(string)> dfs = [&](string a) -> string {
            string res = a;
            vis.insert(a);
            freq += cnt[a];
            for (auto& b : g[a]) {
                if (!vis.count(b)) {
                    string t = dfs(b);
                    if (t < res) {
                        res = move(t);
                    }
                }
            }
            return move(res);
        };

        vector<string> ans;
        for (auto& name : s) {
            if (!vis.count(name)) {
                freq = 0;
                string x = dfs(name);
                ans.emplace_back(x + "(" + to_string(freq) + ")");
            }
        }
        return ans;
    }
};
```

#### Go

```go
func trulyMostPopular(names []string, synonyms []string) (ans []string) {
	g := map[string][]string{}
	for _, s := range synonyms {
		i := strings.Index(s, ",")
		a, b := s[1:i], s[i+1:len(s)-1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	s := map[string]struct{}{}
	cnt := map[string]int{}
	for _, e := range names {
		i := strings.Index(e, "(")
		name, num := e[:i], e[i+1:len(e)-1]
		x, _ := strconv.Atoi(num)
		cnt[name] += x
		s[name] = struct{}{}
	}
	freq := 0
	vis := map[string]struct{}{}
	var dfs func(string) string
	dfs = func(a string) string {
		vis[a] = struct{}{}
		freq += cnt[a]
		res := a
		for _, b := range g[a] {
			if _, ok := vis[b]; !ok {
				t := dfs(b)
				if t < res {
					res = t
				}
			}
		}
		return res
	}
	for name := range s {
		if _, ok := vis[name]; !ok {
			freq = 0
			root := dfs(name)
			ans = append(ans, root+"("+strconv.Itoa(freq)+")")
		}
	}
	return
}
```

#### TypeScript

```ts
function trulyMostPopular(names: string[], synonyms: string[]): string[] {
    const map = new Map<string, string>();
    for (const synonym of synonyms) {
        const [k1, k2] = [...synonym]
            .slice(1, synonym.length - 1)
            .join('')
            .split(',');
        const [v1, v2] = [map.get(k1) ?? k1, map.get(k2) ?? k2];
        const min = v1 < v2 ? v1 : v2;
        const max = v1 < v2 ? v2 : v1;
        map.set(k1, min);
        map.set(k2, min);
        for (const [k, v] of map.entries()) {
            if (v === max) {
                map.set(k, min);
            }
        }
    }

    const keyCount = new Map<string, number>();
    for (const name of names) {
        const num = name.match(/\d+/)[0];
        const k = name.split('(')[0];
        const key = map.get(k) ?? k;
        keyCount.set(key, (keyCount.get(key) ?? 0) + Number(num));
    }
    return [...keyCount.entries()].map(([k, v]) => `${k}(${v})`);
}
```

#### Swift

```swift
class Solution {
    private var graph = [String: [String]]()
    private var count = [String: Int]()
    private var visited = Set<String>()
    private var freq: Int = 0

    func trulyMostPopular(_ names: [String], _ synonyms: [String]) -> [String] {
        for pair in synonyms {
            let cleanPair = pair.dropFirst().dropLast()
            let parts = cleanPair.split(separator: ",").map(String.init)
            let a = parts[0], b = parts[1]
            graph[a, default: []].append(b)
            graph[b, default: []].append(a)
        }

        var namesSet = Set<String>()
        for name in names {
            let index = name.firstIndex(of: "(")!
            let realName = String(name[..<index])
            namesSet.insert(realName)
            let num = Int(name[name.index(after: index)..<name.index(before: name.endIndex)])!
            count[realName] = num
        }

        var result = [String]()
        for name in namesSet {
            if !visited.contains(name) {
                freq = 0
                let representative = dfs(name)
                result.append("\(representative)(\(freq))")
            }
        }

        return result
    }

    private func dfs(_ name: String) -> String {
        var minName = name
        visited.insert(name)
        freq += count[name, default: 0]
        for neighbor in graph[name, default: []] {
            if !visited.contains(neighbor) {
                let temp = dfs(neighbor)
                if temp < minName {
                    minName = temp
                }
            }
        }
        return minName
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
