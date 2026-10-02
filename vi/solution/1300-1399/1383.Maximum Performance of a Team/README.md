---
comments: true
difficulty: Hard
rating: 2091
source: Weekly Contest 180 Q4
tags:
    - Greedy
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1383. Maximum Performance of a Team](https://leetcode.com/problems/maximum-performance-of-a-team)

[中文文档](/solution/1300-1399/1383.Maximum%20Performance%20of%20a%20Team/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <code>n</code>, <code>k</code> và hai mảng số nguyên <code>speed</code>, <code>efficiency</code>, mỗi mảng có độ dài <code>n</code>. Có <code>n</code> kỹ sư được đánh số từ <code>1</code> đến <code>n</code>. <code>speed[i]</code> và <code>efficiency[i]</code> lần lượt biểu thị tốc độ và hiệu suất của kỹ sư thứ <code>i<sup>th</sup></code>.</p>

<p>Chọn <strong>tối đa</strong> <code>k</code> kỹ sư khác nhau trong số <code>n</code> kỹ sư để lập đội có <strong>hiệu suất</strong> cao nhất.</p>

<p>Hiệu suất của một đội bằng tổng tốc độ của các kỹ sư nhân với hiệu suất thấp nhất trong đội.</p>

<p>Hãy trả về <em>hiệu suất lớn nhất của đội</em>. Vì đáp án có thể rất lớn, hãy trả về kết quả theo <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 2
<strong>Đầu ra:</strong> 60
<strong>Giải thích:</strong> 
Ta đạt hiệu suất tối đa khi chọn kỹ sư số 2 (có speed=10 và efficiency=4) và kỹ sư số 5 (có speed=5 và efficiency=7). Khi đó, performance = (10 + 5) * min(4, 7) = 60.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 3
<strong>Đầu ra:</strong> 68
<strong>Giải thích:
</strong>Đây là ví dụ giống ví dụ đầu tiên nhưng với k = 3. Ta có thể chọn kỹ sư số 1, số 2 và số 5 để đạt hiệu suất đội cao nhất. Khi đó, performance = (2 + 10 + 5) * min(5, 4, 7) = 68.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 4
<strong>Đầu ra:</strong> 72
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>speed.length == n</code></li>
	<li><code>efficiency.length == n</code></li>
	<li><code>1 &lt;= speed[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= efficiency[i] &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Điểm của đội gồm tối đa $k$ người bằng tổng tốc độ nhân với hiệu suất thấp nhất. Vì $n \le 10^5$, không thể duyệt các tập con. Duyệt kỹ sư theo hiệu suất từ cao xuống thấp sẽ biến hiệu suất hiện tại thành mức thấp nhất của đội; ta chỉ giữ lại tối đa $k$ người nhanh nhất đã xét. Min-heap loại kỹ sư chậm nhất khi kích thước đạt $k$, rồi cập nhật đáp án bằng tổng tốc độ nhân với hiệu suất hiện tại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxPerformance(
        self, n: int, speed: List[int], efficiency: List[int], k: int
    ) -> int:
        t = sorted(zip(speed, efficiency), key=lambda x: -x[1])
        ans = tot = 0
        mod = 10**9 + 7
        h = []
        for s, e in t:
            tot += s
            ans = max(ans, tot * e)
            heappush(h, s)
            if len(h) == k:
                tot -= heappop(h)
        return ans % mod
```

#### Java

```java
class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int maxPerformance(int n, int[] speed, int[] efficiency, int k) {
        int[][] t = new int[n][2];
        for (int i = 0; i < n; ++i) {
            t[i] = new int[] {speed[i], efficiency[i]};
        }
        Arrays.sort(t, (a, b) -> b[1] - a[1]);
        PriorityQueue<Integer> q = new PriorityQueue<>();
        long tot = 0;
        long ans = 0;
        for (var x : t) {
            int s = x[0], e = x[1];
            tot += s;
            ans = Math.max(ans, tot * e);
            q.offer(s);
            if (q.size() == k) {
                tot -= q.poll();
            }
        }
        return (int) (ans % MOD);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxPerformance(int n, vector<int>& speed, vector<int>& efficiency, int k) {
        vector<pair<int, int>> t(n);
        for (int i = 0; i < n; ++i) t[i] = {-efficiency[i], speed[i]};
        sort(t.begin(), t.end());
        priority_queue<int, vector<int>, greater<int>> q;
        long long ans = 0, tot = 0;
        int mod = 1e9 + 7;
        for (auto& x : t) {
            int s = x.second, e = -x.first;
            tot += s;
            ans = max(ans, tot * e);
            q.push(s);
            if (q.size() == k) {
                tot -= q.top();
                q.pop();
            }
        }
        return (int) (ans % mod);
    }
};
```

#### Go

```go
func maxPerformance(n int, speed []int, efficiency []int, k int) int {
	t := make([][]int, n)
	for i, s := range speed {
		t[i] = []int{s, efficiency[i]}
	}
	sort.Slice(t, func(i, j int) bool { return t[i][1] > t[j][1] })
	var mod int = 1e9 + 7
	ans, tot := 0, 0
	pq := hp{}
	for _, x := range t {
		s, e := x[0], x[1]
		tot += s
		ans = max(ans, tot*e)
		heap.Push(&pq, s)
		if pq.Len() == k {
			tot -= heap.Pop(&pq).(int)
		}
	}
	return ans % mod
}

type hp struct{ sort.IntSlice }

func (h *hp) Push(v any) { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
func (h *hp) Less(i, j int) bool { return h.IntSlice[i] < h.IntSlice[j] }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
