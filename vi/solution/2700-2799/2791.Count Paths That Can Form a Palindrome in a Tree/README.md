---
comments: true
difficulty: Hard
rating: 2677
source: Weekly Contest 355 Q4
tags:
    - Bit Manipulation
    - Tree
    - Depth-First Search
    - Hash Table
---

<!-- problem:start -->

# [2791. Count Paths That Can Form a Palindrome in a Tree](https://leetcode.com/problems/count-paths-that-can-form-a-palindrome-in-a-tree)

[中文文档](/solution/2700-2799/2791.Count%20Paths%20That%20Can%20Form%20a%20Palindrome%20in%20a%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một <strong>cây</strong> (tức là một đồ thị liên thông, vô hướng và không có chu trình) có <strong>gốc</strong> tại nút <code>0</code>, gồm <code>n</code> nút được đánh số từ <code>0</code> đến <code>n - 1</code>. Cây được biểu diễn bằng mảng <strong>0-indexed</strong> <code>parent</code> có kích thước <code>n</code>, trong đó <code>parent[i]</code> là nút cha của nút <code>i</code>. Vì nút <code>0</code> là gốc nên <code>parent[0] == -1</code>.</p>

<p>Bạn cũng được cho một chuỗi <code>s</code> có độ dài <code>n</code>, trong đó <code>s[i]</code> là ký tự gán cho cạnh giữa <code>i</code> và <code>parent[i]</code>. Có thể bỏ qua <code>s[0]</code>.</p>

<p>Trả về <em>số cặp nút </em><code>(u, v)</code><em> sao cho </em><code>u &lt; v</code><em> và các ký tự gán cho những cạnh trên đường đi từ </em><code>u</code><em> đến </em><code>v</code><em> có thể được <strong>sắp xếp lại</strong> để tạo thành một <strong>palindrome</strong></em>.</p>

<p>Một chuỗi là <strong>palindrome</strong> khi đọc từ phải sang trái cũng giống như đọc từ trái sang phải.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2791.Count%20Paths%20That%20Can%20Form%20a%20Palindrome%20in%20a%20Tree/images/treedrawio-8drawio.png" style="width: 281px; height: 181px;" /></p>

<pre>
<strong>Đầu vào:</strong> parent = [-1,0,0,1,1,2], s = &quot;acaabc&quot;
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Các cặp hợp lệ là:
- Tất cả các cặp (0,1), (0,2), (1,3), (1,4) và (2,5) đều tạo thành một ký tự, luôn là palindrome.
- Cặp (2,3) tạo thành chuỗi &quot;aca&quot;, là một palindrome.
- Cặp (1,5) tạo thành chuỗi &quot;cac&quot;, là một palindrome.
- Cặp (3,5) tạo thành chuỗi &quot;acac&quot;, có thể được sắp xếp lại thành palindrome &quot;acca&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> parent = [-1,0,0,0,0], s = &quot;aaaaa&quot;
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Mọi cặp nút (u,v) với u &lt; v đều hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == parent.length == s.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= parent[i] &lt;= n - 1</code> với mọi <code>i &gt;= 1</code></li>
	<li><code>parent[0] == -1</code></li>
	<li><code>parent</code> biểu diễn một cây hợp lệ.</li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hãy đếm các đường đi đơn có các ký tự có thể được sắp xếp lại thành một palindrome. Có số đường đi tăng theo bậc hai, trong khi các nhãn là các chữ cái, nên không thể liệt kê từng đường đi.
>
> Dùng XOR để đóng gói tính chẵn lẻ của các chữ cái từ gốc đến một nút vào một mask $20$-bit; đường đi là XOR của hai đầu mút. Một hoán vị palindrome cho phép nhiều nhất một bit được bật. Trong DFS, một bộ đếm tra cứu mask hiện tại và hai mươi mask khác chỉ khác một bit, sau đó ghi nhận mask hiện tại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPalindromePaths(self, parent: List[int], s: str) -> int:
        def dfs(i: int, xor: int):
            nonlocal ans
            for j, v in g[i]:
                x = xor ^ v
                ans += cnt[x]
                for k in range(26):
                    ans += cnt[x ^ (1 << k)]
                cnt[x] += 1
                dfs(j, x)

        n = len(parent)
        g = defaultdict(list)
        for i in range(1, n):
            p = parent[i]
            g[p].append((i, 1 << (ord(s[i]) - ord('a'))))
        ans = 0
        cnt = Counter({0: 1})
        dfs(0, 0)
        return ans
```

#### Java

```java
class Solution {
    private List<int[]>[] g;
    private Map<Integer, Integer> cnt = new HashMap<>();
    private long ans;

    public long countPalindromePaths(List<Integer> parent, String s) {
        int n = parent.size();
        g = new List[n];
        cnt.put(0, 1);
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int i = 1; i < n; ++i) {
            int p = parent.get(i);
            g[p].add(new int[] {i, 1 << (s.charAt(i) - 'a')});
        }
        dfs(0, 0);
        return ans;
    }

    private void dfs(int i, int xor) {
        for (int[] e : g[i]) {
            int j = e[0], v = e[1];
            int x = xor ^ v;
            ans += cnt.getOrDefault(x, 0);
            for (int k = 0; k < 26; ++k) {
                ans += cnt.getOrDefault(x ^ (1 << k), 0);
            }
            cnt.merge(x, 1, Integer::sum);
            dfs(j, x);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countPalindromePaths(vector<int>& parent, string s) {
        int n = parent.size();
        vector<vector<pair<int, int>>> g(n);
        unordered_map<int, int> cnt;
        cnt[0] = 1;
        for (int i = 1; i < n; ++i) {
            int p = parent[i];
            g[p].emplace_back(i, 1 << (s[i] - 'a'));
        }
        long long ans = 0;
        function<void(int, int)> dfs = [&](int i, int xo) {
            for (auto [j, v] : g[i]) {
                int x = xo ^ v;
                ans += cnt[x];
                for (int k = 0; k < 26; ++k) {
                    ans += cnt[x ^ (1 << k)];
                }
                ++cnt[x];
                dfs(j, x);
            }
        };
        dfs(0, 0);
        return ans;
    }
};
```

#### Go

```go
func countPalindromePaths(parent []int, s string) (ans int64) {
	type pair struct{ i, v int }
	n := len(parent)
	g := make([][]pair, n)
	for i := 1; i < n; i++ {
		p := parent[i]
		g[p] = append(g[p], pair{i, 1 << (s[i] - 'a')})
	}
	cnt := map[int]int{0: 1}
	var dfs func(i, xor int)
	dfs = func(i, xor int) {
		for _, e := range g[i] {
			x := xor ^ e.v
			ans += int64(cnt[x])
			for k := 0; k < 26; k++ {
				ans += int64(cnt[x^(1<<k)])
			}
			cnt[x]++
			dfs(e.i, x)
		}
	}
	dfs(0, 0)
	return
}
```

#### TypeScript

```ts
function countPalindromePaths(parent: number[], s: string): number {
    const n = parent.length;
    const g: [number, number][][] = Array.from({ length: n }, () => []);
    for (let i = 1; i < n; ++i) {
        g[parent[i]].push([i, 1 << (s.charCodeAt(i) - 97)]);
    }
    const cnt: Map<number, number> = new Map();
    cnt.set(0, 1);
    let ans = 0;
    const dfs = (i: number, xor: number): void => {
        for (const [j, v] of g[i]) {
            const x = xor ^ v;
            ans += cnt.get(x) || 0;
            for (let k = 0; k < 26; ++k) {
                ans += cnt.get(x ^ (1 << k)) || 0;
            }
            cnt.set(x, (cnt.get(x) || 0) + 1);
            dfs(j, x);
        }
    };
    dfs(0, 0);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
