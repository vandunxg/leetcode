---
comments: true
difficulty: Medium
rating: 1654
source: Biweekly Contest 13 Q2
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Hash Table
    - String
    - Lowest Common Ancestor
    - Binary Lifting
---

<!-- problem:start -->

# [1257. Smallest Common Region 🔒](https://leetcode.com/problems/smallest-common-region)

[中文文档](/solution/1200-1299/1257.Smallest%20Common%20Region/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách các danh sách <code>regions</code>, trong đó vùng đầu tiên của mỗi danh sách <strong>trực tiếp</strong> bao gồm tất cả vùng còn lại trong danh sách đó.</p>

<p>Nếu vùng <code>x</code> <em>trực tiếp</em> bao gồm vùng <code>y</code>, và vùng <code>y</code> <em>trực tiếp</em> bao gồm vùng <code>z</code>, thì vùng <code>x</code> được xem là <strong>gián tiếp</strong> bao gồm vùng <code>z</code>. Lưu ý rằng vùng <code>x</code> cũng <strong>gián tiếp</strong> bao gồm mọi vùng được <code>y</code> <strong>gián tiếp</strong> bao gồm.</p>

<p>Hiển nhiên, nếu vùng <code>x</code> bao gồm (theo cách <em>trực tiếp</em> hoặc <em>gián tiếp</em>) vùng <code>y</code>, thì kích thước của <code>x</code> lớn hơn hoặc bằng <code>y</code>. Theo định nghĩa, vùng <code>x</code> cũng bao gồm chính nó.</p>

<p>Cho hai vùng <code>region1</code> và <code>region2</code>, hãy trả về <em>vùng nhỏ nhất bao gồm cả hai</em>.</p>

<p>Đảm bảo luôn tồn tại vùng nhỏ nhất như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:
</strong>regions = [[&quot;Earth&quot;,&quot;North America&quot;,&quot;South America&quot;],
[&quot;North America&quot;,&quot;United States&quot;,&quot;Canada&quot;],
[&quot;United States&quot;,&quot;New York&quot;,&quot;Boston&quot;],
[&quot;Canada&quot;,&quot;Ontario&quot;,&quot;Quebec&quot;],
[&quot;South America&quot;,&quot;Brazil&quot;]],
region1 = &quot;Quebec&quot;,
region2 = &quot;New York&quot;
<strong>Đầu ra:</strong> &quot;North America&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> regions = [[&quot;Earth&quot;, &quot;North America&quot;, &quot;South America&quot;],[&quot;North America&quot;, &quot;United States&quot;, &quot;Canada&quot;],[&quot;United States&quot;, &quot;New York&quot;, &quot;Boston&quot;],[&quot;Canada&quot;, &quot;Ontario&quot;, &quot;Quebec&quot;],[&quot;South America&quot;, &quot;Brazil&quot;]], region1 = &quot;Canada&quot;, region2 = &quot;South America&quot;
<strong>Đầu ra:</strong> &quot;Earth&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= regions.length &lt;= 10<sup>4</sup></code></li>
	<li><code>2 &lt;= regions[i].length &lt;= 20</code></li>
	<li><code>1 &lt;= regions[i][j].length, region1.length, region2.length &lt;= 20</code></li>
	<li><code>region1 != region2</code></li>
	<li><code>regions[i][j]</code>, <code>region1</code> và <code>region2</code> chỉ gồm các chữ cái tiếng Anh.</li>
	<li>Dữ liệu đầu vào được tạo sao cho tồn tại một vùng bao gồm tất cả các vùng khác, trực tiếp hoặc gián tiếp.</li>
	<li>Một vùng không thể được bao gồm trực tiếp trong nhiều hơn một vùng.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table

<!-- thinking:start -->

> **Tư duy**
>
> Các vùng tạo thành một cây; ta cần tìm LCA của hai vùng. Với $n \le 10^4$, có thể lần ngược lên root. Parent của mỗi vùng con là tên đầu tiên trong danh sách vùng tương ứng.
>
> Đi từ $region1$ lên root và lưu các vùng vào một set; sau đó đi từ $region2$ lên cho đến khi gặp vùng đầu tiên đã có trong set. Map lưu parent, còn set giúp phát hiện điểm giao đầu tiên.

<!-- thinking:end -->

Dùng hash table $\textit{g}$ để lưu vùng cha của mỗi vùng. Bắt đầu từ $\textit{region1}$, lần lượt đi lên qua các vùng cha cho đến root và lưu chúng vào set $\textit{s}$. Tiếp theo, bắt đầu từ $\textit{region2}$ và đi lên cho đến khi gặp vùng đầu tiên thuộc set $\textit{s}$; đó là vùng chung nhỏ nhất.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là tổng số vùng trong danh sách $\textit{regions}$. 

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findSmallestRegion(
        self, regions: List[List[str]], region1: str, region2: str
    ) -> str:
        g = {}
        for r in regions:
            x = r[0]
            for y in r[1:]:
                g[y] = x
        s = set()
        x = region1
        while x in g:
            s.add(x)
            x = g[x]
        x = region2
        while x in g and x not in s:
            x = g[x]
        return x
```

#### Java

```java
class Solution {
    public String findSmallestRegion(List<List<String>> regions, String region1, String region2) {
        Map<String, String> g = new HashMap<>();
        for (var r : regions) {
            String x = r.get(0);
            for (String y : r.subList(1, r.size())) {
                g.put(y, x);
            }
        }
        Set<String> s = new HashSet<>();
        for (String x = region1; x != null; x = g.get(x)) {
            s.add(x);
        }
        String x = region2;
        while (g.get(x) != null && !s.contains(x)) {
            x = g.get(x);
        }
        return x;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string findSmallestRegion(vector<vector<string>>& regions, string region1, string region2) {
        unordered_map<string, string> g;
        for (const auto& r : regions) {
            string x = r[0];
            for (size_t i = 1; i < r.size(); ++i) {
                g[r[i]] = x;
            }
        }
        unordered_set<string> s;
        for (string x = region1; !x.empty(); x = g[x]) {
            s.insert(x);
        }
        string x = region2;
        while (!g[x].empty() && s.find(x) == s.end()) {
            x = g[x];
        }
        return x;
    }
};
```

#### Go

```go
func findSmallestRegion(regions [][]string, region1 string, region2 string) string {
	g := make(map[string]string)

	for _, r := range regions {
		x := r[0]
		for _, y := range r[1:] {
			g[y] = x
		}
	}

	s := make(map[string]bool)
	for x := region1; x != ""; x = g[x] {
		s[x] = true
	}

	x := region2
	for g[x] != "" && !s[x] {
		x = g[x]
	}

	return x
}
```

#### TypeScript

```ts
function findSmallestRegion(regions: string[][], region1: string, region2: string): string {
    const g: Record<string, string> = {};

    for (const r of regions) {
        const x = r[0];
        for (const y of r.slice(1)) {
            g[y] = x;
        }
    }

    const s: Set<string> = new Set();
    for (let x: string = region1; x !== undefined; x = g[x]) {
        s.add(x);
    }

    let x: string = region2;
    while (g[x] !== undefined && !s.has(x)) {
        x = g[x];
    }

    return x;
}
```

#### Rust

```rust
use std::collections::{HashMap, HashSet};

impl Solution {
    pub fn find_smallest_region(regions: Vec<Vec<String>>, region1: String, region2: String) -> String {
        let mut g: HashMap<String, String> = HashMap::new();

        for r in &regions {
            let x = &r[0];
            for y in &r[1..] {
                g.insert(y.clone(), x.clone());
            }
        }

        let mut s: HashSet<String> = HashSet::new();
        let mut x = Some(region1);
        while let Some(region) = x {
            s.insert(region.clone());
            x = g.get(&region).cloned();
        }

        let mut x = Some(region2);
        while let Some(region) = x {
            if s.contains(&region) {
                return region;
            }
            x = g.get(&region).cloned();
        }

        String::new()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
