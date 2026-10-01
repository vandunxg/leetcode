---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Graph
    - Array
    - String
    - Sorting
    - Eulerian Path
    - Eulerian Circuit
    - Heap (Priority Queue)
    - Semi-Eulerian Graph
---

<!-- problem:start -->

# [332. Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary)

[中文文档](/solution/0300-0399/0332.Reconstruct%20Itinerary/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho danh sách vé máy bay <code>tickets</code>, trong đó <code>tickets[i] = [from<sub>i</sub>, to<sub>i</sub>]</code> biểu diễn sân bay khởi hành và sân bay đến của một chuyến bay. Hãy khôi phục hành trình theo đúng thứ tự và trả về kết quả.</p>

<p>Tất cả vé thuộc về một người khởi hành từ <code>&quot;JFK&quot;</code>, vì vậy hành trình phải bắt đầu tại <code>&quot;JFK&quot;</code>. Nếu có nhiều hành trình hợp lệ, hãy trả về hành trình có thứ tự từ điển nhỏ nhất khi đọc như một chuỗi duy nhất.</p>

<ul>
	<li>Ví dụ, hành trình <code>[&quot;JFK&quot;, &quot;LGA&quot;]</code> có thứ tự từ điển nhỏ hơn <code>[&quot;JFK&quot;, &quot;LGB&quot;]</code>.</li>
</ul>

<p>Có thể giả sử các vé luôn tạo thành ít nhất một hành trình hợp lệ. Bạn phải sử dụng mỗi vé đúng một lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0332.Reconstruct%20Itinerary/images/itinerary1-graph.jpg" style="width: 382px; height: 222px;" />
<pre>
<strong>Đầu vào:</strong> tickets = [[&quot;MUC&quot;,&quot;LHR&quot;],[&quot;JFK&quot;,&quot;MUC&quot;],[&quot;SFO&quot;,&quot;SJC&quot;],[&quot;LHR&quot;,&quot;SFO&quot;]]
<strong>Đầu ra:</strong> [&quot;JFK&quot;,&quot;MUC&quot;,&quot;LHR&quot;,&quot;SFO&quot;,&quot;SJC&quot;]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0332.Reconstruct%20Itinerary/images/itinerary2-graph.jpg" style="width: 222px; height: 230px;" />
<pre>
<strong>Đầu vào:</strong> tickets = [[&quot;JFK&quot;,&quot;SFO&quot;],[&quot;JFK&quot;,&quot;ATL&quot;],[&quot;SFO&quot;,&quot;ATL&quot;],[&quot;ATL&quot;,&quot;JFK&quot;],[&quot;ATL&quot;,&quot;SFO&quot;]]
<strong>Đầu ra:</strong> [&quot;JFK&quot;,&quot;ATL&quot;,&quot;JFK&quot;,&quot;SFO&quot;,&quot;ATL&quot;,&quot;SFO&quot;]
<strong>Giải thích:</strong> Một cách khôi phục khác là [&quot;JFK&quot;,&quot;SFO&quot;,&quot;ATL&quot;,&quot;JFK&quot;,&quot;ATL&quot;,&quot;SFO&quot;], nhưng hành trình này có thứ tự từ điển lớn hơn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= tickets.length &lt;= 300</code></li>
	<li><code>tickets[i].length == 2</code></li>
	<li><code>from<sub>i</sub>.length == 3</code></li>
	<li><code>to<sub>i</sub>.length == 3</code></li>
	<li><code>from<sub>i</sub></code> và <code>to<sub>i</sub></code> chỉ gồm chữ cái tiếng Anh viết hoa.</li>
	<li><code>from<sub>i</sub> != to<sub>i</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đường đi Euler

<!-- thinking:start -->

> **Tư duy**
>
> Dùng mỗi vé đúng một lần và tạo hành trình có thứ tự từ điển nhỏ nhất: một đường đi Euler bắt đầu tại `JFK`.
>
> Lưu các điểm đến theo thứ tự ngược để lần pop cuối cùng lấy ra điểm tiếp theo nhỏ nhất. Thêm các node theo thứ tự hậu tự rồi đảo ngược thứ tự; các ngõ cụt sẽ được ghi nhận trước. Đề bài đảm bảo luôn tồn tại một hành trình.

<!-- thinking:end -->

Bài toán về cơ bản yêu cầu tìm một đường đi bắt đầu từ đỉnh cho trước, đi qua mỗi cạnh đúng một lần và có thứ tự từ điển nhỏ nhất trong số các đường đi như vậy, trên đồ thị có $n$ đỉnh và $m$ cạnh. Đây là bài toán đường đi Euler kinh điển.

Vì đề bài đảm bảo luôn tồn tại ít nhất một hành trình hợp lệ, ta có thể dùng trực tiếp thuật toán Hierholzer để tìm đường đi Euler bắt đầu từ điểm xuất phát.

Độ phức tạp thời gian là $O(m \times \log m)$, độ phức tạp không gian là $O(m)$, với $m$ là số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        def dfs(f: str):
            while g[f]:
                dfs(g[f].pop())
            ans.append(f)

        g = defaultdict(list)
        for f, t in sorted(tickets, reverse=True):
            g[f].append(t)
        ans = []
        dfs("JFK")
        return ans[::-1]
```

#### Java

```java
class Solution {
    private Map<String, List<String>> g = new HashMap<>();
    private List<String> ans = new ArrayList<>();

    public List<String> findItinerary(List<List<String>> tickets) {
        Collections.sort(tickets, (a, b) -> b.get(1).compareTo(a.get(1)));
        for (List<String> ticket : tickets) {
            g.computeIfAbsent(ticket.get(0), k -> new ArrayList<>()).add(ticket.get(1));
        }
        dfs("JFK");
        Collections.reverse(ans);
        return ans;
    }

    private void dfs(String f) {
        while (g.containsKey(f) && !g.get(f).isEmpty()) {
            String t = g.get(f).remove(g.get(f).size() - 1);
            dfs(t);
        }
        ans.add(f);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        sort(tickets.rbegin(), tickets.rend());
        unordered_map<string, vector<string>> g;
        for (const auto& ticket : tickets) {
            g[ticket[0]].push_back(ticket[1]);
        }
        vector<string> ans;
        auto dfs = [&](this auto&& dfs, string& f) -> void {
            while (!g[f].empty()) {
                string t = g[f].back();
                g[f].pop_back();
                dfs(t);
            }
            ans.emplace_back(f);
        };
        string f = "JFK";
        dfs(f);
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func findItinerary(tickets [][]string) (ans []string) {
	sort.Slice(tickets, func(i, j int) bool {
		return tickets[i][0] > tickets[j][0] || (tickets[i][0] == tickets[j][0] && tickets[i][1] > tickets[j][1])
	})
	g := make(map[string][]string)
	for _, ticket := range tickets {
		g[ticket[0]] = append(g[ticket[0]], ticket[1])
	}
	var dfs func(f string)
	dfs = func(f string) {
		for len(g[f]) > 0 {
			t := g[f][len(g[f])-1]
			g[f] = g[f][:len(g[f])-1]
			dfs(t)
		}
		ans = append(ans, f)
	}
	dfs("JFK")
	for i := 0; i < len(ans)/2; i++ {
		ans[i], ans[len(ans)-1-i] = ans[len(ans)-1-i], ans[i]
	}
	return
}
```

#### TypeScript

```ts
function findItinerary(tickets: string[][]): string[] {
    const g: Record<string, string[]> = {};
    tickets.sort((a, b) => b[1].localeCompare(a[1]));
    for (const [f, t] of tickets) {
        g[f] = g[f] || [];
        g[f].push(t);
    }
    const ans: string[] = [];
    const dfs = (f: string) => {
        while (g[f] && g[f].length) {
            const t = g[f].pop()!;
            dfs(t);
        }
        ans.push(f);
    };
    dfs('JFK');
    return ans.reverse();
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
