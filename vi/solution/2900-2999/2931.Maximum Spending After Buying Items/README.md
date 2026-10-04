---
comments: true
difficulty: Hard
rating: 1822
source: Biweekly Contest 117 Q4
tags:
    - Greedy
    - Array
    - Matrix
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2931. Maximum Spending After Buying Items](https://leetcode.com/problems/maximum-spending-after-buying-items)

[Tài liệu tiếng Trung](/solution/2900-2999/2931.Maximum%20Spending%20After%20Buying%20Items/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận số nguyên <strong>0-indexed</strong> kích thước <code>m * n</code> là <code>values</code>, biểu diễn giá trị của <code>m * n</code> sản phẩm khác nhau trong <code>m</code> cửa hàng khác nhau. Mỗi cửa hàng có <code>n</code> sản phẩm, trong đó sản phẩm thứ <code>j<sup>th</sup></code> trong cửa hàng thứ <code>i<sup>th</sup></code> có giá trị là <code>values[i][j]</code>. Ngoài ra, các sản phẩm trong cửa hàng thứ <code>i<sup>th</sup></code> được sắp xếp theo thứ tự không tăng của giá trị. Nghĩa là, <code>values[i][j] &gt;= values[i][j + 1]</code> với mọi <code>0 &lt;= j &lt; n - 1</code>.</p>

<p>Mỗi ngày, bạn muốn mua một sản phẩm từ một trong các cửa hàng. Cụ thể, vào ngày thứ <code>d<sup>th</sup></code>, bạn có thể:</p>

<ul>
	<li>Chọn một cửa hàng bất kỳ <code>i</code>.</li>
	<li>Mua sản phẩm còn lại ngoài cùng bên phải <code>j</code> với giá <code>values[i][j] * d</code>. Nghĩa là, tìm chỉ số lớn nhất <code>j</code> sao cho sản phẩm <code>j</code> chưa từng được mua, rồi mua sản phẩm đó với giá <code>values[i][j] * d</code>.</li>
</ul>

<p><strong>Lưu ý</strong> rằng mọi sản phẩm đều đôi một khác nhau. Ví dụ, nếu bạn đã mua sản phẩm <code>0</code> từ cửa hàng <code>1</code>, bạn vẫn có thể mua sản phẩm <code>0</code> từ bất kỳ cửa hàng nào khác.</p>

<p>Trả về <em><strong>số tiền tối đa có thể chi</strong> để mua toàn bộ </em><code>m * n</code><em> sản phẩm</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> values = [[8,5,2],[6,4,1],[9,7,3]]
<strong>Đầu ra:</strong> 285
<strong>Giải thích:</strong> Vào ngày đầu tiên, ta mua sản phẩm 2 từ cửa hàng 1 với giá values[1][2] * 1 = 1.
Vào ngày thứ hai, ta mua sản phẩm 2 từ cửa hàng 0 với giá values[0][2] * 2 = 4.
Vào ngày thứ ba, ta mua sản phẩm 2 từ cửa hàng 2 với giá values[2][2] * 3 = 9.
Vào ngày thứ tư, ta mua sản phẩm 1 từ cửa hàng 1 với giá values[1][1] * 4 = 16.
Vào ngày thứ năm, ta mua sản phẩm 1 từ cửa hàng 0 với giá values[0][1] * 5 = 25.
Vào ngày thứ sáu, ta mua sản phẩm 0 từ cửa hàng 1 với giá values[1][0] * 6 = 36.
Vào ngày thứ bảy, ta mua sản phẩm 1 từ cửa hàng 2 với giá values[2][1] * 7 = 49.
Vào ngày thứ tám, ta mua sản phẩm 0 từ cửa hàng 0 với giá values[0][0] * 8 = 64.
Vào ngày thứ chín, ta mua sản phẩm 0 từ cửa hàng 2 với giá values[2][0] * 9 = 81.
Do đó, tổng số tiền đã chi là 285.
Có thể chứng minh rằng 285 là số tiền tối đa có thể chi khi mua toàn bộ m * n sản phẩm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> values = [[10,8,6,4,2],[9,7,5,3,2]]
<strong>Đầu ra:</strong> 386
<strong>Giải thích:</strong> Vào ngày đầu tiên, ta mua sản phẩm 4 từ cửa hàng 0 với giá values[0][4] * 1 = 2.
Vào ngày thứ hai, ta mua sản phẩm 4 từ cửa hàng 1 với giá values[1][4] * 2 = 4.
Vào ngày thứ ba, ta mua sản phẩm 3 từ cửa hàng 1 với giá values[1][3] * 3 = 9.
Vào ngày thứ tư, ta mua sản phẩm 3 từ cửa hàng 0 với giá values[0][3] * 4 = 16.
Vào ngày thứ năm, ta mua sản phẩm 2 từ cửa hàng 1 với giá values[1][2] * 5 = 25.
Vào ngày thứ sáu, ta mua sản phẩm 2 từ cửa hàng 0 với giá values[0][2] * 6 = 36.
Vào ngày thứ bảy, ta mua sản phẩm 1 từ cửa hàng 1 với giá values[1][1] * 7 = 49.
Vào ngày thứ tám, ta mua sản phẩm 1 từ cửa hàng 0 với giá values[0][1] * 8 = 64
Vào ngày thứ chín, ta mua sản phẩm 0 từ cửa hàng 1 với giá values[1][0] * 9 = 81.
Vào ngày thứ mười, ta mua sản phẩm 0 từ cửa hàng 0 với giá values[0][0] * 10 = 100.
Do đó, tổng số tiền đã chi là 386.
Có thể chứng minh rằng 386 là số tiền tối đa có thể chi khi mua toàn bộ m * n sản phẩm.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m == values.length &lt;= 10</code></li>
	<li><code>1 &lt;= n == values[i].length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= values[i][j] &lt;= 10<sup>6</sup></code></li>
	<li><code>values[i]</code> được sắp xếp theo thứ tự không tăng.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Priority Queue

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ngày mua một sản phẩm với giá trị nhân với chỉ số ngày, và mỗi cửa hàng được lấy sản phẩm từ phải sang trái. Vì các ngày lớn hơn tạo ra hệ số nhân lớn hơn, ta nên để các giá trị lớn lại mua sau: mỗi ngày lấy sản phẩm ngoài cùng bên phải hiện tại có giá trị nhỏ nhất trong các cửa hàng.
>
> Các đầu phải đó tạo thành một min-heap. Lấy phần tử nhỏ nhất, cộng $d \cdot v$, rồi thêm sản phẩm ngay trước đó của cửa hàng tương ứng. Có tổng cộng $m \cdot n$ thao tác trên heap với $m \le 10$ và $n \le 10^4$.

<!-- thinking:end -->

Theo mô tả bài toán, để tối đa hóa tổng chi phí, ta nên ưu tiên mua các sản phẩm có giá trị nhỏ hơn và để các sản phẩm có giá trị lớn hơn mua sau. Vì vậy, ta sử dụng priority queue (min-heap) để lưu sản phẩm có giá trị nhỏ nhất chưa được mua ở mỗi cửa hàng. Ban đầu, ta thêm sản phẩm ngoài cùng bên phải của mỗi cửa hàng vào priority queue.

Mỗi ngày, ta lấy sản phẩm có giá trị nhỏ nhất ra khỏi priority queue, cộng giá trị của nó vào đáp án, rồi thêm sản phẩm ngay trước nó trong cửa hàng tương ứng vào priority queue. Ta lặp lại thao tác trên cho đến khi priority queue rỗng.

Độ phức tạp thời gian là $O(m \times n \times \log m)$, độ phức tạp không gian là $O(m)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của mảng $values$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSpending(self, values: List[List[int]]) -> int:
        n = len(values[0])
        pq = [(row[-1], i, n - 1) for i, row in enumerate(values)]
        heapify(pq)
        ans = d = 0
        while pq:
            d += 1
            v, i, j = heappop(pq)
            ans += v * d
            if j:
                heappush(pq, (values[i][j - 1], i, j - 1))
        return ans
```

#### Java

```java
class Solution {
    public long maxSpending(int[][] values) {
        int m = values.length, n = values[0].length;
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        for (int i = 0; i < m; ++i) {
            pq.offer(new int[] {values[i][n - 1], i, n - 1});
        }
        long ans = 0;
        for (int d = 1; !pq.isEmpty(); ++d) {
            var p = pq.poll();
            int v = p[0], i = p[1], j = p[2];
            ans += (long) v * d;
            if (j > 0) {
                pq.offer(new int[] {values[i][j - 1], i, j - 1});
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
    long long maxSpending(vector<vector<int>>& values) {
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<tuple<int, int, int>>> pq;
        int m = values.size(), n = values[0].size();
        for (int i = 0; i < m; ++i) {
            pq.emplace(values[i][n - 1], i, n - 1);
        }
        long long ans = 0;
        for (int d = 1; pq.size(); ++d) {
            auto [v, i, j] = pq.top();
            pq.pop();
            ans += 1LL * v * d;
            if (j) {
                pq.emplace(values[i][j - 1], i, j - 1);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxSpending(values [][]int) (ans int64) {
	pq := hp{}
	n := len(values[0])
	for i, row := range values {
		heap.Push(&pq, tuple{row[n-1], i, n - 1})
	}
	for d := 1; len(pq) > 0; d++ {
		p := heap.Pop(&pq).(tuple)
		ans += int64(p.v * d)
		if p.j > 0 {
			heap.Push(&pq, tuple{values[p.i][p.j-1], p.i, p.j - 1})
		}
	}
	return
}

type tuple struct{ v, i, j int }
type hp []tuple

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].v < h[j].v }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)        { *h = append(*h, v.(tuple)) }
func (h *hp) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

#### TypeScript

```ts
function maxSpending(values: number[][]): number {
    const m = values.length;
    const n = values[0].length;
    const pq = new PriorityQueue({ compare: (a, b) => a[0] - b[0] });
    for (let i = 0; i < m; ++i) {
        pq.enqueue([values[i][n - 1], i, n - 1]);
    }

    let ans = 0;
    for (let d = 1; !pq.isEmpty(); ++d) {
        const [v, i, j] = pq.dequeue()!;
        ans += v * d;
        if (j > 0) {
            pq.enqueue([values[i][j - 1], i, j - 1]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
