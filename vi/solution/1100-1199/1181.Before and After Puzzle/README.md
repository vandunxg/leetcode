---
comments: true
difficulty: Medium
rating: 1558
source: Biweekly Contest 8 Q2
tags:
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [1181. Before and After Puzzle 🔒](https://leetcode.com/problems/before-and-after-puzzle)

[中文文档](/solution/1100-1199/1181.Before%20and%20After%20Puzzle/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách <code>phrases</code>, hãy tạo danh sách các câu đố Before and After.</p>

<p><em>Phrase</em> là chuỗi chỉ gồm chữ cái tiếng Anh viết thường và dấu cách. Phrase không bắt đầu hoặc kết thúc bằng dấu cách, và không có hai dấu cách liên tiếp.</p>

<p><em>Câu đố Before and After</em> là phrase được tạo bằng cách ghép hai phrase sao cho <strong>từ cuối của phrase thứ nhất</strong> trùng với <strong>từ đầu của phrase thứ hai</strong>. Lưu ý rằng chỉ từ cuối của phrase thứ nhất và từ đầu của phrase thứ hai được gộp làm một.</p>

<p>Hãy trả về các câu đố Before and After có thể tạo từ mọi cặp phrase <code>phrases[i]</code> và <code>phrases[j]</code> với <code>i != j</code>. Thứ tự ghép hai phrase có ý nghĩa, vì vậy cần xét cả hai thứ tự.</p>

<p>Hãy trả về danh sách các chuỗi <strong>không trùng lặp</strong>, được <strong>sắp xếp theo thứ tự từ điển</strong> sau khi loại bỏ mọi phrase trùng trong các câu đố Before and After được tạo ra.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">phrases = [&quot;writing code&quot;,&quot;code rocks&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;writing code rocks&quot;]</span></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">phrases = [&quot;mission statement&quot;,&quot;a quick bite to eat&quot;,&quot;a chip off the old block&quot;,&quot;chocolate bar&quot;,&quot;mission impossible&quot;,&quot;a man on a mission&quot;,&quot;block party&quot;,&quot;eat my words&quot;,&quot;bar of soap&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;a chip off the old block party&quot;,&quot;a man on a mission impossible&quot;,&quot;a man on a mission statement&quot;,&quot;a quick bite to eat my words&quot;,&quot;chocolate bar of soap&quot;]</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">phrases = [&quot;a&quot;,&quot;b&quot;,&quot;a&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;a&quot;]</span></p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">phrases = [&quot;ab ba&quot;,&quot;ba ab&quot;,&quot;ab ba&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;ab ba ab&quot;,&quot;ba ab ba&quot;]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= phrases.length &lt;= 100</code></li>
	<li><code>1 &lt;= phrases[i].length &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Hai phrase khác nhau ghép được khi và chỉ khi từ cuối của phrase thứ nhất trùng với từ đầu của phrase thứ hai; từ trùng đó chỉ xuất hiện một lần trong kết quả. Vì $n$ nhỏ, ta lưu từ đầu và từ cuối, thử các cặp $(i,j)$, thêm kết quả ghép hợp lệ vào set rồi sắp xếp.

<!-- thinking:end -->

Trước tiên, duyệt danh sách `phrases` và lưu từ đầu, từ cuối của mỗi phrase vào mảng $ps$, trong đó $ps[i][0]$ và $ps[i][1]$ lần lượt là từ đầu và từ cuối của phrase thứ $i$.

Tiếp theo, xét tất cả cặp $(i, j)$ với $i, j \in [0, n)$ và $i \neq j$. Nếu $ps[i][1] = ps[j][0]$, ta ghép phrase thứ $i$ và phrase thứ $j$ để tạo phrase mới $phrases[i] + phrases[j][len(ps[j][0]):]$, rồi thêm phrase đó vào hash table $s$.

Cuối cùng, chuyển hash table $s$ thành mảng rồi sắp xếp để thu được đáp án.

Độ phức tạp thời gian là $O(n^2 \times m \times (\log n + \log m))$ và độ phức tạp không gian là $O(n^2 \times m)$. Trong đó, $n$ là số phần tử của mảng `phrases`, còn $m$ là độ dài trung bình của mỗi phrase.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def beforeAndAfterPuzzles(self, phrases: List[str]) -> List[str]:
        ps = []
        for p in phrases:
            ws = p.split()
            ps.append((ws[0], ws[-1]))
        n = len(ps)
        ans = []
        for i in range(n):
            for j in range(n):
                if i != j and ps[i][1] == ps[j][0]:
                    ans.append(phrases[i] + phrases[j][len(ps[j][0]) :])
        return sorted(set(ans))
```

#### Java

```java
class Solution {
    public List<String> beforeAndAfterPuzzles(String[] phrases) {
        int n = phrases.length;
        var ps = new String[n][];
        for (int i = 0; i < n; ++i) {
            var ws = phrases[i].split(" ");
            ps[i] = new String[] {ws[0], ws[ws.length - 1]};
        }
        Set<String> s = new HashSet<>();
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i != j && ps[i][1].equals(ps[j][0])) {
                    s.add(phrases[i] + phrases[j].substring(ps[j][0].length()));
                }
            }
        }
        var ans = new ArrayList<>(s);
        Collections.sort(ans);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> beforeAndAfterPuzzles(vector<string>& phrases) {
        int n = phrases.size();
        pair<string, string> ps[n];
        for (int i = 0; i < n; ++i) {
            int j = phrases[i].find(' ');
            if (j == string::npos) {
                ps[i] = {phrases[i], phrases[i]};
            } else {
                int k = phrases[i].rfind(' ');
                ps[i] = {phrases[i].substr(0, j), phrases[i].substr(k + 1)};
            }
        }
        unordered_set<string> s;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i != j && ps[i].second == ps[j].first) {
                    s.insert(phrases[i] + phrases[j].substr(ps[i].second.size()));
                }
            }
        }
        vector<string> ans(s.begin(), s.end());
        sort(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func beforeAndAfterPuzzles(phrases []string) []string {
	n := len(phrases)
	ps := make([][2]string, n)
	for i, p := range phrases {
		ws := strings.Split(p, " ")
		ps[i] = [2]string{ws[0], ws[len(ws)-1]}
	}
	s := map[string]bool{}
	for i := 0; i < n; i++ {
		for j := 0; j < n; j++ {
			if i != j && ps[i][1] == ps[j][0] {
				s[phrases[i]+phrases[j][len(ps[j][0]):]] = true
			}
		}
	}
	ans := make([]string, 0, len(s))
	for k := range s {
		ans = append(ans, k)
	}
	sort.Strings(ans)
	return ans
}
```

#### TypeScript

```ts
function beforeAndAfterPuzzles(phrases: string[]): string[] {
    const ps: string[][] = [];
    for (const p of phrases) {
        const ws = p.split(' ');
        ps.push([ws[0], ws[ws.length - 1]]);
    }
    const n = ps.length;
    const s: Set<string> = new Set();
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            if (i !== j && ps[i][1] === ps[j][0]) {
                s.add(`${phrases[i]}${phrases[j].substring(ps[j][0].length)}`);
            }
        }
    }
    return [...s].sort();
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
