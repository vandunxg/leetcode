---
comments: true
difficulty: Medium
rating: 1904
source: Biweekly Contest 126 Q3
tags:
    - Greedy
    - Hash Table
    - String
    - Counting
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3081. Replace Question Marks in String to Minimize Its Value](https://leetcode.com/problems/replace-question-marks-in-string-to-minimize-its-value)

[中文文档](/solution/3000-3099/3081.Replace%20Question%20Marks%20in%20String%20to%20Minimize%20Its%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code>. <code>s[i]</code> là một chữ cái tiếng Anh viết thường hoặc <code>&#39;?&#39;</code>.</p>

<p>Với một chuỗi <code>t</code> có độ dài <code>m</code> và <strong>chỉ</strong> chứa các chữ cái tiếng Anh viết thường, ta định nghĩa hàm <code>cost(i)</code> cho chỉ số <code>i</code> là số ký tự <strong>bằng</strong> <code>t[i]</code> đã xuất hiện trước đó, tức trong khoảng <code>[0, i - 1]</code>.</p>

<p><strong>Giá trị</strong> của <code>t</code> là <strong>tổng</strong> của <code>cost(i)</code> trên mọi chỉ số <code>i</code>.</p>

<p>Ví dụ, với chuỗi <code>t = &quot;aab&quot;</code>:</p>

<ul>
	<li><code>cost(0) = 0</code></li>
	<li><code>cost(1) = 1</code></li>
	<li><code>cost(2) = 0</code></li>
	<li>Do đó, giá trị của <code>&quot;aab&quot;</code> là <code>0 + 1 + 0 = 1</code>.</li>
</ul>

<p>Nhiệm vụ của bạn là thay thế <strong>tất cả</strong> các lần xuất hiện của <code>&#39;?&#39;</code> trong <code>s</code> bằng một chữ cái tiếng Anh viết thường bất kỳ sao cho <strong>giá trị</strong> của <code>s</code> là <strong>nhỏ nhất</strong>.</p>

<p>Trả về <em>chuỗi biểu thị chuỗi đã được thay thế các lần xuất hiện của </em><code>&#39;?&#39;</code><em>. Nếu có nhiều chuỗi cho ra <strong>giá trị nhỏ nhất</strong>, hãy trả về chuỗi <span data-keyword="lexicographically-smaller-string">nhỏ nhất theo thứ tự từ điển</span>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> s = &quot;???&quot; </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> &quot;abc&quot; </span></p>

<p><strong>Giải thích: </strong> Trong ví dụ này, ta có thể thay thế các lần xuất hiện của <code>&#39;?&#39;</code> để biến <code>s</code> thành <code>&quot;abc&quot;</code>.</p>

<p>Với <code>&quot;abc&quot;</code>, <code>cost(0) = 0</code>, <code>cost(1) = 0</code> và <code>cost(2) = 0</code>.</p>

<p>Giá trị của <code>&quot;abc&quot;</code> là <code>0</code>.</p>

<p>Một số cách thay thế khác của <code>s</code> cũng cho giá trị <code>0</code> là <code>&quot;cba&quot;</code>, <code>&quot;abz&quot;</code> và <code>&quot;hey&quot;</code>.</p>

<p>Trong số đó, ta chọn chuỗi nhỏ nhất theo thứ tự từ điển.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">s = &quot;a?a?&quot;</span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">&quot;abac&quot;</span></p>

<p><strong>Giải thích: </strong> Trong ví dụ này, các lần xuất hiện của <code>&#39;?&#39;</code> có thể được thay thế để biến <code>s</code> thành <code>&quot;abac&quot;</code>.</p>

<p>Với <code>&quot;abac&quot;</code>, <code>cost(0) = 0</code>, <code>cost(1) = 0</code>, <code>cost(2) = 1</code> và <code>cost(3) = 0</code>.</p>

<p>Giá trị của <code>&quot;abac&quot;</code> là&nbsp;<code>1</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là một chữ cái tiếng Anh viết thường hoặc <code>&#39;?&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Priority Queue

<!-- thinking:start -->

> **Tư duy**
>
> Các dấu hỏi được thay bằng chữ cái tiếng Anh viết thường để tối thiểu hóa $\sum \textit{freq}(\textit{freq}-1)/2$, sau đó tạo ra chuỗi nhỏ nhất theo thứ tự từ điển. $n \le 10^5$.
>
> Chi phí bậc hai nhỏ nhất khi tần suất được cân bằng, vì vậy mỗi `?` nên được thay bằng chữ cái hiện có tần suất nhỏ nhất. Tập đa phần tử thu được sẽ được ghi lại vào các vị trí `?` theo thứ tự đã sắp xếp.
>
> Một min-heap gồm $26$ cặp $(\textit{cnt},c)$ tạo ra tập đa phần tử này; ta sắp xếp tập đó rồi thay thế các dấu hỏi từ trái sang phải.

<!-- thinking:end -->

Theo đề bài, ta thấy rằng nếu một chữ cái $c$ xuất hiện $v$ lần, thì điểm số mà nó đóng góp vào đáp án là $1 + 2 + \cdots + (v - 1) = \frac{v \times (v - 1)}{2}$. Để đáp án nhỏ nhất có thể, ta nên thay thế các dấu hỏi bằng những chữ cái xuất hiện ít hơn.

Do đó, ta có thể dùng priority queue để duy trì số lần xuất hiện của mỗi chữ cái, mỗi lần lấy ra chữ cái có số lần xuất hiện nhỏ nhất, ghi nó vào mảng $t$, sau đó tăng số lần xuất hiện của chữ cái đó lên một và đưa nó trở lại priority queue. Cuối cùng, ta sắp xếp mảng $t$, rồi duyệt chuỗi $s$ và lần lượt thay thế mỗi dấu hỏi bằng các chữ cái trong mảng $t$.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimizeStringValue(self, s: str) -> str:
        cnt = Counter(s)
        pq = [(cnt[c], c) for c in ascii_lowercase]
        heapify(pq)
        t = []
        for _ in range(s.count("?")):
            v, c = pq[0]
            t.append(c)
            heapreplace(pq, (v + 1, c))
        t.sort()
        cs = list(s)
        j = 0
        for i, c in enumerate(s):
            if c == "?":
                cs[i] = t[j]
                j += 1
        return "".join(cs)
```

#### Java

```java
class Solution {
    public String minimizeStringValue(String s) {
        int[] cnt = new int[26];
        int n = s.length();
        int k = 0;
        char[] cs = s.toCharArray();
        for (char c : cs) {
            if (c == '?') {
                ++k;
            } else {
                ++cnt[c - 'a'];
            }
        }
        PriorityQueue<int[]> pq
            = new PriorityQueue<>((a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        for (int i = 0; i < 26; ++i) {
            pq.offer(new int[] {cnt[i], i});
        }
        int[] t = new int[k];
        for (int j = 0; j < k; ++j) {
            int[] p = pq.poll();
            t[j] = p[1];
            pq.offer(new int[] {p[0] + 1, p[1]});
        }
        Arrays.sort(t);

        for (int i = 0, j = 0; i < n; ++i) {
            if (cs[i] == '?') {
                cs[i] = (char) (t[j++] + 'a');
            }
        }
        return new String(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string minimizeStringValue(string s) {
        int cnt[26]{};
        int k = 0;
        for (char& c : s) {
            if (c == '?') {
                ++k;
            } else {
                ++cnt[c - 'a'];
            }
        }
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
        for (int i = 0; i < 26; ++i) {
            pq.push({cnt[i], i});
        }
        vector<int> t(k);
        for (int i = 0; i < k; ++i) {
            auto [v, c] = pq.top();
            pq.pop();
            t[i] = c;
            pq.push({v + 1, c});
        }
        sort(t.begin(), t.end());
        int j = 0;
        for (char& c : s) {
            if (c == '?') {
                c = t[j++] + 'a';
            }
        }
        return s;
    }
};
```

#### Go

```go
func minimizeStringValue(s string) string {
	cnt := [26]int{}
	k := 0
	for _, c := range s {
		if c == '?' {
			k++
		} else {
			cnt[c-'a']++
		}
	}
	pq := hp{}
	for i, c := range cnt {
		heap.Push(&pq, pair{c, i})
	}
	t := make([]int, k)
	for i := 0; i < k; i++ {
		p := heap.Pop(&pq).(pair)
		t[i] = p.c
		p.v++
		heap.Push(&pq, p)
	}
	sort.Ints(t)
	cs := []byte(s)
	j := 0
	for i, c := range cs {
		if c == '?' {
			cs[i] = byte(t[j] + 'a')
			j++
		}
	}
	return string(cs)
}

type pair struct{ v, c int }
type hp []pair

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].v < h[j].v || h[i].v == h[j].v && h[i].c < h[j].c }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)        { *h = append(*h, v.(pair)) }
func (h *hp) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
