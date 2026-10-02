---
comments: true
difficulty: Hard
tags:
    - Breadth-First Search
    - Hash Table
    - String
---

<!-- problem:start -->

# [854. K-Similar Strings](https://leetcode.com/problems/k-similar-strings)

[中文文档](/solution/0800-0899/0854.K-Similar%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Hai chuỗi <code>s1</code> và <code>s2</code> được gọi là <code>k</code><strong>-tương tự</strong> (với một số nguyên không âm <code>k</code>) nếu có thể đổi chỗ hai ký tự trong <code>s1</code> đúng <code>k</code> lần để thu được chuỗi bằng <code>s2</code>.</p>

<p>Cho hai chuỗi hoán vị của nhau <code>s1</code> và <code>s2</code>, hãy trả về giá trị <code>k</code> nhỏ nhất sao cho chúng <code>k</code><strong>-tương tự</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;ab&quot;, s2 = &quot;ba&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Hai chuỗi 1-tương tự vì chỉ cần đổi chỗ một lần để biến s1 thành s2: &quot;ab&quot; --&gt; &quot;ba&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;abc&quot;, s2 = &quot;bca&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hai chuỗi 2-tương tự vì có thể đổi chỗ hai lần để biến s1 thành s2: &quot;abc&quot; --&gt; &quot;bac&quot; --&gt; &quot;bca&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s1.length &lt;= 20</code></li>
	<li><code>s2.length == s1.length</code></li>
	<li><code>s1</code> và <code>s2</code> chỉ chứa các chữ cái viết thường thuộc tập <code>{&#39;a&#39;, &#39;b&#39;, &#39;c&#39;, &#39;d&#39;, &#39;e&#39;, &#39;f&#39;}</code>.</li>
	<li><code>s2</code> là một hoán vị của <code>s1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta đổi chỗ hai ký tự trong $s1$ cho đến khi chuỗi bằng $s2$. Hai chuỗi là hoán vị của nhau và có độ dài $\le 20$, nên có thể dùng BFS trên toàn bộ chuỗi.
>
> Với mỗi trạng thái, chỉ đổi ký tự sai khác đầu tiên với một ký tự phía sau cần nằm ở vị trí đó. Bỏ qua các phép đổi không cần thiết; số layer BFS chính là số lần đổi chỗ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kSimilarity(self, s1: str, s2: str) -> int:
        def next(s):
            i = 0
            while s[i] == s2[i]:
                i += 1
            res = []
            for j in range(i + 1, n):
                if s[j] == s2[i] and s[j] != s2[j]:
                    res.append(s2[: i + 1] + s[i + 1 : j] + s[i] + s[j + 1 :])
            return res

        q = deque([s1])
        vis = {s1}
        ans, n = 0, len(s1)
        while 1:
            for _ in range(len(q)):
                s = q.popleft()
                if s == s2:
                    return ans
                for nxt in next(s):
                    if nxt not in vis:
                        vis.add(nxt)
                        q.append(nxt)
            ans += 1
```

#### Java

```java
class Solution {
    public int kSimilarity(String s1, String s2) {
        Deque<String> q = new ArrayDeque<>();
        Set<String> vis = new HashSet<>();
        q.offer(s1);
        vis.add(s1);
        int ans = 0;
        while (true) {
            for (int i = q.size(); i > 0; --i) {
                String s = q.pollFirst();
                if (s.equals(s2)) {
                    return ans;
                }
                for (String nxt : next(s, s2)) {
                    if (!vis.contains(nxt)) {
                        vis.add(nxt);
                        q.offer(nxt);
                    }
                }
            }
            ++ans;
        }
    }

    private List<String> next(String s, String s2) {
        int i = 0, n = s.length();
        char[] cs = s.toCharArray();
        for (; cs[i] == s2.charAt(i); ++i) {
        }

        List<String> res = new ArrayList<>();
        for (int j = i + 1; j < n; ++j) {
            if (cs[j] == s2.charAt(i) && cs[j] != s2.charAt(j)) {
                swap(cs, i, j);
                res.add(new String(cs));
                swap(cs, i, j);
            }
        }
        return res;
    }

    private void swap(char[] cs, int i, int j) {
        char t = cs[i];
        cs[i] = cs[j];
        cs[j] = t;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int kSimilarity(string s1, string s2) {
        queue<string> q{{s1}};
        unordered_set<string> vis{{s1}};
        int ans = 0;
        while (1) {
            for (int i = q.size(); i; --i) {
                auto s = q.front();
                q.pop();
                if (s == s2) {
                    return ans;
                }
                for (auto& nxt : next(s, s2)) {
                    if (!vis.count(nxt)) {
                        vis.insert(nxt);
                        q.push(nxt);
                    }
                }
            }
            ++ans;
        }
    }

    vector<string> next(string& s, string& s2) {
        int i = 0, n = s.size();
        for (; s[i] == s2[i]; ++i) {}
        vector<string> res;
        for (int j = i + 1; j < n; ++j) {
            if (s[j] == s2[i] && s[j] != s2[j]) {
                swap(s[i], s[j]);
                res.push_back(s);
                swap(s[i], s[j]);
            }
        }
        return res;
    }
};
```

#### Go

```go
func kSimilarity(s1 string, s2 string) int {
	next := func(s string) []string {
		i := 0
		res := []string{}
		for ; s[i] == s2[i]; i++ {
		}
		for j := i + 1; j < len(s1); j++ {
			if s[j] == s2[i] && s[j] != s2[j] {
				res = append(res, s[:i]+string(s[j])+s[i+1:j]+string(s[i])+s[j+1:])
			}
		}
		return res
	}

	q := []string{s1}
	vis := map[string]bool{s1: true}
	ans := 0
	for {
		for i := len(q); i > 0; i-- {
			s := q[0]
			q = q[1:]
			if s == s2 {
				return ans
			}
			for _, nxt := range next(s) {
				if !vis[nxt] {
					vis[nxt] = true
					q = append(q, nxt)
				}
			}
		}
		ans++
	}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> BFS thông thường mở rộng theo từng layer. Một nửa số vị trí sai khác là heuristic admissible vì mỗi lần đổi chỗ sửa được tối đa hai vị trí. Ưu tiên lấy trạng thái có $dist+h$ nhỏ nhất.
>
> Các trạng thái kề giống Lời giải 1 và mỗi cạnh vẫn có trọng số $1$, nên tính tối ưu được bảo toàn; chuỗi có ít vị trí sai khác hơn sẽ được lấy khỏi queue sớm hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kSimilarity(self, s1: str, s2: str) -> int:
        def f(s):
            cnt = sum(c != s2[i] for i, c in enumerate(s))
            return (cnt + 1) >> 1

        def next(s):
            i = 0
            while s[i] == s2[i]:
                i += 1
            res = []
            for j in range(i + 1, n):
                if s[j] == s2[i] and s[j] != s2[j]:
                    res.append(s2[: i + 1] + s[i + 1 : j] + s[i] + s[j + 1 :])
            return res

        q = [(f(s1), s1)]
        dist = {s1: 0}
        n = len(s1)
        while 1:
            _, s = heappop(q)
            if s == s2:
                return dist[s]
            for nxt in next(s):
                if nxt not in dist or dist[nxt] > dist[s] + 1:
                    dist[nxt] = dist[s] + 1
                    heappush(q, (dist[nxt] + f(nxt), nxt))
```

#### Java

```java
class Solution {
    public int kSimilarity(String s1, String s2) {
        PriorityQueue<Pair<Integer, String>> q
            = new PriorityQueue<>(Comparator.comparingInt(Pair::getKey));
        q.offer(new Pair<>(f(s1, s2), s1));
        Map<String, Integer> dist = new HashMap<>();
        dist.put(s1, 0);
        while (true) {
            String s = q.poll().getValue();
            if (s.equals(s2)) {
                return dist.get(s);
            }
            for (String nxt : next(s, s2)) {
                if (!dist.containsKey(nxt) || dist.get(nxt) > dist.get(s) + 1) {
                    dist.put(nxt, dist.get(s) + 1);
                    q.offer(new Pair<>(dist.get(nxt) + f(nxt, s2), nxt));
                }
            }
        }
    }

    private int f(String s, String s2) {
        int cnt = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) != s2.charAt(i)) {
                ++cnt;
            }
        }
        return (cnt + 1) >> 1;
    }

    private List<String> next(String s, String s2) {
        int i = 0, n = s.length();
        char[] cs = s.toCharArray();
        for (; cs[i] == s2.charAt(i); ++i) {
        }

        List<String> res = new ArrayList<>();
        for (int j = i + 1; j < n; ++j) {
            if (cs[j] == s2.charAt(i) && cs[j] != s2.charAt(j)) {
                swap(cs, i, j);
                res.add(new String(cs));
                swap(cs, i, j);
            }
        }
        return res;
    }

    private void swap(char[] cs, int i, int j) {
        char t = cs[i];
        cs[i] = cs[j];
        cs[j] = t;
    }
}
```

#### C++

```cpp
using pis = pair<int, string>;

class Solution {
public:
    int kSimilarity(string s1, string s2) {
        priority_queue<pis, vector<pis>, greater<pis>> q;
        q.push({f(s1, s2), s1});
        unordered_map<string, int> dist;
        dist[s1] = 0;
        while (1) {
            auto [_, s] = q.top();
            q.pop();
            if (s == s2) {
                return dist[s];
            }
            for (auto& nxt : next(s, s2)) {
                if (!dist.count(nxt) || dist[nxt] > dist[s] + 1) {
                    dist[nxt] = dist[s] + 1;
                    q.push({dist[nxt] + f(nxt, s2), nxt});
                }
            }
        }
    }

    int f(string& s, string& s2) {
        int cnt = 0;
        for (int i = 0; i < s.size(); ++i) {
            cnt += s[i] != s2[i];
        }
        return (cnt + 1) >> 1;
    }

    vector<string> next(string& s, string& s2) {
        int i = 0, n = s.size();
        for (; s[i] == s2[i]; ++i) {}
        vector<string> res;
        for (int j = i + 1; j < n; ++j) {
            if (s[j] == s2[i] && s[j] != s2[j]) {
                swap(s[i], s[j]);
                res.push_back(s);
                swap(s[i], s[j]);
            }
        }
        return res;
    }
};
```

#### Go

```go
func kSimilarity(s1 string, s2 string) int {
	next := func(s string) []string {
		i := 0
		res := []string{}
		for ; s[i] == s2[i]; i++ {
		}
		for j := i + 1; j < len(s1); j++ {
			if s[j] == s2[i] && s[j] != s2[j] {
				res = append(res, s[:i]+string(s[j])+s[i+1:j]+string(s[i])+s[j+1:])
			}
		}
		return res
	}

	f := func(s string) int {
		cnt := 0
		for i := range s {
			if s[i] != s2[i] {
				cnt++
			}
		}
		return (cnt + 1) >> 1
	}

	q := hp{pair{f(s1), s1}}
	dist := map[string]int{s1: 0}
	for {
		s := heap.Pop(&q).(pair).s
		if s == s2 {
			return dist[s]
		}
		for _, nxt := range next(s) {
			if v, ok := dist[nxt]; !ok || v > dist[s]+1 {
				dist[nxt] = dist[s] + 1
				heap.Push(&q, pair{dist[nxt] + f(nxt), nxt})
			}
		}
	}
}

type pair struct {
	v int
	s string
}
type hp []pair

func (h hp) Len() int { return len(h) }
func (h hp) Less(i, j int) bool {
	a, b := h[i], h[j]
	return a.v < b.v
}
func (h hp) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)   { *h = append(*h, v.(pair)) }
func (h *hp) Pop() any     { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
