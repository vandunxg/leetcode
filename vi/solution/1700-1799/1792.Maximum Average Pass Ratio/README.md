---
comments: true
difficulty: Medium
rating: 1817
source: Weekly Contest 232 Q3
tags:
    - Greedy
    - Array
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1792. Maximum Average Pass Ratio](https://leetcode.com/problems/maximum-average-pass-ratio)

[中文文档](/solution/1700-1799/1792.Maximum%20Average%20Pass%20Ratio/README.md)

## Mô tả

<!-- description:start -->

<p>Một trường học có nhiều lớp và mỗi lớp sẽ thi cuối kỳ. Cho mảng số nguyên 2 chiều <code>classes</code>, trong đó <code>classes[i] = [pass<sub>i</sub>, total<sub>i</sub>]</code>. Biết rằng trong lớp thứ <code>i<sup>th</sup></code> có tổng cộng <code>total<sub>i</sub></code> học sinh, nhưng chỉ có <code>pass<sub>i</sub></code> học sinh sẽ đỗ.</p>

<p>Cho thêm số nguyên <code>extraStudents</code>. Có <code>extraStudents</code> học sinh xuất sắc khác, được <strong>đảm bảo</strong> sẽ đỗ kỳ thi của bất kỳ lớp nào được xếp vào. Hãy phân công mỗi học sinh trong <code>extraStudents</code> vào các lớp sao cho <strong>tối đa hóa</strong> <strong>tỷ lệ đỗ trung bình</strong> của <strong>tất cả</strong> các lớp.</p>

<p><strong>Tỷ lệ đỗ</strong> của một lớp bằng số học sinh đỗ chia cho tổng số học sinh của lớp. <strong>Tỷ lệ đỗ trung bình</strong> là tổng tỷ lệ đỗ của mọi lớp chia cho số lớp.</p>

<p>Trả về <em>tỷ lệ đỗ trung bình <strong>lớn nhất</strong> có thể đạt được sau khi phân công </em><code>extraStudents</code><em> học sinh. </em>Đáp án sai khác không quá <code>10<sup>-5</sup></code> so với kết quả thực sẽ được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> classes = [[1,2],[3,5],[2,2]], <code>extraStudents</code> = 2
<strong>Output:</strong> 0.78333
<strong>Giải thích:</strong> Có thể xếp hai học sinh thêm vào lớp đầu tiên. Tỷ lệ đỗ trung bình sẽ bằng (3/4 + 3/5 + 2/2) / 3 = 0.78333.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> classes = [[2,4],[3,9],[4,5],[2,10]], <code>extraStudents</code> = 4
<strong>Output:</strong> 0.53485
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= classes.length &lt;= 10<sup>5</sup></code></li>
	<li><code>classes[i].length == 2</code></li>
	<li><code>1 &lt;= pass<sub>i</sub> &lt;= total<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= extraStudents &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàng đợi ưu tiên (max-heap của mức tăng)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi học sinh đỗ thêm vào một lớp; ta cần tối đa hóa trung bình các tỷ lệ đỗ. Mức tăng $\frac{a+1}{b+1}-\frac{a}{b}$ giảm khi lớp lớn dần, nên luôn đưa học sinh tiếp theo vào lớp đang có mức tăng lớn nhất.
>
> Dùng heap với khóa là mức tăng (hoặc giá trị đối của nó), lấy ra một lớp, tăng cả hai số đếm rồi đưa lớp trở lại heap. Cuối cùng tính trung bình các tỷ lệ.

<!-- thinking:end -->

Giả sử một lớp hiện có tỷ lệ đỗ $\frac{a}{b}$. Nếu xếp thêm một học sinh giỏi vào lớp, tỷ lệ đỗ sẽ thành $\frac{a+1}{b+1}$. Mức tăng tỷ lệ đỗ là $\frac{a+1}{b+1} - \frac{a}{b}$.

Ta duy trì một max-heap lưu mức tăng tỷ lệ đỗ của từng lớp.

Thực hiện `extraStudents` thao tác. Mỗi lần lấy lớp ở đầu heap, cộng $1$ vào cả số học sinh và số học sinh đỗ của lớp, tính lại mức tăng tỷ lệ đỗ rồi đưa lớp trở lại heap. Lặp lại đến khi phân công hết học sinh.

Cuối cùng, cộng tỷ lệ đỗ của tất cả các lớp rồi chia cho số lớp để được đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lớp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxAverageRatio(self, classes: List[List[int]], extraStudents: int) -> float:
        h = [(a / b - (a + 1) / (b + 1), a, b) for a, b in classes]
        heapify(h)
        for _ in range(extraStudents):
            _, a, b = heappop(h)
            a, b = a + 1, b + 1
            heappush(h, (a / b - (a + 1) / (b + 1), a, b))
        return sum(v[1] / v[2] for v in h) / len(classes)
```

#### Java

```java
class Solution {
    public double maxAverageRatio(int[][] classes, int extraStudents) {
        PriorityQueue<double[]> pq = new PriorityQueue<>((a, b) -> {
            double x = (a[0] + 1) / (a[1] + 1) - a[0] / a[1];
            double y = (b[0] + 1) / (b[1] + 1) - b[0] / b[1];
            return Double.compare(y, x);
        });
        for (var e : classes) {
            pq.offer(new double[] {e[0], e[1]});
        }
        while (extraStudents-- > 0) {
            var e = pq.poll();
            double a = e[0] + 1, b = e[1] + 1;
            pq.offer(new double[] {a, b});
        }
        double ans = 0;
        while (!pq.isEmpty()) {
            var e = pq.poll();
            ans += e[0] / e[1];
        }
        return ans / classes.length;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double maxAverageRatio(vector<vector<int>>& classes, int extraStudents) {
        priority_queue<tuple<double, int, int>> pq;
        for (auto& e : classes) {
            int a = e[0], b = e[1];
            double x = (double) (a + 1) / (b + 1) - (double) a / b;
            pq.push({x, a, b});
        }
        while (extraStudents--) {
            auto [_, a, b] = pq.top();
            pq.pop();
            a++;
            b++;
            double x = (double) (a + 1) / (b + 1) - (double) a / b;
            pq.push({x, a, b});
        }
        double ans = 0;
        while (pq.size()) {
            auto [_, a, b] = pq.top();
            pq.pop();
            ans += (double) a / b;
        }
        return ans / classes.size();
    }
};
```

#### Go

```go
func maxAverageRatio(classes [][]int, extraStudents int) float64 {
	pq := hp{}
	for _, e := range classes {
		a, b := e[0], e[1]
		x := float64(a+1)/float64(b+1) - float64(a)/float64(b)
		heap.Push(&pq, tuple{x, a, b})
	}
	for i := 0; i < extraStudents; i++ {
		e := heap.Pop(&pq).(tuple)
		a, b := e.a+1, e.b+1
		x := float64(a+1)/float64(b+1) - float64(a)/float64(b)
		heap.Push(&pq, tuple{x, a, b})
	}
	var ans float64
	for len(pq) > 0 {
		e := heap.Pop(&pq).(tuple)
		ans += float64(e.a) / float64(e.b)
	}
	return ans / float64(len(classes))
}

type tuple struct {
	x float64
	a int
	b int
}

type hp []tuple

func (h hp) Len() int { return len(h) }
func (h hp) Less(i, j int) bool {
	a, b := h[i], h[j]
	return a.x > b.x
}
func (h hp) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)   { *h = append(*h, v.(tuple)) }
func (h *hp) Pop() any     { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

#### TypeScript

```ts
function maxAverageRatio(classes: number[][], extraStudents: number): number {
    function calcGain(a: number, b: number): number {
        return (a + 1) / (b + 1) - a / b;
    }
    const pq = new PriorityQueue<[number, number]>(
        (p, q) => calcGain(q[0], q[1]) - calcGain(p[0], p[1]),
    );
    for (const [a, b] of classes) {
        pq.enqueue([a, b]);
    }
    while (extraStudents-- > 0) {
        const item = pq.dequeue();
        const [a, b] = item;
        pq.enqueue([a + 1, b + 1]);
    }
    let ans = 0;
    while (!pq.isEmpty()) {
        const item = pq.dequeue()!;
        const [a, b] = item;
        ans += a / b;
    }
    return ans / classes.length;
}
```

#### Rust

```rust
use std::cmp::Ordering;
use std::collections::BinaryHeap;

impl Solution {
    pub fn max_average_ratio(classes: Vec<Vec<i32>>, extra_students: i32) -> f64 {
        struct Node {
            gain: f64,
            a: i32,
            b: i32,
        }

        impl PartialEq for Node {
            fn eq(&self, other: &Self) -> bool {
                self.gain == other.gain
            }
        }
        impl Eq for Node {}

        impl PartialOrd for Node {
            fn partial_cmp(&self, other: &Self) -> Option<Ordering> {
                self.gain.partial_cmp(&other.gain)
            }
        }
        impl Ord for Node {
            fn cmp(&self, other: &Self) -> Ordering {
                self.partial_cmp(other).unwrap()
            }
        }

        fn calc_gain(a: i32, b: i32) -> f64 {
            (a + 1) as f64 / (b + 1) as f64 - a as f64 / b as f64
        }

        let n = classes.len() as f64;
        let mut pq = BinaryHeap::new();

        for c in classes {
            let a = c[0];
            let b = c[1];
            pq.push(Node { gain: calc_gain(a, b), a, b });
        }

        let mut extra = extra_students;
        while extra > 0 {
            if let Some(mut node) = pq.pop() {
                node.a += 1;
                node.b += 1;
                node.gain = calc_gain(node.a, node.b);
                pq.push(node);
            }
            extra -= 1;
        }

        let mut sum = 0.0;
        while let Some(node) = pq.pop() {
            sum += node.a as f64 / node.b as f64;
        }

        sum / n
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
