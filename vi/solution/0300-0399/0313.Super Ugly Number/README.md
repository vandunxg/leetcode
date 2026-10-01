---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [313. Super Ugly Number](https://leetcode.com/problems/super-ugly-number)

[中文文档](/solution/0300-0399/0313.Super%20Ugly%20Number/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Số super ugly</strong> là số nguyên dương có các thừa số nguyên tố đều thuộc mảng <code>primes</code>.</p>

<p>Cho số nguyên <code>n</code> và mảng số nguyên <code>primes</code>, hãy trả về <em><strong>số nằm ở vị trí</strong></em> <code>n<sup>th</sup></code> trong dãy số super ugly.</p>

<p><strong>Đảm bảo</strong> số <strong>super ugly</strong> ở vị trí <code>n<sup>th</sup></code> nằm trong phạm vi số nguyên có dấu <strong>32-bit</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 12, primes = [2,7,13,19]
<strong>Đầu ra:</strong> 32
<strong>Giải thích:</strong> [1,2,4,7,8,13,14,16,19,26,28,32] là dãy gồm 12 số super ugly đầu tiên ứng với primes = [2,7,13,19].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, primes = [2,3,5]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> 1 không có thừa số nguyên tố, vì vậy mọi thừa số nguyên tố của nó đều thuộc mảng primes = [2,3,5].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= primes.length &lt;= 100</code></li>
	<li><code>2 &lt;= primes[i] &lt;= 1000</code></li>
	<li><code>primes[i]</code> được <strong>đảm bảo</strong> là số nguyên tố.</li>
	<li>Các giá trị trong <code>primes</code> đều <strong>khác nhau</strong> và được sắp xếp theo <strong>thứ tự tăng dần</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Priority Queue (Min Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Số super ugly chỉ có thể có các thừa số nguyên tố đã cho. Thử chia từng số nguyên sẽ quá chậm khi $n$ lớn.
>
> Bắt đầu từ $1$, lấy giá trị nhỏ nhất $x$ ra khỏi heap rồi thêm $x\times p$ vào nếu tích không bị overflow. Nếu $x$ chia hết cho số nguyên tố hiện tại, bỏ qua các số nguyên tố tiếp theo (tương tự Euler sieve). Giá trị được lấy ra lần thứ $n$ là đáp án.

<!-- thinking:end -->

Ta dùng priority queue (min-heap) để lưu các số super ugly có thể có, ban đầu đưa $1$ vào queue.

Mỗi lần, ta lấy số super ugly nhỏ nhất $x$ khỏi queue, nhân $x$ với từng giá trị trong mảng `primes`, rồi đưa các tích vào queue. Lặp lại thao tác này $n$ lần để tìm số super ugly thứ $n$.

Vì đề bài đảm bảo số super ugly thứ $n$ nằm trong phạm vi số nguyên có dấu 32-bit, trước khi đưa tích vào queue, ta có thể kiểm tra xem tích có vượt quá $2^{31} - 1$ hay không. Nếu có thì không cần thêm tích đó vào queue. Ngoài ra, có thể dùng Euler sieve để tối ưu.

Độ phức tạp thời gian là $O(n \times m \times \log (n \times m))$, độ phức tạp không gian là $O(n \times m)$, trong đó $m$ là độ dài mảng `primes` và $n$ là số nguyên đã cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nthSuperUglyNumber(self, n: int, primes: List[int]) -> int:
        q = [1]
        x = 0
        mx = (1 << 31) - 1
        for _ in range(n):
            x = heappop(q)
            for k in primes:
                if x <= mx // k:
                    heappush(q, k * x)
                if x % k == 0:
                    break
        return x
```

#### Java

```java
class Solution {
    public int nthSuperUglyNumber(int n, int[] primes) {
        PriorityQueue<Integer> q = new PriorityQueue<>();
        q.offer(1);
        int x = 0;
        while (n-- > 0) {
            x = q.poll();
            while (!q.isEmpty() && q.peek() == x) {
                q.poll();
            }
            for (int k : primes) {
                if (k <= Integer.MAX_VALUE / x) {
                    q.offer(k * x);
                }
                if (x % k == 0) {
                    break;
                }
            }
        }
        return x;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int nthSuperUglyNumber(int n, vector<int>& primes) {
        priority_queue<int, vector<int>, greater<int>> q;
        q.push(1);
        int x = 0;
        while (n--) {
            x = q.top();
            q.pop();
            for (int& k : primes) {
                if (x <= INT_MAX / k) {
                    q.push(k * x);
                }
                if (x % k == 0) {
                    break;
                }
            }
        }
        return x;
    }
};
```

#### Go

```go
func nthSuperUglyNumber(n int, primes []int) (x int) {
	q := hp{[]int{1}}
	for n > 0 {
		n--
		x = heap.Pop(&q).(int)
		for _, k := range primes {
			if x <= math.MaxInt32/k {
				heap.Push(&q, k*x)
			}
			if x%k == 0 {
				break
			}
		}
	}
	return
}

type hp struct{ sort.IntSlice }

func (h *hp) Push(v any) { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Dynamic Programming + Heap nhiều pointer

<!-- thinking:start -->

> **Tư duy**
>
> Ở lời giải 1, mỗi số ugly được nhân với tất cả prime nên heap tăng lớn. Thay vào đó, dùng một pointer cho mỗi prime: phần tử ở đỉnh heap là ứng viên tiếp theo; sau khi ghi nhận nó, thêm bội số tiếp theo của prime đó. Heap chỉ cần $O(m)$ phần tử, thời gian chạy là $O(n\log m)$.

<!-- thinking:end -->

Lưu $n$ số super ugly đầu tiên trong $ugly[1..n]$ và duy trì một min-heap. Mỗi phần tử heap ứng với một prime $p$ và ghi lại ứng viên tiếp theo $p \times ugly[\textit{index}]$.

Khởi tạo $ugly[1] = 1$ rồi thêm $(p, p, 2)$ vào heap cho mỗi prime. Liên tục lấy phần tử nhỏ nhất trong heap và ghi vào $ugly$, sau đó tăng pointer của prime tương ứng rồi đưa phần tử đó trở lại heap. Mỗi prime chỉ có một pointer nên không cần nhân từng số ugly được tạo ra với toàn bộ danh sách prime.

Độ phức tạp thời gian là $O(n \times \log m)$ và độ phức tạp không gian là $O(n + m)$, trong đó $m$ là độ dài của $\textit{primes}$.

<!-- tabs:start -->

#### Go

```go
type Ugly struct{ value, prime, index int }
type Queue []Ugly

func (u Queue) Len() int           { return len(u) }
func (u Queue) Swap(i, j int)      { u[i], u[j] = u[j], u[i] }
func (u Queue) Less(i, j int) bool { return u[i].value < u[j].value }
func (u *Queue) Push(v any)        { *u = append(*u, v.(Ugly)) }
func (u *Queue) Pop() any {
	old, x := *u, (*u)[len(*u)-1]
	*u = old[:len(old)-1]
	return x
}

func nthSuperUglyNumber(n int, primes []int) int {
	ugly, pq, p := make([]int, n+1), &Queue{}, 2
	ugly[1] = 1
	heap.Init(pq)
	for _, v := range primes {
		heap.Push(pq, Ugly{value: v, prime: v, index: 2})
	}
	for p <= n {
		top := heap.Pop(pq).(Ugly)
		if ugly[p-1] != top.value {
			ugly[p], p = top.value, p+1
		}
		top.value, top.index = ugly[top.index]*top.prime, top.index+1
		heap.Push(pq, top)
	}
	return ugly[n]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
