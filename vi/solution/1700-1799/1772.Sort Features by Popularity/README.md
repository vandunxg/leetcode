---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [1772. Sort Features by Popularity 🔒](https://leetcode.com/problems/sort-features-by-popularity)

[中文文档](/solution/1700-1799/1772.Sort%20Features%20by%20Popularity/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>features</code>, trong đó <code>features[i]</code> là một từ biểu diễn tên tính năng của sản phẩm mới nhất bạn đang phát triển. Bạn đã thực hiện khảo sát để người dùng cho biết những tính năng họ thích. Cho mảng chuỗi <code>responses</code>, trong đó mỗi <code>responses[i]</code> là chuỗi gồm các từ cách nhau bởi dấu cách.</p>

<p><strong>Độ phổ biến</strong> của một tính năng là số <code>responses[i]</code> chứa tính năng đó. Hãy sắp xếp các tính năng theo độ phổ biến không tăng. Nếu hai tính năng có cùng độ phổ biến, sắp xếp theo chỉ số ban đầu trong <code>features</code>. Một phản hồi có thể chứa cùng một tính năng nhiều lần, nhưng tính năng đó chỉ được tính một lần trong độ phổ biến.</p>

<p>Trả về <em>các tính năng theo thứ tự đã sắp xếp.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> features = [&quot;cooler&quot;,&quot;lock&quot;,&quot;touch&quot;], responses = [&quot;i like cooler cooler&quot;,&quot;lock touch cool&quot;,&quot;locker like touch&quot;]
<strong>Output:</strong> [&quot;touch&quot;,&quot;cooler&quot;,&quot;lock&quot;]
<strong>Explanation:</strong> appearances(&quot;cooler&quot;) = 1, appearances(&quot;lock&quot;) = 1, appearances(&quot;touch&quot;) = 2. Since &quot;cooler&quot; and &quot;lock&quot; both had 1 appearance, &quot;cooler&quot; comes first because &quot;cooler&quot; came first in the features array.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> features = [&quot;a&quot;,&quot;aa&quot;,&quot;b&quot;,&quot;c&quot;], responses = [&quot;a&quot;,&quot;a aa&quot;,&quot;a a a a a&quot;,&quot;b a&quot;]
<strong>Output:</strong> [&quot;a&quot;,&quot;aa&quot;,&quot;b&quot;,&quot;c&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= features.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= features[i].length &lt;= 10</code></li>
	<li><code>features</code> contains no duplicates.</li>
	<li><code>features[i]</code> consists of lowercase letters.</li>
	<li><code>1 &lt;= responses.length &lt;= 10<sup>2</sup></code></li>
	<li><code>1 &lt;= responses[i].length &lt;= 10<sup>3</sup></code></li>
<li><code>responses[i]</code> gồm các chữ cái viết thường và dấu cách.</li>
	<li><code>responses[i]</code> contains no two consecutive spaces.</li>
	<li><code>responses[i]</code> has no leading or trailing spaces.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table + sắp xếp tùy chỉnh

<!-- thinking:start -->

> **Tư duy**
>
> Độ phổ biến là số phản hồi nhắc đến một tính năng (mỗi phản hồi chỉ tính một lần). Sắp xếp ổn định $\textit{features}$ theo số lần này giảm dần.
>
> Dùng set để loại từ trùng trong từng phản hồi, tăng bộ đếm, rồi sắp xếp theo $-cnt[w]$ để các trường hợp bằng nhau giữ nguyên thứ tự ban đầu.

<!-- thinking:end -->

Ta duyệt `responses`, tạm lưu từng từ trong `responses[i]` vào hash table `vis`. Sau đó, ghi các từ trong `vis` vào hash table `cnt` và đếm số phản hồi xuất hiện mỗi từ.

Tiếp theo, dùng cách sắp xếp tùy chỉnh để sắp xếp các từ trong `features` theo số lần xuất hiện giảm dần. Nếu số lần xuất hiện bằng nhau, sắp xếp theo chỉ số xuất hiện tăng dần.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là độ dài của `features`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortFeatures(self, features: List[str], responses: List[str]) -> List[str]:
        cnt = Counter()
        for s in responses:
            for w in set(s.split()):
                cnt[w] += 1
        return sorted(features, key=lambda w: -cnt[w])
```

#### Java

```java
class Solution {
    public String[] sortFeatures(String[] features, String[] responses) {
        Map<String, Integer> cnt = new HashMap<>();
        for (String s : responses) {
            Set<String> vis = new HashSet<>();
            for (String w : s.split(" ")) {
                if (vis.add(w)) {
                    cnt.merge(w, 1, Integer::sum);
                }
            }
        }
        int n = features.length;
        Integer[] idx = new Integer[n];
        for (int i = 0; i < n; i++) {
            idx[i] = i;
        }
        Arrays.sort(idx, (i, j) -> {
            int x = cnt.getOrDefault(features[i], 0);
            int y = cnt.getOrDefault(features[j], 0);
            return x == y ? i - j : y - x;
        });
        String[] ans = new String[n];
        for (int i = 0; i < n; i++) {
            ans[i] = features[idx[i]];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> sortFeatures(vector<string>& features, vector<string>& responses) {
        unordered_map<string, int> cnt;
        for (auto& s : responses) {
            istringstream iss(s);
            string w;
            unordered_set<string> st;
            while (iss >> w) {
                st.insert(w);
            }
            for (auto& w : st) {
                ++cnt[w];
            }
        }
        int n = features.size();
        vector<int> idx(n);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) {
            int x = cnt[features[i]], y = cnt[features[j]];
            return x == y ? i < j : x > y;
        });
        vector<string> ans(n);
        for (int i = 0; i < n; ++i) {
            ans[i] = features[idx[i]];
        }
        return ans;
    }
};
```

#### Go

```go
func sortFeatures(features []string, responses []string) []string {
	cnt := map[string]int{}
	for _, s := range responses {
		vis := map[string]bool{}
		for _, w := range strings.Split(s, " ") {
			if !vis[w] {
				cnt[w]++
				vis[w] = true
			}
		}
	}
	sort.SliceStable(features, func(i, j int) bool { return cnt[features[i]] > cnt[features[j]] })
	return features
}
```

#### TypeScript

```ts
function sortFeatures(features: string[], responses: string[]): string[] {
    const cnt: Map<string, number> = new Map();
    for (const s of responses) {
        const vis: Set<string> = new Set();
        for (const w of s.split(' ')) {
            if (vis.has(w)) {
                continue;
            }
            vis.add(w);
            cnt.set(w, (cnt.get(w) || 0) + 1);
        }
    }
    const n = features.length;
    const idx: number[] = Array.from({ length: n }, (_, i) => i);
    idx.sort((i, j) => {
        const x = cnt.get(features[i]) || 0;
        const y = cnt.get(features[j]) || 0;
        return x === y ? i - j : y - x;
    });
    return idx.map(i => features[i]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
