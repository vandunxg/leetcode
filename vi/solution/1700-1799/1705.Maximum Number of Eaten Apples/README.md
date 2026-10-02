---
comments: true
difficulty: Medium
rating: 1929
source: Weekly Contest 221 Q2
tags:
    - Greedy
    - Array
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1705. Maximum Number of Eaten Apples](https://leetcode.com/problems/maximum-number-of-eaten-apples)

[中文文档](/solution/1700-1799/1705.Maximum%20Number%20of%20Eaten%20Apples/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây táo đặc biệt cho quả mỗi ngày trong <code>n</code> ngày. Vào ngày thứ <code>i<sup>th</sup></code>, cây mọc <code>apples[i]</code> quả, sẽ hỏng sau <code>days[i]</code> ngày; nghĩa là đến ngày <code>i + days[i]</code>, táo bị hỏng và không thể ăn. Có những ngày cây không mọc quả, được biểu diễn bởi <code>apples[i] == 0</code> và <code>days[i] == 0</code>.</p>

<p>Bạn quyết định ăn <strong>nhiều nhất</strong> một quả táo mỗi ngày. Lưu ý rằng bạn vẫn có thể ăn sau <code>n</code> ngày đầu tiên.</p>

<p>Cho hai mảng số nguyên <code>days</code> và <code>apples</code> có độ dài <code>n</code>, hãy trả về <em>số quả táo nhiều nhất bạn có thể ăn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> apples = [1,2,3,5,2], days = [3,2,1,4,2]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Bạn có thể ăn 7 quả táo:
- Ngày đầu tiên, bạn ăn một quả mọc vào ngày đầu tiên.
- Ngày thứ hai, bạn ăn một quả mọc vào ngày thứ hai.
- Ngày thứ ba, bạn ăn một quả mọc vào ngày thứ hai. Sau ngày này, những quả mọc vào ngày thứ ba bị hỏng.
- Từ ngày thứ tư đến ngày thứ bảy, bạn ăn những quả mọc vào ngày thứ tư.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> apples = [3,0,0,0,0,2], days = [3,0,0,0,0,2]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Bạn có thể ăn 5 quả táo:
- Từ ngày đầu tiên đến ngày thứ ba, bạn ăn những quả mọc vào ngày đầu tiên.
- Không làm gì vào ngày thứ tư và thứ năm.
- Ngày thứ sáu và thứ bảy, bạn ăn những quả mọc vào ngày thứ sáu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == apples.length == days.length</code></li>
	<li><code>1 &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= apples[i], days[i] &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>days[i] = 0</code> khi và chỉ khi <code>apples[i] = 0</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Hàng đợi ưu tiên

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ngày chỉ được ăn nhiều nhất một quả táo và quả đó không được hỏng. Ăn những quả còn lâu mới hỏng trước sẽ lãng phí các quả sắp hết hạn và làm giảm tổng số táo ăn được.
>
> Trong các quả còn ăn được, luôn ăn quả sắp hỏng sớm nhất. Chính sách này tương ứng với một min-heap, với khóa là thời điểm hết hạn.
>
> Vào ngày $i$, thêm lô táo mới dưới dạng $(\textit{expiry},\textit{count})$. Sau khi loại các lô đã hỏng, lấy một quả ở đầu heap rồi đưa phần còn lại trở lại. Tiếp tục cho đến khi cây không còn ra quả và heap rỗng.

<!-- thinking:end -->

Ta có thể tham lam chọn những quả sắp hỏng nhất trong số các quả chưa hỏng để ăn được nhiều táo nhất.

Do đó, ta dùng priority queue (min-heap) để lưu thời điểm hỏng và số lượng của từng lô táo. Mỗi lần, ta lấy lô có thời điểm hỏng nhỏ nhất, giảm số lượng đi một. Nếu số lượng sau khi giảm vẫn khác không, ta đưa lô đó trở lại priority queue. Nếu táo đã hỏng, ta xóa lô đó khỏi priority queue.

Độ phức tạp thời gian là $O(n \times \log n + M)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài mảng $\textit{days}$ và $M = \max(\textit{days})$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def eatenApples(self, apples: List[int], days: List[int]) -> int:
        n = len(days)
        i = ans = 0
        q = []
        while i < n or q:
            if i < n and apples[i]:
                heappush(q, (i + days[i] - 1, apples[i]))
            while q and q[0][0] < i:
                heappop(q)
            if q:
                t, v = heappop(q)
                v -= 1
                ans += 1
                if v and t > i:
                    heappush(q, (t, v))
            i += 1
        return ans
```

#### Java

```java
class Solution {
    public int eatenApples(int[] apples, int[] days) {
        PriorityQueue<int[]> q = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
        int n = days.length;
        int ans = 0, i = 0;
        while (i < n || !q.isEmpty()) {
            if (i < n && apples[i] > 0) {
                q.offer(new int[] {i + days[i] - 1, apples[i]});
            }
            while (!q.isEmpty() && q.peek()[0] < i) {
                q.poll();
            }
            if (!q.isEmpty()) {
                var p = q.poll();
                ++ans;
                if (--p[1] > 0 && p[0] > i) {
                    q.offer(p);
                }
            }
            ++i;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int eatenApples(vector<int>& apples, vector<int>& days) {
        using pii = pair<int, int>;
        priority_queue<pii, vector<pii>, greater<pii>> q;
        int n = days.size();
        int ans = 0, i = 0;
        while (i < n || !q.empty()) {
            if (i < n && apples[i]) {
                q.emplace(i + days[i] - 1, apples[i]);
            }
            while (!q.empty() && q.top().first < i) {
                q.pop();
            }
            if (!q.empty()) {
                auto [t, v] = q.top();
                q.pop();
                --v;
                ++ans;
                if (v && t > i) {
                    q.emplace(t, v);
                }
            }
            ++i;
        }
        return ans;
    }
};
```

#### Go

```go
func eatenApples(apples []int, days []int) int {
	var h hp
	ans, n := 0, len(apples)
	for i := 0; i < n || len(h) > 0; i++ {
		if i < n && apples[i] > 0 {
			heap.Push(&h, pair{i + days[i] - 1, apples[i]})
		}
		for len(h) > 0 && h[0].first < i {
			heap.Pop(&h)
		}
		if len(h) > 0 {
			h[0].second--
			if h[0].first == i || h[0].second == 0 {
				heap.Pop(&h)
			}
			ans++
		}
	}
	return ans
}

type pair struct {
	first  int
	second int
}

type hp []pair

func (a hp) Len() int           { return len(a) }
func (a hp) Swap(i, j int)      { a[i], a[j] = a[j], a[i] }
func (a hp) Less(i, j int) bool { return a[i].first < a[j].first }
func (a *hp) Push(x any)        { *a = append(*a, x.(pair)) }
func (a *hp) Pop() any          { l := len(*a); t := (*a)[l-1]; *a = (*a)[:l-1]; return t }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
