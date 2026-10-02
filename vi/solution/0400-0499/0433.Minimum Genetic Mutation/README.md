---
comments: true
difficulty: Medium
tags:
    - Breadth-First Search
    - Hash Table
    - String
    - Bidirectional Search
---

<!-- problem:start -->

# [433. Minimum Genetic Mutation](https://leetcode.com/problems/minimum-genetic-mutation)

[中文文档](/solution/0400-0499/0433.Minimum%20Genetic%20Mutation/README.md)

## Mô tả

<!-- description:start -->

<p>Chuỗi gene có thể được biểu diễn bằng chuỗi dài 8 ký tự, gồm các ký tự <code>&#39;A&#39;</code>, <code>&#39;C&#39;</code>, <code>&#39;G&#39;</code> và <code>&#39;T&#39;</code>.</p>

<p>Giả sử ta cần tìm cách biến đổi gene từ chuỗi <code>startGene</code> thành chuỗi <code>endGene</code>. Mỗi mutation được định nghĩa là thay đổi đúng một ký tự trong chuỗi gene.</p>

<ul>
	<li>Ví dụ, <code>&quot;AACCGGTT&quot; --&gt; &quot;AACCGGTA&quot;</code> là một mutation.</li>
</ul>

<p>Ngoài ra còn có gene bank <code>bank</code> lưu tất cả các đột biến gene hợp lệ. Một chuỗi gene phải có trong <code>bank</code> thì mới được xem là hợp lệ.</p>

<p>Cho hai chuỗi gene <code>startGene</code> và <code>endGene</code> cùng gene bank <code>bank</code>. Hãy trả về <em>số mutation ít nhất cần thực hiện để biến </em><code>startGene</code><em> thành </em><code>endGene</code>. Nếu không thể biến đổi, trả về <code>-1</code>.</p>

