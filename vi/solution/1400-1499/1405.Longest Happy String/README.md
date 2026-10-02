---
comments: true
difficulty: Medium
rating: 1820
source: Weekly Contest 183 Q3
tags:
    - Greedy
    - String
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1405. Longest Happy String](https://leetcode.com/problems/longest-happy-string)

[中文文档](/solution/1400-1499/1405.Longest%20Happy%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuỗi <code>s</code> được gọi là <strong>happy</strong> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li><code>s</code> chỉ chứa các chữ cái <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code> và <code>&#39;c&#39;</code>.</li>
	<li><code>s</code> không chứa bất kỳ chuỗi con nào là <code>&quot;aaa&quot;</code>, <code>&quot;bbb&quot;</code> hoặc <code>&quot;ccc&quot;</code>.</li>
	<li><code>s</code> chứa <strong>không quá</strong> <code>a</code> lần xuất hiện của chữ cái <code>&#39;a&#39;</code>.</li>
	<li><code>s</code> chứa <strong>không quá</strong> <code>b</code> lần xuất hiện của chữ cái <code>&#39;b&#39;</code>.</li>
	<li><code>s</code> chứa <strong>không quá</strong> <code>c</code> lần xuất hiện của chữ cái <code>&#39;c&#39;</code>.</li>
</ul>

<p>Cho ba số nguyên <code>a</code>, <code>b</code> và <code>c</code>, hãy trả về <em>chuỗi <strong>happy</strong> dài nhất có thể</em>. Nếu có nhiều chuỗi <code>happy</code> dài nhất, hãy trả về <em>bất kỳ chuỗi nào trong số đó</em>. Nếu không tồn tại chuỗi như vậy, hãy trả về <em>chuỗi rỗng </em><code>&quot;&quot;</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 1, b = 1, c = 7
<strong>Đầu ra:</strong> &quot;ccaccbcc&quot;
<strong>Giải thích:</strong> &quot;ccbccacc&quot; cũng là một đáp án đúng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 7, b = 1, c = 0
<strong>Đầu ra:</strong> &quot;aabaa&quot;
<strong>Giải thích:</strong> Trong trường hợp này, đây là đáp án đúng duy nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= a, b, c &lt;= 100</code></li>
	<li><code>a + b + c &gt; 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Priority Queue

<!-- thinking:start -->

> **Tư duy**
>
> Vì $a+b+c\le 300$, ta có thể tìm kiếm, nhưng đáp án tối ưu tuân theo một quy tắc cục bộ: luôn dùng chữ cái còn nhiều nhất, miễn là không tạo ra ba ký tự giống nhau liên tiếp.
>
> Nếu hai ký tự cuối đã là chữ cái đó, hãy dùng chữ cái có số lượng còn lại lớn thứ hai. Max-heap theo số lượng còn lại thực hiện được điều này: lấy phần tử đầu, thêm nếu hợp lệ; nếu không thì lấy phần tử tiếp theo, sau đó đưa các phần còn lại trở lại heap.
>
> Dừng lại khi không thể thêm chữ cái nào, khi đó ta thu được chuỗi happy dài nhất.

<!-- thinking:end -->

Chiến lược greedy ưu tiên chọn ký tự có số lần xuất hiện còn lại nhiều nhất. Bằng cách sử dụng priority queue hoặc sắp xếp, ta đảm bảo mỗi lần chọn ký tự có số lần xuất hiện còn lại nhiều nhất (để tránh có ba ký tự giống nhau liên tiếp, trong một số trường hợp ta cần chọn ký tự có số lần xuất hiện còn lại nhiều thứ hai).

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestDiverseString(self, a: int, b: int, c: int) -> str:
        h = []
        if a > 0:
            heappush(h, [-a, 'a'])
        if b > 0:
            heappush(h, [-b, 'b'])
        if c > 0:
            heappush(h, [-c, 'c'])

        ans = []
        while len(h) > 0:
            cur = heappop(h)
            if len(ans) >= 2 and ans[-1] == cur[1] and ans[-2] == cur[1]:
                if len(h) == 0:
                    break
                nxt = heappop(h)
                ans.append(nxt[1])
                if -nxt[0] > 1:
                    nxt[0] += 1
                    heappush(h, nxt)
                heappush(h, cur)
            else:
                ans.append(cur[1])
                if -cur[0] > 1:
                    cur[0] += 1
                    heappush(h, cur)

        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String longestDiverseString(int a, int b, int c) {
        Queue<int[]> pq = new PriorityQueue<>((x, y) -> y[1] - x[1]);
        if (a > 0) {
            pq.offer(new int[] {'a', a});
        }
        if (b > 0) {
            pq.offer(new int[] {'b', b});
        }
        if (c > 0) {
            pq.offer(new int[] {'c', c});
        }

        StringBuilder sb = new StringBuilder();
        while (pq.size() > 0) {
            int[] cur = pq.poll();
            int n = sb.length();
            if (n >= 2 && sb.codePointAt(n - 1) == cur[0] && sb.codePointAt(n - 2) == cur[0]) {
                if (pq.size() == 0) {
                    break;
                }
                int[] next = pq.poll();
                sb.append((char) next[0]);
                if (next[1] > 1) {
                    next[1]--;
                    pq.offer(next);
                }
                pq.offer(cur);
            } else {
                sb.append((char) cur[0]);
                if (cur[1] > 1) {
                    cur[1]--;
                    pq.offer(cur);
                }
            }
        }

        return sb.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string longestDiverseString(int a, int b, int c) {
        using pci = pair<char, int>;
        auto cmp = [](pci x, pci y) { return x.second < y.second; };
        priority_queue<pci, vector<pci>, decltype(cmp)> pq(cmp);

        if (a > 0) pq.push({'a', a});
        if (b > 0) pq.push({'b', b});
        if (c > 0) pq.push({'c', c});

        string ans;
        while (!pq.empty()) {
            pci cur = pq.top();
            pq.pop();
            int n = ans.size();
            if (n >= 2 && ans[n - 1] == cur.first && ans[n - 2] == cur.first) {
                if (pq.empty()) break;
                pci nxt = pq.top();
                pq.pop();
                ans.push_back(nxt.first);
                if (--nxt.second > 0) {
                    pq.push(nxt);
                }
                pq.push(cur);
            } else {
                ans.push_back(cur.first);
                if (--cur.second > 0) {
                    pq.push(cur);
                }
            }
        }

        return ans;
    }
};
```

#### Go

```go
type pair struct {
	c   byte
	num int
}

type hp []pair

func (a hp) Len() int           { return len(a) }
func (a hp) Swap(i, j int)      { a[i], a[j] = a[j], a[i] }
func (a hp) Less(i, j int) bool { return a[i].num > a[j].num }
func (a *hp) Push(x any)        { *a = append(*a, x.(pair)) }
func (a *hp) Pop() any          { l := len(*a); t := (*a)[l-1]; *a = (*a)[:l-1]; return t }

func longestDiverseString(a int, b int, c int) string {
	var h hp
	if a > 0 {
		heap.Push(&h, pair{'a', a})
	}
	if b > 0 {
		heap.Push(&h, pair{'b', b})
	}
	if c > 0 {
		heap.Push(&h, pair{'c', c})
	}

	var ans []byte
	for len(h) > 0 {
		cur := heap.Pop(&h).(pair)
		if len(ans) >= 2 && ans[len(ans)-1] == cur.c && ans[len(ans)-2] == cur.c {
			if len(h) == 0 {
				break
			}
			next := heap.Pop(&h).(pair)
			ans = append(ans, next.c)
			if next.num > 1 {
				next.num--
				heap.Push(&h, next)
			}
			heap.Push(&h, cur)
		} else {
			ans = append(ans, cur.c)
			if cur.num > 1 {
				cur.num--
				heap.Push(&h, cur)
			}
		}
	}

	return string(ans)
}
```

#### TypeScript

```ts
function longestDiverseString(a: number, b: number, c: number): string {
    const pq = new PriorityQueue<number[]>((a, b) => b[1] - a[1]);

    if (a > 0) pq.enqueue(['a'.charCodeAt(0), a]);
    if (b > 0) pq.enqueue(['b'.charCodeAt(0), b]);
    if (c > 0) pq.enqueue(['c'.charCodeAt(0), c]);

    const sb: number[] = [];

    while (!pq.isEmpty()) {
        const cur = pq.dequeue();
        const n = sb.length;

        if (n >= 2 && sb[n - 1] === cur[0] && sb[n - 2] === cur[0]) {
            if (pq.isEmpty()) break;

            const next = pq.dequeue();
            sb.push(next[0]);

            if (next[1] > 1) {
                next[1]--;
                pq.enqueue(next);
            }
            pq.enqueue(cur);
        } else {
            sb.push(cur[0]);
            if (cur[1] > 1) {
                cur[1]--;
                pq.enqueue(cur);
            }
        }
    }

    return String.fromCharCode(...sb);
}
```

<!-- tabs:end -->
