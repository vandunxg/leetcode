---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [857. Minimum Cost to Hire K Workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers)

[中文文档](/solution/0800-0899/0857.Minimum%20Cost%20to%20Hire%20K%20Workers/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> công nhân. Cho hai mảng số nguyên <code>quality</code> và <code>wage</code>, trong đó <code>quality[i]</code> là chất lượng của công nhân thứ <code>i<sup>th</sup></code>, còn <code>wage[i]</code> là mức lương tối thiểu mong muốn của người đó.</p>

<p>Ta muốn thuê chính xác <code>k</code> công nhân để tạo thành một <strong>nhóm được trả lương</strong>. Nhóm này phải được trả lương theo các quy tắc sau:</p>

<ol>
	<li>Mỗi công nhân trong nhóm phải được trả ít nhất mức lương tối thiểu họ mong muốn.</li>
	<li>Mức lương của mỗi công nhân trong nhóm phải tỷ lệ thuận với chất lượng của họ. Nghĩa là nếu chất lượng của một người gấp đôi người khác trong nhóm thì mức lương của họ cũng phải gấp đôi.</li>
</ol>

<p>Cho số nguyên <code>k</code>, hãy trả về <em>chi phí thấp nhất để lập một nhóm được trả lương thỏa mãn các điều kiện trên</em>. Đáp án được chấp nhận nếu sai lệch không quá <code>10<sup>-5</sup></code> so với kết quả thực tế.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> quality = [10,20,5], wage = [70,50,30], k = 2
<strong>Đầu ra:</strong> 105.00000
<strong>Giải thích:</strong> Ta trả 70 cho công nhân thứ 0 và 35 cho công nhân thứ 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> quality = [3,1,10,10,1], wage = [4,8,2,2,7], k = 3
<strong>Đầu ra:</strong> 30.66667
<strong>Giải thích:</strong> Ta trả 4 cho công nhân thứ 0 và trả riêng 13.33333 cho mỗi công nhân thứ 2 và thứ 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == quality.length == wage.length</code></li>
	<li><code>1 &lt;= k &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= quality[i], wage[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mọi người được trả lương theo cùng một tỷ lệ, đủ để mỗi người nhận ít nhất mức lương mong muốn; ta chọn $k$ công nhân sao cho tổng lương thấp nhất. Tỷ lệ này là giá trị lớn nhất của $\textit{wage}/\textit{quality}$ trong nhóm. Với $n\le 10^4$, không thể xét mọi tập con.
>
> Thêm công nhân theo thứ tự tỷ lệ tăng dần để tỷ lệ hiện tại đáp ứng cả nhóm. Dùng max-heap để loại công nhân có chất lượng lớn nhất nhằm giữ kích thước nhóm bằng $k$. Mỗi khi nhóm có đủ $k$ người, cập nhật chi phí bằng tỷ lệ hiện tại nhân tổng chất lượng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mincostToHireWorkers(
        self, quality: List[int], wage: List[int], k: int
    ) -> float:
        t = sorted(zip(quality, wage), key=lambda x: x[1] / x[0])
        ans, tot = inf, 0
        h = []
        for q, w in t:
            tot += q
            heappush(h, -q)
            if len(h) == k:
                ans = min(ans, w / q * tot)
                tot += heappop(h)
        return ans
```

#### Java

```java
class Solution {
    public double mincostToHireWorkers(int[] quality, int[] wage, int k) {
        int n = quality.length;
        Pair<Double, Integer>[] t = new Pair[n];
        for (int i = 0; i < n; ++i) {
            t[i] = new Pair<>((double) wage[i] / quality[i], quality[i]);
        }
        Arrays.sort(t, (a, b) -> Double.compare(a.getKey(), b.getKey()));
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);
        double ans = 1e18;
        int tot = 0;
        for (var e : t) {
            tot += e.getValue();
            pq.offer(e.getValue());
            if (pq.size() == k) {
                ans = Math.min(ans, tot * e.getKey());
                tot -= pq.poll();
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
    double mincostToHireWorkers(vector<int>& quality, vector<int>& wage, int k) {
        int n = quality.size();
        vector<pair<double, int>> t(n);
        for (int i = 0; i < n; ++i) {
            t[i] = {(double) wage[i] / quality[i], quality[i]};
        }
        sort(t.begin(), t.end());
        priority_queue<int> pq;
        double ans = 1e18;
        int tot = 0;
        for (auto& [x, q] : t) {
            tot += q;
            pq.push(q);
            if (pq.size() == k) {
                ans = min(ans, tot * x);
                tot -= pq.top();
                pq.pop();
            }
        }
        return ans;
    }
};
```

#### Go

```go
func mincostToHireWorkers(quality []int, wage []int, k int) float64 {
	t := []pair{}
	for i, q := range quality {
		t = append(t, pair{float64(wage[i]) / float64(q), q})
	}
	sort.Slice(t, func(i, j int) bool { return t[i].x < t[j].x })
	tot := 0
	var ans float64 = 1e18
	pq := hp{}
	for _, e := range t {
		tot += e.q
		heap.Push(&pq, e.q)
		if pq.Len() == k {
			ans = min(ans, float64(tot)*e.x)
			tot -= heap.Pop(&pq).(int)
		}
	}
	return ans
}

type pair struct {
	x float64
	q int
}

type hp struct{ sort.IntSlice }

func (h *hp) Push(v any) { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
func (h *hp) Less(i, j int) bool { return h.IntSlice[i] > h.IntSlice[j] }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
