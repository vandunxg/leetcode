---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Hash Table
    - String
    - Counting
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [358. Rearrange String k Distance Apart 🔒](https://leetcode.com/problems/rearrange-string-k-distance-apart)

[中文文档](/solution/0300-0399/0358.Rearrange%20String%20k%20Distance%20Apart/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và số nguyên <code>k</code>. Hãy sắp xếp lại <code>s</code> sao cho các ký tự giống nhau cách nhau ít nhất <code>k</code> vị trí. Nếu không thể sắp xếp lại chuỗi thỏa điều kiện, hãy trả về chuỗi rỗng <code>&quot;&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aabbcc&quot;, k = 3
<strong>Đầu ra:</strong> &quot;abcabc&quot;
<strong>Giải thích:</strong> Các chữ cái giống nhau cách nhau ít nhất 3 vị trí.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaabc&quot;, k = 3
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong> Không thể sắp xếp lại chuỗi để thỏa điều kiện.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaadbbcc&quot;, k = 2
<strong>Đầu ra:</strong> &quot;abacabcd&quot;
<strong>Giải thích:</strong> Các chữ cái giống nhau cách nhau ít nhất 2 vị trí.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 3 * 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>0 &lt;= k &lt;= s.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Hash Table + Priority Queue (Max Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp lại để các ký tự giống nhau cách nhau ít nhất $k$ vị trí; nếu không thể thì trả về chuỗi rỗng. Ưu tiên đặt ký tự xuất hiện nhiều trước rồi tạm giữ chúng trong queue.
>
> Mỗi lần lấy một ký tự khỏi max-heap, thêm ký tự đó vào kết quả rồi đưa nó vào queue trong $k$ lượt. Khi queue đủ $k$ phần tử, đưa ký tự có số lần xuất hiện còn lại vào lại heap. Nếu heap rỗng trước khi tạo đủ độ dài chuỗi thì không có cách sắp xếp hợp lệ.

<!-- thinking:end -->

Ta dùng hash table hoặc mảng $\textit{cnt}$ để đếm số lần xuất hiện của từng ký tự trong chuỗi. Sau đó, dùng max-heap $\textit{pq}$ để lưu từng ký tự cùng số lần xuất hiện. Mỗi phần tử trong heap là tuple $(v, c)$, trong đó $v$ là số lần xuất hiện còn lại và $c$ là ký tự.

Khi sắp xếp chuỗi, ta liên tục pop phần tử đầu $(v, c)$ khỏi heap, thêm ký tự $c$ vào chuỗi kết quả, rồi đưa $(v-1, c)$ vào queue $\textit{q}$. Khi queue $\textit{q}$ có ít nhất $k$ phần tử, ta pop phần tử đầu queue; nếu $v > 0$, đưa phần tử đó trở lại heap. Lặp lại cho đến khi heap rỗng.

Cuối cùng, kiểm tra độ dài chuỗi kết quả có bằng độ dài chuỗi ban đầu hay không. Nếu bằng, trả về chuỗi kết quả; nếu không, trả về chuỗi rỗng.

Độ phức tạp thời gian là $O(n \log n)$, với $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $|\Sigma|$ là kích thước bộ ký tự, bằng $26$ trong bài này.

Các bài tương tự:

- [767. Reorganize String](https://github.com/doocs/leetcode/blob/main/solution/0700-0799/0767.Reorganize%20String/README.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rearrangeString(self, s: str, k: int) -> str:
        cnt = Counter(s)
        pq = [(-v, c) for c, v in cnt.items()]
        heapify(pq)
        q = deque()
        ans = []
        while pq:
            v, c = heappop(pq)
            ans.append(c)
            q.append((v + 1, c))
            if len(q) >= k:
                e = q.popleft()
                if e[0]:
                    heappush(pq, e)
        return "" if len(ans) < len(s) else "".join(ans)
```

#### Java

```java
class Solution {
    public String rearrangeString(String s, int k) {
        int[] cnt = new int[26];
        for (char c : s.toCharArray()) {
            ++cnt[c - 'a'];
        }
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> b[0] - a[0]);
        for (int i = 0; i < cnt.length; ++i) {
            if (cnt[i] > 0) {
                pq.offer(new int[] {cnt[i], i});
            }
        }
        Deque<int[]> q = new ArrayDeque<>();
        StringBuilder ans = new StringBuilder();
        while (!pq.isEmpty()) {
            var p = pq.poll();
            p[0] -= 1;
            ans.append((char) ('a' + p[1]));
            q.offerLast(p);
            if (q.size() >= k) {
                p = q.pollFirst();
                if (p[0] > 0) {
                    pq.offer(p);
                }
            }
        }
        return ans.length() < s.length() ? "" : ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string rearrangeString(string s, int k) {
        vector<int> cnt(26, 0);
        for (char c : s) {
            ++cnt[c - 'a'];
        }

        priority_queue<pair<int, int>> pq;
        for (int i = 0; i < 26; ++i) {
            if (cnt[i] > 0) {
                pq.emplace(cnt[i], i);
            }
        }

        queue<pair<int, int>> q;
        string ans;
        while (!pq.empty()) {
            auto p = pq.top();
            pq.pop();
            p.first -= 1;
            ans.push_back('a' + p.second);
            q.push(p);
            if (q.size() >= k) {
                p = q.front();
                q.pop();
                if (p.first > 0) {
                    pq.push(p);
                }
            }
        }

        return ans.size() < s.size() ? "" : ans;
    }
};
```

#### Go

```go
func rearrangeString(s string, k int) string {
	cnt := [26]int{}
	for _, c := range s {
		cnt[c-'a']++
	}
	pq := priorityqueue.NewWith(func(a, b any) int {
		x := a.([2]int)
		y := b.([2]int)
		return y[0] - x[0]
	})

	for i := 0; i < 26; i++ {
		if cnt[i] > 0 {
			pq.Enqueue([2]int{cnt[i], i})
		}
	}

	var q [][2]int
	var ans strings.Builder

	for pq.Size() > 0 {
		p, _ := pq.Dequeue()
		pair := p.([2]int)
		pair[0]--
		ans.WriteByte(byte('a' + pair[1]))
		q = append(q, pair)

		if len(q) >= k {
			front := q[0]
			q = q[1:]
			if front[0] > 0 {
				pq.Enqueue(front)
			}
		}
	}

	if ans.Len() < len(s) {
		return ""
	}
	return ans.String()
}
```

#### TypeScript

```ts
export function rearrangeString(s: string, k: number): string {
    const cnt: number[] = Array(26).fill(0);
    for (const c of s) {
        cnt[c.charCodeAt(0) - 'a'.charCodeAt(0)]++;
    }

    const pq = new PriorityQueue<[number, number]>((a, b) => b[0] - a[0]);
    for (let i = 0; i < 26; i++) {
        if (cnt[i] > 0) {
            pq.enqueue([cnt[i], i]);
        }
    }

    const q: [number, number][] = [];
    const ans: string[] = [];
    while (!pq.isEmpty()) {
        const [count, idx] = pq.dequeue()!;
        const newCount = count - 1;
        ans.push(String.fromCharCode('a'.charCodeAt(0) + idx));
        q.push([newCount, idx]);
        if (q.length >= k) {
            const [frontCount, frontIdx] = q.shift()!;
            if (frontCount > 0) {
                pq.enqueue([frontCount, frontIdx]);
            }
        }
    }
    return ans.length < s.length ? '' : ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
