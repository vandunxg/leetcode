---
comments: true
difficulty: Medium
tags:
    - Concurrency
---

<!-- problem:start -->

# [1116. Print Zero Even Odd](https://leetcode.com/problems/print-zero-even-odd)

[中文文档](/solution/1100-1199/1116.Print%20Zero%20Even%20Odd/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có hàm <code>printNumber</code>, nhận một số nguyên làm tham số và in số đó ra console.</p>

<ul>
	<li>Ví dụ, gọi <code>printNumber(7)</code> sẽ in <code>7</code> ra console.</li>
</ul>

<p>Bạn được cho một instance của class <code>ZeroEvenOdd</code> có ba hàm: <code>zero</code>, <code>even</code> và <code>odd</code>. Cùng một instance <code>ZeroEvenOdd</code> sẽ được truyền cho ba thread khác nhau:</p>

<ul>
	<li><strong>Thread A:</strong> gọi <code>zero()</code> và chỉ in các số <code>0</code>.</li>
	<li><strong>Thread B:</strong> gọi <code>even()</code> và chỉ in các số chẵn.</li>
	<li><strong>Thread C:</strong> gọi <code>odd()</code> và chỉ in các số lẻ.</li>
</ul>

<p>Hãy sửa class đã cho để in dãy <code>&quot;010203040506...&quot;</code> có độ dài bằng <code>2n</code>.</p>

<p>Hãy triển khai class <code>ZeroEvenOdd</code>:</p>

<ul>
	<li><code>ZeroEvenOdd(int n)</code> khởi tạo object với số <code>n</code>, biểu thị các số cần được in.</li>
	<li><code>void zero(printNumber)</code> gọi <code>printNumber</code> để in một số 0.</li>
	<li><code>void even(printNumber)</code> gọi <code>printNumber</code> để in một số chẵn.</li>
	<li><code>void odd(printNumber)</code> gọi <code>printNumber</code> để in một số lẻ.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> &quot;0102&quot;
<strong>Giải thích:</strong> Có ba thread được chạy async.
Một thread gọi zero(), thread khác gọi even(), và thread còn lại gọi odd().
&quot;0102&quot; là output đúng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> &quot;0102030405&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Multithreading + Semaphore

<!-- thinking:start -->

> **Tư duy**
>
> Dãy cần in là $010203\ldots$: mỗi số đều đứng sau một số 0, và ở mỗi lượt chỉ một trong hai hàm in số chẵn/lẻ được chạy. Ban đầu, chỉ semaphore $z$ có giá trị $1$. Sau khi `zero` in, nó đánh thức `odd` hoặc `even` tùy theo tính chẵn lẻ; thread đó in số rồi đánh thức `zero`, luân phiên quyền thực thi giữa ba thread.

<!-- thinking:end -->

Ta dùng ba semaphore $z$, $e$ và $o$ để kiểm soát thứ tự thực thi của ba thread. Ban đầu, $z$ được đặt thành $1$, còn $e$ và $o$ được đặt thành $0$.

- Semaphore $z$ kiểm soát việc thực thi hàm `zero`. Khi giá trị của $z$ là $1$, hàm `zero` có thể chạy. Sau khi chạy, giá trị của $z$ được đặt thành $0$, còn giá trị của $e$ hoặc $o$ được đặt thành $1$, tùy theo hàm `even` hay `odd` cần chạy tiếp theo.
- Semaphore $e$ kiểm soát việc thực thi hàm `even`. Khi giá trị của $e$ là $1$, hàm `even` có thể chạy. Sau khi chạy, giá trị của $z$ được đặt thành $1$, còn giá trị của $e$ được đặt thành $0$.
- Semaphore $o$ kiểm soát việc thực thi hàm `odd`. Khi giá trị của $o$ là $1$, hàm `odd` có thể chạy. Sau khi chạy, giá trị của $z$ được đặt thành $1$, còn giá trị của $o$ được đặt thành $0$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
from threading import Semaphore


class ZeroEvenOdd:
    def __init__(self, n):
        self.n = n
        self.z = Semaphore(1)
        self.e = Semaphore(0)
        self.o = Semaphore(0)

    # printNumber(x) outputs "x", where x is an integer.
    def zero(self, printNumber: 'Callable[[int], None]') -> None:
        for i in range(self.n):
            self.z.acquire()
            printNumber(0)
            if i % 2 == 0:
                self.o.release()
            else:
                self.e.release()

    def even(self, printNumber: 'Callable[[int], None]') -> None:
        for i in range(2, self.n + 1, 2):
            self.e.acquire()
            printNumber(i)
            self.z.release()

    def odd(self, printNumber: 'Callable[[int], None]') -> None:
        for i in range(1, self.n + 1, 2):
            self.o.acquire()
            printNumber(i)
            self.z.release()
```

#### Java

```java
class ZeroEvenOdd {
    private int n;
    private Semaphore z = new Semaphore(1);
    private Semaphore e = new Semaphore(0);
    private Semaphore o = new Semaphore(0);

    public ZeroEvenOdd(int n) {
        this.n = n;
    }

    // printNumber.accept(x) outputs "x", where x is an integer.
    public void zero(IntConsumer printNumber) throws InterruptedException {
        for (int i = 0; i < n; ++i) {
            z.acquire(1);
            printNumber.accept(0);
            if (i % 2 == 0) {
                o.release(1);
            } else {
                e.release(1);
            }
        }
    }

    public void even(IntConsumer printNumber) throws InterruptedException {
        for (int i = 2; i <= n; i += 2) {
            e.acquire(1);
            printNumber.accept(i);
            z.release(1);
        }
    }

    public void odd(IntConsumer printNumber) throws InterruptedException {
        for (int i = 1; i <= n; i += 2) {
            o.acquire(1);
            printNumber.accept(i);
            z.release(1);
        }
    }
}
```

#### C++

```cpp
#include <semaphore.h>

class ZeroEvenOdd {
private:
    int n;
    sem_t z, e, o;

public:
    ZeroEvenOdd(int n) {
        this->n = n;
        sem_init(&z, 0, 1);
        sem_init(&e, 0, 0);
        sem_init(&o, 0, 0);
    }

    // printNumber(x) outputs "x", where x is an integer.
    void zero(function<void(int)> printNumber) {
        for (int i = 0; i < n; ++i) {
            sem_wait(&z);
            printNumber(0);
            if (i % 2 == 0) {
                sem_post(&o);
            } else {
                sem_post(&e);
            }
        }
    }

    void even(function<void(int)> printNumber) {
        for (int i = 2; i <= n; i += 2) {
            sem_wait(&e);
            printNumber(i);
            sem_post(&z);
        }
    }

    void odd(function<void(int)> printNumber) {
        for (int i = 1; i <= n; i += 2) {
            sem_wait(&o);
            printNumber(i);
            sem_post(&z);
        }
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
