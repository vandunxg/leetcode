---
comments: true
difficulty: Medium
rating: 1763
source: Weekly Contest 318 Q3
tags:
    - Array
    - Two Pointers
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2462. Total Cost to Hire K Workers](https://leetcode.com/problems/total-cost-to-hire-k-workers)

[中文文档](/solution/2400-2499/2462.Total%20Cost%20to%20Hire%20K%20Workers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> là <code>costs</code>, trong đó <code>costs[i]</code> là chi phí thuê công nhân thứ <code>i<sup>th</sup></code>.</p>

<p>Bạn cũng được cho hai số nguyên <code>k</code> và <code>candidates</code>. Ta muốn thuê chính xác <code>k</code> công nhân theo các quy tắc sau:</p>

<ul>
	<li>Ta sẽ thực hiện <code>k</code> phiên tuyển dụng và thuê chính xác một công nhân trong mỗi phiên.</li>
	<li>Trong mỗi phiên tuyển dụng, chọn công nhân có chi phí thấp nhất trong số <code>candidates</code> công nhân đầu tiên hoặc <code>candidates</code> công nhân cuối cùng. Nếu có nhiều công nhân cùng có chi phí thấp nhất, chọn công nhân có chỉ số nhỏ nhất.
	<ul>
		<li>Ví dụ, nếu <code>costs = [3,2,7,7,1,2]</code> và <code>candidates = 2</code>, trong phiên tuyển dụng đầu tiên, ta sẽ chọn công nhân thứ <code>4<sup>th</sup></code> vì họ có chi phí thấp nhất <code>[<u>3,2</u>,7,7,<u><strong>1</strong>,2</u>]</code>.</li>
		<li>Trong phiên tuyển dụng thứ hai, ta sẽ chọn công nhân thứ <code>1<sup>st</sup></code> vì họ có cùng chi phí thấp nhất với công nhân thứ <code>4<sup>th</sup></code>, nhưng có chỉ số nhỏ hơn <code>[<u>3,<strong>2</strong></u>,7,<u>7,2</u>]</code>. Lưu ý rằng chỉ số có thể thay đổi trong quá trình này.</li>
	</ul>
	</li>
	<li>Nếu còn ít hơn candidates công nhân, chọn công nhân có chi phí thấp nhất trong số đó. Nếu có nhiều công nhân cùng có chi phí thấp nhất, chọn công nhân có chỉ số nhỏ nhất.</li>
	<li>Mỗi công nhân chỉ có thể được chọn một lần.</li>
</ul>

<p>Trả về <em>tổng chi phí để thuê chính xác </em><code>k</code><em> công nhân.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> costs = [17,12,10,2,7,2,11,20,8], k = 3, candidates = 4
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Tổng cộng ta thuê 3 công nhân. Ban đầu tổng chi phí là 0.
- Trong vòng tuyển dụng đầu tiên, ta chọn công nhân từ [<u>17,12,10,2</u>,7,<u>2,11,20,8</u>]. Chi phí thấp nhất là 2, và khi có nhiều công nhân cùng chi phí, ta chọn công nhân có chỉ số nhỏ nhất, tức là chỉ số 3. Tổng chi phí = 0 + 2 = 2.
- Trong vòng tuyển dụng thứ hai, ta chọn công nhân từ [<u>17,12,10,7</u>,<u>2,11,20,8</u>]. Chi phí thấp nhất là 2 (chỉ số 4). Tổng chi phí = 2 + 2 = 4.
- Trong vòng tuyển dụng thứ ba, ta chọn công nhân từ [<u>17,12,10,7,11,20,8</u>]. Chi phí thấp nhất là 7 (chỉ số 3). Tổng chi phí = 4 + 7 = 11. Lưu ý rằng công nhân có chỉ số 3 nằm đồng thời trong bốn công nhân đầu tiên và bốn công nhân cuối cùng.
Tổng chi phí thuê là 11.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> costs = [1,2,4,1], k = 3, candidates = 3
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Tổng cộng ta thuê 3 công nhân. Ban đầu tổng chi phí là 0.
- Trong vòng tuyển dụng đầu tiên, ta chọn công nhân từ [<u>1,2,4,1</u>]. Chi phí thấp nhất là 1, và khi có nhiều công nhân cùng chi phí, ta chọn công nhân có chỉ số nhỏ nhất, tức là chỉ số 0. Tổng chi phí = 0 + 1 = 1. Lưu ý rằng các công nhân có chỉ số 1 và 2 nằm đồng thời trong ba công nhân đầu tiên và ba công nhân cuối cùng.
- Trong vòng tuyển dụng thứ hai, ta chọn công nhân từ [<u>2,4,1</u>]. Chi phí thấp nhất là 1 (chỉ số 2). Tổng chi phí = 1 + 1 = 2.
- Trong vòng tuyển dụng thứ ba, còn ít hơn ba công nhân. Ta chọn công nhân từ những công nhân còn lại [<u>2,4</u>]. Chi phí thấp nhất là 2 (chỉ số 0). Tổng chi phí = 2 + 2 = 4.
Tổng chi phí thuê là 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= costs.length &lt;= 10<sup>5 </sup></code></li>
	<li><code>1 &lt;= costs[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k, candidates &lt;= costs.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Priority Queue (Min Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần thuê trong tổng số $k$ lần, ta chọn công nhân rẻ nhất trong số $\textit{candidates}$ công nhân đầu và cuối; $n\le 10^5$. Nếu hai phía bao phủ toàn bộ mảng, chỉ cần chọn $k$ công nhân có chi phí nhỏ nhất trên toàn cục.
>
> Nếu không, một heap lưu cả hai phía (kèm chỉ số). Sau khi lấy một công nhân ra, ta đưa công nhân chưa dùng tiếp theo ở phía tương ứng vào heap. Dừng bổ sung khi hai con trỏ vượt qua nhau.

<!-- thinking:end -->

Trước hết, ta kiểm tra xem $candidates \times 2$ có lớn hơn hoặc bằng $n$ hay không. Nếu có, ta trả về ngay tổng chi phí của $k$ công nhân có chi phí nhỏ nhất.

Nếu không, ta sử dụng một min heap $pq$ để duy trì chi phí của $candidates$ công nhân đầu tiên và $candidates$ công nhân cuối cùng.

Đầu tiên, ta thêm chi phí và chỉ số tương ứng của $candidates$ công nhân đầu tiên vào min heap $pq$, sau đó thêm chi phí và chỉ số tương ứng của $candidates$ công nhân cuối cùng vào min heap $pq$. Ta dùng hai con trỏ $l$ và $r$ lần lượt trỏ đến các chỉ số của nhóm công nhân ở đầu và cuối, ban đầu $l = candidates$, $r = n - candidates - 1$.

Sau đó, ta thực hiện $k$ lần lặp. Trong mỗi lần, ta lấy công nhân có chi phí nhỏ nhất từ min heap $pq$ và cộng chi phí của họ vào đáp án. Nếu $l > r$, nghĩa là toàn bộ công nhân ở hai nhóm đầu và cuối đã được chọn, nên ta bỏ qua phần còn lại. Ngược lại, nếu chỉ số của công nhân hiện tại nhỏ hơn $l$, đó là công nhân ở phía đầu; ta thêm chi phí và chỉ số của công nhân thứ $l$ vào min heap $pq$, sau đó tăng $l$. Nếu không, ta thêm chi phí và chỉ số của công nhân thứ $r$ vào min heap $pq$, sau đó giảm $r$.

Sau khi vòng lặp kết thúc, ta trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $costs$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def totalCost(self, costs: List[int], k: int, candidates: int) -> int:
        n = len(costs)
        if candidates * 2 >= n:
            return sum(sorted(costs)[:k])
        pq = []
        for i, c in enumerate(costs[:candidates]):
            heappush(pq, (c, i))
        for i in range(n - candidates, n):
            heappush(pq, (costs[i], i))
        heapify(pq)
        l, r = candidates, n - candidates - 1
        ans = 0
        for _ in range(k):
            c, i = heappop(pq)
            ans += c
            if l > r:
                continue
            if i < l:
                heappush(pq, (costs[l], l))
                l += 1
            else:
                heappush(pq, (costs[r], r))
                r -= 1
        return ans
```

#### Java

```java
class Solution {
    public long totalCost(int[] costs, int k, int candidates) {
        int n = costs.length;
        long ans = 0;
        if (candidates * 2 >= n) {
            Arrays.sort(costs);
            for (int i = 0; i < k; ++i) {
                ans += costs[i];
            }
            return ans;
        }
        PriorityQueue<int[]> pq
            = new PriorityQueue<>((a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        for (int i = 0; i < candidates; ++i) {
            pq.offer(new int[] {costs[i], i});
            pq.offer(new int[] {costs[n - i - 1], n - i - 1});
        }
        int l = candidates, r = n - candidates - 1;
        while (k-- > 0) {
            var p = pq.poll();
            ans += p[0];
            if (l > r) {
                continue;
            }
            if (p[1] < l) {
                pq.offer(new int[] {costs[l], l++});
            } else {
                pq.offer(new int[] {costs[r], r--});
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long totalCost(vector<int>& costs, int k, int candidates) {
        int n = costs.size();
        if (candidates * 2 > n) {
            sort(costs.begin(), costs.end());
            return accumulate(costs.begin(), costs.begin() + k, 0LL);
        }
        using pii = pair<int, int>;
        priority_queue<pii, vector<pii>, greater<pii>> pq;
        for (int i = 0; i < candidates; ++i) {
            pq.emplace(costs[i], i);
            pq.emplace(costs[n - i - 1], n - i - 1);
        }
        long long ans = 0;
        int l = candidates, r = n - candidates - 1;
        while (k--) {
            auto [cost, i] = pq.top();
            pq.pop();
            ans += cost;
            if (l > r) {
                continue;
            }
            if (i < l) {
                pq.emplace(costs[l], l++);
            } else {
                pq.emplace(costs[r], r--);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func totalCost(costs []int, k int, candidates int) (ans int64) {
	n := len(costs)
	if candidates*2 > n {
		sort.Ints(costs)
		for _, x := range costs[:k] {
			ans += int64(x)
		}
		return
	}
	pq := hp{}
	for i, x := range costs[:candidates] {
		heap.Push(&pq, pair{x, i})
		heap.Push(&pq, pair{costs[n-i-1], n - i - 1})
	}
	l, r := candidates, n-candidates-1
	for ; k > 0; k-- {
		p := heap.Pop(&pq).(pair)
		ans += int64(p.cost)
		if l > r {
			continue
		}
		if p.i < l {
			heap.Push(&pq, pair{costs[l], l})
			l++
		} else {
			heap.Push(&pq, pair{costs[r], r})
			r--
		}
	}
	return
}

type pair struct{ cost, i int }
type hp []pair

func (h hp) Len() int { return len(h) }
func (h hp) Less(i, j int) bool {
	return h[i].cost < h[j].cost || (h[i].cost == h[j].cost && h[i].i < h[j].i)
}
func (h hp) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)   { *h = append(*h, v.(pair)) }
func (h *hp) Pop() any     { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

#### TypeScript

```ts
function totalCost(costs: number[], k: number, candidates: number): number {
    const n = costs.length;
    if (candidates * 2 >= n) {
        costs.sort((a, b) => a - b);
        return costs.slice(0, k).reduce((acc, x) => acc + x, 0);
    }
    const pq = new PriorityQueue<number[]>((a, b) => (a[0] === b[0] ? a[1] - b[1] : a[0] - b[0]));
    for (let i = 0; i < candidates; ++i) {
        pq.enqueue([costs[i], i]);
        pq.enqueue([costs[n - i - 1], n - i - 1]);
    }
    let [l, r] = [candidates, n - candidates - 1];
    let ans = 0;
    while (k--) {
        const [cost, i] = pq.dequeue()!;
        ans += cost;
        if (l > r) {
            continue;
        }
        if (i < l) {
            pq.enqueue([costs[l], l++]);
        } else {
            pq.enqueue([costs[r], r--]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
