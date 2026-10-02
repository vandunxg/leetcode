---
comments: true
difficulty: Hard
rating: 2610
source: Biweekly Contest 41 Q4
tags:
    - Segment Tree
    - Queue
    - Array
    - Dynamic Programming
    - Prefix Sum
    - Monotonic Queue
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1687. Delivering Boxes from Storage to Ports](https://leetcode.com/problems/delivering-boxes-from-storage-to-ports)

[中文文档](/solution/1600-1699/1687.Delivering%20Boxes%20from%20Storage%20to%20Ports/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn cần giao một số thùng hàng từ kho đến các cảng bằng một con tàu. Tuy nhiên, tàu có <strong>giới hạn</strong> về <strong>số lượng thùng</strong> và <strong>tổng khối lượng</strong> có thể chở.</p>

<p>Cho mảng <code>boxes</code>, trong đó <code>boxes[i] = [ports<sub>​​i</sub>​, weight<sub>i</sub>]</code>, cùng ba số nguyên <code>portsCount</code>, <code>maxBoxes</code> và <code>maxWeight</code>.</p>

<ul>
	<li><code>ports<sub>​​i</sub></code> là cảng cần giao thùng thứ <code>i<sup>th</sup></code>, còn <code>weights<sub>i</sub></code> là khối lượng của thùng thứ <code>i<sup>th</sup></code>.</li>
	<li><code>portsCount</code> là số lượng cảng.</li>
	<li><code>maxBoxes</code> và <code>maxWeight</code> lần lượt là giới hạn số thùng và khối lượng của tàu.</li>
</ul>

<p>Các thùng phải được giao <strong>theo đúng thứ tự đã cho</strong>. Tàu thực hiện các bước sau:</p>

<ul>
	<li>Tàu lấy một số thùng từ queue <code>boxes</code>, không vượt quá các ràng buộc <code>maxBoxes</code> và <code>maxWeight</code>.</li>
	<li>Với mỗi thùng đã chở, <strong>theo thứ tự</strong>, tàu thực hiện một <strong>chuyến</strong> đến cảng cần giao và giao thùng. Nếu tàu đã ở đúng cảng thì không cần <strong>chuyến</strong> nào, thùng được giao ngay.</li>
	<li>Sau đó tàu thực hiện một <strong>chuyến</strong> quay về kho để lấy thêm thùng từ queue.</li>
</ul>

<p>Sau khi giao hết các thùng, tàu phải kết thúc tại kho.</p>

<p>Hãy trả về <em><strong>số chuyến</strong> <strong>nhỏ nhất</strong> mà tàu cần thực hiện để giao tất cả thùng đến các cảng tương ứng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> boxes = [[1,1],[2,1],[1,1]], portsCount = 2, maxBoxes = 3, maxWeight = 3
<strong>Output:</strong> 4
<strong>Giải thích:</strong> Chiến lược tối ưu như sau:
- Tàu lấy tất cả thùng trong queue, đi đến cảng 1, rồi cảng 2, rồi lại cảng 1, sau đó quay về kho. Tổng cộng 4 chuyến.
Vì vậy tổng số chuyến là 4.
Lưu ý rằng thùng thứ nhất và thứ ba không thể giao cùng nhau vì các thùng phải được giao theo thứ tự (tức là thùng thứ hai phải được giao ở cảng 2 trước thùng thứ ba).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> boxes = [[1,2],[3,3],[3,1],[3,1],[2,4]], portsCount = 3, maxBoxes = 3, maxWeight = 6
<strong>Output:</strong> 6
<strong>Giải thích:</strong> Chiến lược tối ưu như sau:
- Tàu lấy thùng đầu tiên, đi đến cảng 1 rồi quay về kho. 2 chuyến.
- Tàu lấy các thùng thứ hai, thứ ba và thứ tư, đi đến cảng 3 rồi quay về kho. 2 chuyến.
- Tàu lấy thùng thứ năm, đi đến cảng 2 rồi quay về kho. 2 chuyến.
Vì vậy tổng số chuyến là 2 + 2 + 2 = 6.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> boxes = [[1,4],[1,2],[2,1],[2,1],[3,2],[3,4]], portsCount = 3, maxBoxes = 6, maxWeight = 7
<strong>Output:</strong> 6
<strong>Giải thích:</strong> Chiến lược tối ưu như sau:
- Tàu lấy thùng thứ nhất và thứ hai, đi đến cảng 1 rồi quay về kho. 2 chuyến.
- Tàu lấy thùng thứ ba và thứ tư, đi đến cảng 2 rồi quay về kho. 2 chuyến.
- Tàu lấy thùng thứ năm và thứ sáu, đi đến cảng 3 rồi quay về kho. 2 chuyến.
Vì vậy tổng số chuyến là 2 + 2 + 2 = 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= boxes.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= portsCount, maxBoxes, maxWeight &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= ports<sub>​​i</sub> &lt;= portsCount</code></li>
	<li><code>1 &lt;= weights<sub>i</sub> &lt;= maxWeight</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Các thùng phải được lấy theo các đoạn liên tiếp, bị giới hạn bởi số lượng và khối lượng. Một chuyến gồm lượt đi-về kho cùng các lần di chuyển giữa các cảng kề nhau khác nhau. $f[i]$ là số chuyến ít nhất để giao xong $i$ thùng, bằng cách duyệt điểm cắt trước đó $j$.
>
> Số lần chuyển cảng là một đoạn của prefix $cs$ cộng thêm $2$ cho lượt đi-về. Với $n=10^5$, chuyển trạng thái $O(n^2)$ này chỉ là lời giải cơ sở đúng nhưng quá chậm.

<!-- thinking:end -->

Đặt $f[i]$ là số chuyến ít nhất cần dùng để vận chuyển $i$ thùng đầu tiên từ kho đến các cảng tương ứng, nên đáp án là $f[n]$.

Các thùng phải được vận chuyển theo thứ tự trong mảng. Mỗi lần, tàu lấy một số thùng liên tiếp theo thứ tự rồi giao từng thùng đến cảng tương ứng. Sau khi giao xong, tàu quay về kho.

Vì vậy, ta có thể duyệt chỉ số $j$ của thùng cuối cùng được vận chuyển trước chuyến cuối. Khi đó $f[i]$ có thể chuyển từ $f[j]$. Khi chuyển trạng thái, cần xét:

- Khi chuyển từ $f[j]$, số thùng trên tàu không được vượt quá $maxBoxes$.
- Khi chuyển từ $f[j]$, tổng khối lượng các thùng trên tàu không được vượt quá $maxWeight$.

Phương trình chuyển trạng thái là:

$$
f[i] = \min_{j \in [i - maxBoxes, i - 1]} \left(f[j] + \sum_{k = j + 1}^i \textit{cost}(k)\right)
$$

Trong đó, $\sum_{k = j + 1}^i \textit{cost}(k)$ là số chuyến cần để giao các thùng trong $[j+1,..i]$ đến cảng tương ứng trong một chuyến. Phần này có thể tính nhanh bằng prefix sum.

Ví dụ, giả sử ta lấy các thùng $1, 2, 3$ và cần giao chúng đến các cảng $4, 4, 5$. Trước tiên tàu đi từ kho đến cảng $4$, rồi từ cảng $4$ đến cảng $5$, cuối cùng từ cảng $5$ quay về kho. Như vậy cần $2$ chuyến cho lượt đi từ kho đến cảng và lượt về từ cảng về kho. Số lần đi giữa các cảng phụ thuộc vào việc hai cảng kề nhau có giống nhau hay không. Nếu khác nhau, số chuyến tăng $1$, nếu giống nhau thì không đổi. Vì vậy, ta có thể dùng prefix sum để tính số chuyến giữa các cảng, rồi cộng thêm hai chuyến đầu và cuối để tính số chuyến giao các thùng trong $[j+1,..i]$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số thùng. Với các ràng buộc đã cho, cách này vượt quá giới hạn thời gian.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def boxDelivering(
        self, boxes: List[List[int]], portsCount: int, maxBoxes: int, maxWeight: int
    ) -> int:
        n = len(boxes)
        ws = list(accumulate((box[1] for box in boxes), initial=0))
        c = [int(a != b) for a, b in pairwise(box[0] for box in boxes)]
        cs = list(accumulate(c, initial=0))
        f = [inf] * (n + 1)
        f[0] = 0
        for i in range(1, n + 1):
            for j in range(max(0, i - maxBoxes), i):
                if ws[i] - ws[j] <= maxWeight:
                    f[i] = min(f[i], f[j] + cs[i - 1] - cs[j] + 2)
        return f[n]
```

#### Java

```java
class Solution {
    public int boxDelivering(int[][] boxes, int portsCount, int maxBoxes, int maxWeight) {
        int n = boxes.length;
        long[] ws = new long[n + 1];
        int[] cs = new int[n];
        for (int i = 0; i < n; ++i) {
            int p = boxes[i][0], w = boxes[i][1];
            ws[i + 1] = ws[i] + w;
            if (i < n - 1) {
                cs[i + 1] = cs[i] + (p != boxes[i + 1][0] ? 1 : 0);
            }
        }
        int[] f = new int[n + 1];
        Arrays.fill(f, 1 << 30);
        f[0] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = Math.max(0, i - maxBoxes); j < i; ++j) {
                if (ws[i] - ws[j] <= maxWeight) {
                    f[i] = Math.min(f[i], f[j] + cs[i - 1] - cs[j] + 2);
                }
            }
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int boxDelivering(vector<vector<int>>& boxes, int portsCount, int maxBoxes, int maxWeight) {
        int n = boxes.size();
        long ws[n + 1];
        int cs[n];
        ws[0] = cs[0] = 0;
        for (int i = 0; i < n; ++i) {
            int p = boxes[i][0], w = boxes[i][1];
            ws[i + 1] = ws[i] + w;
            if (i < n - 1) cs[i + 1] = cs[i] + (p != boxes[i + 1][0]);
        }
        int f[n + 1];
        memset(f, 0x3f, sizeof f);
        f[0] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = max(0, i - maxBoxes); j < i; ++j) {
                if (ws[i] - ws[j] <= maxWeight) {
                    f[i] = min(f[i], f[j] + cs[i - 1] - cs[j] + 2);
                }
            }
        }
        return f[n];
    }
};
```

#### Go

```go
func boxDelivering(boxes [][]int, portsCount int, maxBoxes int, maxWeight int) int {
	n := len(boxes)
	ws := make([]int, n+1)
	cs := make([]int, n)
	for i, box := range boxes {
		p, w := box[0], box[1]
		ws[i+1] = ws[i] + w
		if i < n-1 {
			t := 0
			if p != boxes[i+1][0] {
				t++
			}
			cs[i+1] = cs[i] + t
		}
	}
	f := make([]int, n+1)
	for i := 1; i <= n; i++ {
		f[i] = 1 << 30
		for j := max(0, i-maxBoxes); j < i; j++ {
			if ws[i]-ws[j] <= maxWeight {
				f[i] = min(f[i], f[j]+cs[i-1]-cs[j]+2)
			}
		}
	}
	return f[n]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động + Tối ưu bằng monotonic queue

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tìm giá trị nhỏ nhất của $f[j]-cs[j]$ trong $[i-\textit{maxBoxes},i)$ dưới ràng buộc prefix khối lượng. Monotonic queue lưu các ứng viên $j$, giúp mỗi lần chuyển có chi phí trung bình $O(1)$.
>
> Loại phần tử đầu khi vi phạm giới hạn số lượng hoặc khối lượng, đồng thời giữ $f-cs$ tăng dần ở phía sau, nên tổng thời gian là $O(n)$.

<!-- thinking:end -->

Kích thước dữ liệu của bài toán có thể đạt $10^5$, còn Lời giải 1 có độ phức tạp $O(n^2)$ nên vượt quá giới hạn thời gian. Quan sát kỹ:

$$
f[i] = \min(f[i], f[j] + cs[i - 1] - cs[j] + 2)
$$

Thực tế, ta cần tìm $j$ trong cửa sổ $[i-maxBoxes,..i-1]$ sao cho $f[j] - cs[j]$ nhỏ nhất. Để tìm giá trị nhỏ nhất trong cửa sổ trượt, ta có thể dùng monotonic queue, giúp lấy giá trị nhỏ nhất thỏa điều kiện trong $O(1)$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số thùng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def boxDelivering(
        self, boxes: List[List[int]], portsCount: int, maxBoxes: int, maxWeight: int
    ) -> int:
        n = len(boxes)
        ws = list(accumulate((box[1] for box in boxes), initial=0))
        c = [int(a != b) for a, b in pairwise(box[0] for box in boxes)]
        cs = list(accumulate(c, initial=0))
        f = [0] * (n + 1)
        q = deque([0])
        for i in range(1, n + 1):
            while q and (i - q[0] > maxBoxes or ws[i] - ws[q[0]] > maxWeight):
                q.popleft()
            if q:
                f[i] = cs[i - 1] + f[q[0]] - cs[q[0]] + 2
            if i < n:
                while q and f[q[-1]] - cs[q[-1]] >= f[i] - cs[i]:
                    q.pop()
                q.append(i)
        return f[n]
```

#### Java

```java
class Solution {
    public int boxDelivering(int[][] boxes, int portsCount, int maxBoxes, int maxWeight) {
        int n = boxes.length;
        long[] ws = new long[n + 1];
        int[] cs = new int[n];
        for (int i = 0; i < n; ++i) {
            int p = boxes[i][0], w = boxes[i][1];
            ws[i + 1] = ws[i] + w;
            if (i < n - 1) {
                cs[i + 1] = cs[i] + (p != boxes[i + 1][0] ? 1 : 0);
            }
        }
        int[] f = new int[n + 1];
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(0);
        for (int i = 1; i <= n; ++i) {
            while (!q.isEmpty()
                && (i - q.peekFirst() > maxBoxes || ws[i] - ws[q.peekFirst()] > maxWeight)) {
                q.pollFirst();
            }
            if (!q.isEmpty()) {
                f[i] = cs[i - 1] + f[q.peekFirst()] - cs[q.peekFirst()] + 2;
            }
            if (i < n) {
                while (!q.isEmpty() && f[q.peekLast()] - cs[q.peekLast()] >= f[i] - cs[i]) {
                    q.pollLast();
                }
                q.offer(i);
            }
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int boxDelivering(vector<vector<int>>& boxes, int portsCount, int maxBoxes, int maxWeight) {
        int n = boxes.size();
        long ws[n + 1];
        int f[n + 1];
        int cs[n];
        ws[0] = cs[0] = f[0] = 0;
        for (int i = 0; i < n; ++i) {
            int p = boxes[i][0], w = boxes[i][1];
            ws[i + 1] = ws[i] + w;
            if (i < n - 1) cs[i + 1] = cs[i] + (p != boxes[i + 1][0]);
        }
        deque<int> q{{0}};
        for (int i = 1; i <= n; ++i) {
            while (!q.empty() && (i - q.front() > maxBoxes || ws[i] - ws[q.front()] > maxWeight)) q.pop_front();
            if (!q.empty()) f[i] = cs[i - 1] + f[q.front()] - cs[q.front()] + 2;
            if (i < n) {
                while (!q.empty() && f[q.back()] - cs[q.back()] >= f[i] - cs[i]) q.pop_back();
                q.push_back(i);
            }
        }
        return f[n];
    }
};
```

#### Go

```go
func boxDelivering(boxes [][]int, portsCount int, maxBoxes int, maxWeight int) int {
	n := len(boxes)
	ws := make([]int, n+1)
	cs := make([]int, n)
	for i, box := range boxes {
		p, w := box[0], box[1]
		ws[i+1] = ws[i] + w
		if i < n-1 {
			t := 0
			if p != boxes[i+1][0] {
				t++
			}
			cs[i+1] = cs[i] + t
		}
	}
	f := make([]int, n+1)
	q := []int{0}
	for i := 1; i <= n; i++ {
		for len(q) > 0 && (i-q[0] > maxBoxes || ws[i]-ws[q[0]] > maxWeight) {
			q = q[1:]
		}
		if len(q) > 0 {
			f[i] = cs[i-1] + f[q[0]] - cs[q[0]] + 2
		}
		if i < n {
			for len(q) > 0 && f[q[len(q)-1]]-cs[q[len(q)-1]] >= f[i]-cs[i] {
				q = q[:len(q)-1]
			}
			q = append(q, i)
		}
	}
	return f[n]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