<p>Lưu ý, chuỗi bắt đầu được giả định là hợp lệ nên có thể không xuất hiện trong bank.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> startGene = &quot;AACCGGTT&quot;, endGene = &quot;AACCGGTA&quot;, bank = [&quot;AACCGGTA&quot;]
<strong>Đầu ra:</strong> 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> startGene = &quot;AACCGGTT&quot;, endGene = &quot;AAACGGTA&quot;, bank = [&quot;AACCGGTA&quot;,&quot;AACCGCTA&quot;,&quot;AAACGGTA&quot;]
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= bank.length &lt;= 10</code></li>
	<li><code>startGene.length == endGene.length == bank[i].length == 8</code></li>
	<li><code>startGene</code>, <code>endGene</code> và <code>bank[i]</code> chỉ gồm các ký tự <code>[&#39;A&#39;, &#39;C&#39;, &#39;G&#39;, &#39;T&#39;]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi mutation thay đổi một ký tự và phải đưa tới một chuỗi có trong bank; mục tiêu là tìm số bước ít nhất. Đây là bài toán tìm đường đi ngắn nhất trên đồ thị không trọng số, nên depth-first search không đảm bảo tìm được số bước nhỏ nhất.
>
> Chạy BFS từ chuỗi bắt đầu. Ở mỗi bước, xét các chuỗi trong bank chưa dùng và chỉ khác đúng một vị trí. Lần đầu dequeue chuỗi gene đích chính là đáp án; nếu queue rỗng thì không thể tới đích.
>
> Bank có kích thước nhỏ, nên chỉ cần kiểm tra khoảng cách Hamming từng cặp có bằng $1$ hay không. Set visited giúp tránh enqueue cùng một chuỗi gene nhiều lần.

<!-- thinking:end -->

Dùng queue `q` để lưu chuỗi gene hiện tại cùng số lần thay đổi, và set `vis` để lưu các chuỗi gene đã thăm. Ban đầu, thêm chuỗi gene bắt đầu `start` vào queue `q` và set `vis`.

Sau đó, liên tục lấy một chuỗi gene khỏi queue `q`. Nếu chuỗi này bằng chuỗi gene đích thì trả về số lần thay đổi hiện tại. Nếu không, duyệt gene bank `bank` và tính số vị trí khác nhau giữa chuỗi hiện tại với từng chuỗi trong bank. Nếu số vị trí khác nhau bằng $1$ và chuỗi đó chưa được thăm, thêm nó vào queue `q` và set `vis`.

Nếu queue `q` rỗng, nghĩa là không thể hoàn tất việc biến đổi gene, khi đó trả về $-1$.

Độ phức tạp thời gian là $O(C \times n \times m)$ và độ phức tạp không gian là $O(n \times m)$, trong đó $n$ là độ dài chuỗi gene, $m$ là số chuỗi trong gene bank, còn $C$ là kích thước bảng ký tự của chuỗi gene. Trong bài này, $C = 4$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMutation(self, startGene: str, endGene: str, bank: List[str]) -> int:
        q = deque([(startGene, 0)])
        vis = {startGene}
        while q:
            gene, depth = q.popleft()
            if gene == endGene:
                return depth
            for nxt in bank:
                diff = sum(a != b for a, b in zip(gene, nxt))
                if diff == 1 and nxt not in vis:
                    q.append((nxt, depth + 1))
                    vis.add(nxt)
        return -1
```

#### Java

```java
class Solution {
    public int minMutation(String startGene, String endGene, String[] bank) {
        Deque<String> q = new ArrayDeque<>();
        q.offer(startGene);
        Set<String> vis = new HashSet<>();
        vis.add(startGene);
        int depth = 0;
        while (!q.isEmpty()) {
            for (int m = q.size(); m > 0; --m) {
                String gene = q.poll();
                if (gene.equals(endGene)) {
                    return depth;
                }
                for (String next : bank) {
                    int c = 2;
                    for (int k = 0; k < 8 && c > 0; ++k) {
                        if (gene.charAt(k) != next.charAt(k)) {
                            --c;
                        }
                    }
                    if (c > 0 && !vis.contains(next)) {
                        q.offer(next);
                        vis.add(next);
                    }
                }
            }
            ++depth;
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMutation(string startGene, string endGene, vector<string>& bank) {
        queue<pair<string, int>> q{{{startGene, 0}}};
        unordered_set<string> vis = {startGene};
        while (!q.empty()) {
            auto [gene, depth] = q.front();
            q.pop();
            if (gene == endGene) {
                return depth;
            }
            for (const string& next : bank) {
                int c = 2;
                for (int k = 0; k < 8 && c; ++k) {
                    c -= gene[k] != next[k];
                }
                if (c && !vis.contains(next)) {
                    vis.insert(next);
                    q.push({next, depth + 1});
                }
            }
        }
        return -1;
    }
};
```

#### Go

```go
func minMutation(startGene string, endGene string, bank []string) int {
	type pair struct {
		s     string
		depth int
	}
	q := []pair{pair{startGene, 0}}
	vis := map[string]bool{startGene: true}
	for len(q) > 0 {
		p := q[0]
		q = q[1:]
		if p.s == endGene {
			return p.depth
		}
		for _, next := range bank {
			diff := 0
			for i := 0; i < len(startGene); i++ {
				if p.s[i] != next[i] {
					diff++
				}
			}
			if diff == 1 && !vis[next] {
				vis[next] = true
				q = append(q, pair{next, p.depth + 1})
			}
		}
	}
	return -1
}
```

#### TypeScript

```ts
function minMutation(startGene: string, endGene: string, bank: string[]): number {
    const q: [string, number][] = [[startGene, 0]];
    const vis = new Set<string>([startGene]);
    for (const [gene, depth] of q) {
        if (gene === endGene) {
            return depth;
        }
        for (const next of bank) {
            let c = 2;
            for (let k = 0; k < 8 && c > 0; ++k) {
                if (gene[k] !== next[k]) {
                    --c;
                }
            }
            if (c && !vis.has(next)) {
                q.push([next, depth + 1]);
                vis.add(next);
            }
        }
    }
    return -1;
}
```

#### Rust

```rust
use std::collections::{HashSet, VecDeque};

impl Solution {
    pub fn min_mutation(start_gene: String, end_gene: String, bank: Vec<String>) -> i32 {
        let mut q = VecDeque::new();
        q.push_back((start_gene.clone(), 0));
        let mut vis = HashSet::new();
        vis.insert(start_gene);

        while let Some((gene, depth)) = q.pop_front() {
            if gene == end_gene {
                return depth;
            }
            for next in &bank {
                let mut c = 2;
                for k in 0..8 {
                    if gene.as_bytes()[k] != next.as_bytes()[k] {
                        c -= 1;
                    }
                    if c == 0 {
                        break;
                    }
                }
                if c > 0 && !vis.contains(next) {
                    vis.insert(next.clone());
                    q.push_back((next.clone(), depth + 1));
                }
            }
        }
        -1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
