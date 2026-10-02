---
comments: true
difficulty: Hard
rating: 2014
source: Weekly Contest 176 Q4
tags:
    - Array
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1354. Construct Target Array With Multiple Sums](https://leetcode.com/problems/construct-target-array-with-multiple-sums)

[中文文档](/solution/1300-1399/1354.Construct%20Target%20Array%20With%20Multiple%20Sums/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>target</code> gồm n số nguyên. Bắt đầu với mảng <code>arr</code> gồm <code>n</code> số 1, bạn có thể thực hiện quy trình sau:</p>

<ul>
	<li>Gọi <code>x</code> là tổng tất cả phần tử hiện tại trong mảng.</li>
	<li>Chọn chỉ số <code>i</code> sao cho <code>0 &lt;= i &lt; n</code>, rồi gán giá trị tại chỉ số <code>i</code> của <code>arr</code> bằng <code>x</code>.</li>
	<li>Bạn có thể lặp lại quy trình này tùy ý số lần.</li>
</ul>

<p>Trả về <code>true</code> <em>nếu có thể tạo mảng</em> <code>target</code> <em>từ</em> <code>arr</code><em>; nếu không, trả về</em> <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = [9,3,5]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Bắt đầu với arr = [1, 1, 1] 
[1, 1, 1], tổng = 3, chọn chỉ số 1
[1, 3, 1], tổng = 5, chọn chỉ số 2
[1, 3, 5], tổng = 9, chọn chỉ số 0
[9, 3, 5] Hoàn tất
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = [1,1,1,2]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể tạo mảng target từ [1,1,1,1].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = [8,5]
<strong>Đầu ra:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == target.length</code></li>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= target[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dựng ngược + priority queue (max heap)

<!-- thinking:start -->

> **Tư duy**
>
> Bắt đầu từ mảng toàn số 1, mỗi bước thay một phần tử bằng tổng mảng; ta cần xác định có thể đạt được $\textit{target}$ hay không. Khi dựng xuôi, không biết nên tăng chỉ số nào, trong khi giá trị có thể lên tới $10^9$. Khi dựng ngược, phần tử lớn nhất hiện tại hẳn là phần tử vừa được ghi: trước đó nó bằng tổng các phần tử còn lại $t$, nên giá trị trước đó là $mx \bmod t$ (giúp bỏ qua nhiều bước giảm liên tiếp). Dùng max heap lặp lại thao tác này cho đến khi mọi phần tử đều bằng 1; nếu $t=0$ hoặc $mx-t<1$ thì không thể tạo được mảng.

<!-- thinking:end -->

Ta thấy nếu dựng mảng đích $\textit{target}$ từ mảng $\textit{arr}$ theo chiều xuôi, mỗi lần rất khó xác định nên chọn chỉ số $i$ nào, khiến bài toán phức tạp. Ngược lại, khi dựng ngược từ $\textit{target}$, ở mỗi bước ta phải chọn phần tử lớn nhất hiện tại; lựa chọn này là duy nhất nên bài toán đơn giản hơn.

Vì vậy, ta dùng priority queue (max heap) để lưu các phần tử của mảng $\textit{target}$, đồng thời dùng biến $s$ lưu tổng của chúng. Mỗi lần lấy phần tử lớn nhất $mx$ khỏi priority queue và tính tổng $t$ của các phần tử còn lại trong mảng hiện tại. Nếu $t \lt 1$ hoặc $mx - t \lt 1$, không thể tạo được mảng $\textit{target}$ nên trả về `false`. Nếu không, ta tính $mx \bmod t$. Nếu $mx \bmod t = 0$ thì đặt $x = t$; ngược lại, đặt $x = mx \bmod t$. Đưa $x$ vào priority queue và cập nhật $s$. Lặp lại đến khi mọi phần tử trong priority queue đều bằng $1$, khi đó trả về `true`.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài của mảng $\textit{target}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPossible(self, target: List[int]) -> bool:
        s = sum(target)
        pq = [-x for x in target]
        heapify(pq)
        while -pq[0] > 1:
            mx = -heappop(pq)
            t = s - mx
            if t == 0 or mx - t < 1:
                return False
            x = (mx % t) or t
            heappush(pq, -x)
            s = s - mx + x
        return True
```

#### Java

```java
class Solution {
    public boolean isPossible(int[] target) {
        PriorityQueue<Long> pq = new PriorityQueue<>(Collections.reverseOrder());
        long s = 0;
        for (int x : target) {
            s += x;
            pq.offer((long) x);
        }
        while (pq.peek() > 1) {
            long mx = pq.poll();
            long t = s - mx;
            if (t == 0 || mx - t < 1) {
                return false;
            }
            long x = mx % t;
            if (x == 0) {
                x = t;
            }
            pq.offer(x);
            s = s - mx + x;
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isPossible(vector<int>& target) {
        priority_queue<int> pq;
        long long s = 0;
        for (int i = 0; i < target.size(); i++) {
            s += target[i];
            pq.push(target[i]);
        }
        while (pq.top() != 1) {
            int mx = pq.top();
            pq.pop();
            long long t = s - mx;
            if (t < 1 || mx - t < 1) {
                return false;
            }
            int x = mx % t;
            if (x == 0) {
                x = t;
            }
            pq.push(x);
            s = s - mx + x;
        }
        return true;
    }
};
```

#### Go

```go
func isPossible(target []int) bool {
	pq := &hp{target}
	s := 0
	for _, x := range target {
		s += x
	}
	heap.Init(pq)
	for target[0] > 1 {
		mx := target[0]
		t := s - mx
		if t < 1 || mx-t < 1 {
			return false
		}
		x := mx % t
		if x == 0 {
			x = t
		}
		target[0] = x
		heap.Fix(pq, 0)
		s = s - mx + x
	}
	return true
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] > h.IntSlice[j] }
func (hp) Pop() (_ any)         { return }
func (hp) Push(any)             {}
```

#### TypeScript

```ts
function isPossible(target: number[]): boolean {
    const pq = new MaxPriorityQueue<number>();
    let s = 0;
    for (const x of target) {
        s += x;
        pq.enqueue(x);
    }
    while (pq.front() > 1) {
        const mx = pq.dequeue();
        const t = s - mx;
        if (t < 1 || mx - t < 1) {
            return false;
        }
        const x = mx % t || t;
        pq.enqueue(x);
        s = s - mx + x;
    }
    return true;
}
```

#### Rust

```rust
use std::collections::BinaryHeap;

impl Solution {
    pub fn is_possible(target: Vec<i32>) -> bool {
        let mut pq = BinaryHeap::from(target.clone());
        let mut s: i64 = target.iter().map(|&x| x as i64).sum();

        while let Some(&mx) = pq.peek() {
            if mx == 1 {
                break;
            }
            let mx = pq.pop().unwrap() as i64;
            let t = s - mx;
            if t < 1 || mx - t < 1 {
                return false;
            }
            let x = if mx % t == 0 { t } else { mx % t };
            pq.push(x as i32);
            s = s - mx + x;
        }
        true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
