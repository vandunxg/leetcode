---
comments: true
difficulty: Medium
rating: 1850
source: Biweekly Contest 6 Q3
tags:
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [1152. Analyze User Website Visit Pattern 🔒](https://leetcode.com/problems/analyze-user-website-visit-pattern)

[中文文档](/solution/1100-1199/1152.Analyze%20User%20Website%20Visit%20Pattern/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng chuỗi <code>username</code> và <code>website</code>, cùng một mảng số nguyên <code>timestamp</code>. Các mảng có cùng độ dài; bộ ba <code>[username[i], website[i], timestamp[i]]</code> cho biết người dùng <code>username[i]</code> đã truy cập website <code>website[i]</code> tại thời điểm <code>timestamp[i]</code>.</p>

<p>Một <strong>pattern</strong> là danh sách gồm ba website (không nhất thiết phải khác nhau).</p>

<ul>
	<li>Ví dụ, <code>[&quot;home&quot;, &quot;away&quot;, &quot;love&quot;]</code>, <code>[&quot;leetcode&quot;, &quot;love&quot;, &quot;leetcode&quot;]</code> và <code>[&quot;luffy&quot;, &quot;luffy&quot;, &quot;luffy&quot;]</code> đều là các pattern.</li>
</ul>

<p><strong>Score</strong> của một <strong>pattern</strong> là số người dùng đã truy cập tất cả website trong pattern theo đúng thứ tự xuất hiện.</p>

<ul>
	<li>Ví dụ, nếu pattern là <code>[&quot;home&quot;, &quot;away&quot;, &quot;love&quot;]</code>, score là số người dùng <code>x</code> đã truy cập <code>&quot;home&quot;</code>, sau đó truy cập <code>&quot;away&quot;</code>, rồi mới truy cập <code>&quot;love&quot;</code>.</li>
	<li>Tương tự, nếu pattern là <code>[&quot;leetcode&quot;, &quot;love&quot;, &quot;leetcode&quot;]</code>, score là số người dùng <code>x</code> đã truy cập <code>&quot;leetcode&quot;</code>, sau đó truy cập <code>&quot;love&quot;</code>, rồi truy cập <code>&quot;leetcode&quot;</code> thêm <strong>một lần nữa</strong>.</li>
	<li>Ngoài ra, nếu pattern là <code>[&quot;luffy&quot;, &quot;luffy&quot;, &quot;luffy&quot;]</code>, score là số người dùng <code>x</code> đã truy cập <code>&quot;luffy&quot;</code> vào ba thời điểm khác nhau.</li>
</ul>

<p>Trả về <strong>pattern</strong> có <strong>score</strong> lớn nhất. Nếu có nhiều pattern cùng đạt score lớn nhất, trả về pattern nhỏ nhất theo thứ tự từ điển.</p>

<p>Lưu ý rằng các website trong pattern <strong>không cần</strong> được truy cập <em>liền kề nhau</em>; chỉ cần được truy cập theo thứ tự xuất hiện trong pattern.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> username = [&quot;joe&quot;,&quot;joe&quot;,&quot;joe&quot;,&quot;james&quot;,&quot;james&quot;,&quot;james&quot;,&quot;james&quot;,&quot;mary&quot;,&quot;mary&quot;,&quot;mary&quot;], timestamp = [1,2,3,4,5,6,7,8,9,10], website = [&quot;home&quot;,&quot;about&quot;,&quot;career&quot;,&quot;home&quot;,&quot;cart&quot;,&quot;maps&quot;,&quot;home&quot;,&quot;home&quot;,&quot;about&quot;,&quot;career&quot;]
<strong>Đầu ra:</strong> [&quot;home&quot;,&quot;about&quot;,&quot;career&quot;]
<strong>Giải thích:</strong> Các bộ ba trong ví dụ này là:
[&quot;joe&quot;,&quot;home&quot;,1],[&quot;joe&quot;,&quot;about&quot;,2],[&quot;joe&quot;,&quot;career&quot;,3],[&quot;james&quot;,&quot;home&quot;,4],[&quot;james&quot;,&quot;cart&quot;,5],[&quot;james&quot;,&quot;maps&quot;,6],[&quot;james&quot;,&quot;home&quot;,7],[&quot;mary&quot;,&quot;home&quot;,8],[&quot;mary&quot;,&quot;about&quot;,9], and [&quot;mary&quot;,&quot;career&quot;,10].
Pattern (&quot;home&quot;, &quot;about&quot;, &quot;career&quot;) có score 2 (joe và mary).
Pattern (&quot;home&quot;, &quot;cart&quot;, &quot;maps&quot;) có score 1 (james).
Pattern (&quot;home&quot;, &quot;cart&quot;, &quot;home&quot;) có score 1 (james).
Pattern (&quot;home&quot;, &quot;maps&quot;, &quot;home&quot;) có score 1 (james).
Pattern (&quot;cart&quot;, &quot;maps&quot;, &quot;home&quot;) có score 1 (james).
Pattern (&quot;home&quot;, &quot;home&quot;, &quot;home&quot;) có score 0 (không người dùng nào truy cập home 3 lần).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> username = [&quot;ua&quot;,&quot;ua&quot;,&quot;ua&quot;,&quot;ub&quot;,&quot;ub&quot;,&quot;ub&quot;], timestamp = [1,2,3,4,5,6], website = [&quot;a&quot;,&quot;b&quot;,&quot;a&quot;,&quot;a&quot;,&quot;b&quot;,&quot;c&quot;]
<strong>Đầu ra:</strong> [&quot;a&quot;,&quot;b&quot;,&quot;a&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= username.length &lt;= 50</code></li>
	<li><code>1 &lt;= username[i].length &lt;= 10</code></li>
	<li><code>timestamp.length == username.length</code></li>
	<li><code>1 &lt;= timestamp[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>website.length == username.length</code></li>
	<li><code>1 &lt;= website[i].length &lt;= 10</code></li>
	<li><code>username[i]</code> và <code>website[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Đảm bảo có ít nhất một người dùng đã truy cập ít nhất ba website.</li>
	<li>Tất cả bộ ba <code>[username[i], timestamp[i], website[i]]</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Pattern là bộ ba website được sắp theo thời gian; mỗi người dùng chỉ đóng góp một lần cho mỗi bộ ba khác nhau. Sắp xếp theo thời gian, nhóm website theo người dùng, liệt kê các bộ ba chỉ số tăng dần và lưu vào set, sau đó đếm số người dùng cho từng bộ ba rồi chọn bộ có tần suất cao nhất; nếu hòa thì chọn theo thứ tự từ điển.

<!-- thinking:end -->

Trước tiên, ta dùng hash table $d$ để lưu các website mỗi người dùng truy cập. Sau đó, duyệt $d$. Với từng người dùng, ta liệt kê mọi bộ ba website họ đã truy cập và đếm mỗi bộ ba phân biệt một lần. Cuối cùng, duyệt các bộ ba và trả về bộ có số lần xuất hiện cao nhất; nếu hòa, chọn bộ nhỏ nhất theo thứ tự từ điển.

Độ phức tạp thời gian là $O(n^3)$ và độ phức tạp không gian là $O(n^3)$, trong đó $n$ là độ dài của `username`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mostVisitedPattern(
        self, username: List[str], timestamp: List[int], website: List[str]
    ) -> List[str]:
        d = defaultdict(list)
        for user, _, site in sorted(
            zip(username, timestamp, website), key=lambda x: x[1]
        ):
            d[user].append(site)

        cnt = Counter()
        for sites in d.values():
            m = len(sites)
            s = set()
            if m > 2:
                for i in range(m - 2):
                    for j in range(i + 1, m - 1):
                        for k in range(j + 1, m):
                            s.add((sites[i], sites[j], sites[k]))
            for t in s:
                cnt[t] += 1
        return sorted(cnt.items(), key=lambda x: (-x[1], x[0]))[0][0]
```

#### Java

```java
class Solution {
    public List<String> mostVisitedPattern(String[] username, int[] timestamp, String[] website) {
        Map<String, List<Node>> d = new HashMap<>();
        int n = username.length;
        for (int i = 0; i < n; ++i) {
            String user = username[i];
            int ts = timestamp[i];
            String site = website[i];
            d.computeIfAbsent(user, k -> new ArrayList<>()).add(new Node(user, ts, site));
        }
        Map<String, Integer> cnt = new HashMap<>();
        for (var sites : d.values()) {
            int m = sites.size();
            Set<String> s = new HashSet<>();
            if (m > 2) {
                Collections.sort(sites, (a, b) -> a.ts - b.ts);
                for (int i = 0; i < m - 2; ++i) {
                    for (int j = i + 1; j < m - 1; ++j) {
                        for (int k = j + 1; k < m; ++k) {
                            s.add(sites.get(i).site + "," + sites.get(j).site + ","
                                + sites.get(k).site);
                        }
                    }
                }
            }
            for (String t : s) {
                cnt.put(t, cnt.getOrDefault(t, 0) + 1);
            }
        }
        int mx = 0;
        String t = "";
        for (var e : cnt.entrySet()) {
            if (mx < e.getValue() || (mx == e.getValue() && e.getKey().compareTo(t) < 0)) {
                mx = e.getValue();
                t = e.getKey();
            }
        }
        return Arrays.asList(t.split(","));
    }
}

class Node {
    String user;
    int ts;
    String site;

    Node(String user, int ts, String site) {
        this.user = user;
        this.ts = ts;
        this.site = site;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> mostVisitedPattern(vector<string>& username, vector<int>& timestamp, vector<string>& website) {
        unordered_map<string, vector<pair<int, string>>> d;
        int n = username.size();
        for (int i = 0; i < n; ++i) {
            auto user = username[i];
            int ts = timestamp[i];
            auto site = website[i];
            d[user].emplace_back(ts, site);
        }
        unordered_map<string, int> cnt;
        for (auto& [_, sites] : d) {
            int m = sites.size();
            unordered_set<string> s;
            if (m > 2) {
                sort(sites.begin(), sites.end());
                for (int i = 0; i < m - 2; ++i) {
                    for (int j = i + 1; j < m - 1; ++j) {
                        for (int k = j + 1; k < m; ++k) {
                            s.insert(sites[i].second + "," + sites[j].second + "," + sites[k].second);
                        }
                    }
                }
            }
            for (auto& t : s) {
                cnt[t]++;
            }
        }
        int mx = 0;
        string t;
        for (auto& [p, v] : cnt) {
            if (mx < v || (mx == v && t > p)) {
                mx = v;
                t = p;
            }
        }
        return split(t, ',');
    }

    vector<string> split(string& s, char c) {
        vector<string> res;
        stringstream ss(s);
        string t;
        while (getline(ss, t, c)) {
            res.push_back(t);
        }
        return res;
    }
};
```

#### Go

```go
func mostVisitedPattern(username []string, timestamp []int, website []string) []string {
	d := map[string][]pair{}
	for i, user := range username {
		ts := timestamp[i]
		site := website[i]
		d[user] = append(d[user], pair{ts, site})
	}
	cnt := map[string]int{}
	for _, sites := range d {
		m := len(sites)
		s := map[string]bool{}
		if m > 2 {
			sort.Slice(sites, func(i, j int) bool { return sites[i].ts < sites[j].ts })
			for i := 0; i < m-2; i++ {
				for j := i + 1; j < m-1; j++ {
					for k := j + 1; k < m; k++ {
						s[sites[i].site+","+sites[j].site+","+sites[k].site] = true
					}
				}
			}
		}
		for t := range s {
			cnt[t]++
		}
	}
	mx, t := 0, ""
	for p, v := range cnt {
		if mx < v || (mx == v && p < t) {
			mx = v
			t = p
		}
	}
	return strings.Split(t, ",")
}

type pair struct {
	ts   int
	site string
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
