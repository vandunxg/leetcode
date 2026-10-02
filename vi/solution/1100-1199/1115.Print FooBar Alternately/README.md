---
comments: true
difficulty: Medium
tags:
    - Concurrency
---

<!-- problem:start -->

# [1115. Print FooBar Alternately](https://leetcode.com/problems/print-foobar-alternately)

[中文文档](/solution/1100-1199/1115.Print%20FooBar%20Alternately/README.md)

## Mô tả

<!-- description:start -->

<p>Giả sử bạn được cho đoạn code sau:</p>

<pre>
class FooBar {
  public void foo() {
    for (int i = 0; i &lt; n; i++) {
      print(&quot;foo&quot;);
    }
  }

  public void bar() {
    for (int i = 0; i &lt; n; i++) {
      print(&quot;bar&quot;);
    }
  }
}
</pre>

<p>Cùng một instance của <code>FooBar</code> sẽ được truyền cho hai thread khác nhau:</p>

<ul>
	<li>thread <code>A</code> sẽ gọi <code>foo()</code>, còn</li>
	<li>thread <code>B</code> sẽ gọi <code>bar()</code>.</li>
</ul>

<p>Hãy sửa chương trình đã cho để in <code>&quot;foobar&quot;</code> <code>n</code> lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> &quot;foobar&quot;
<strong>Giải thích:</strong> Hai thread được chạy bất đồng bộ. Một thread gọi foo(), còn thread kia gọi bar().
&quot;foobar&quot; được in 1 lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> &quot;foobarfoobar&quot;
<strong>Giải thích:</strong> &quot;foobar&quot; được in 2 lần.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Multithreading và Semaphore

<!-- thinking:start -->

> **Tư duy**
>
> Hai thread phải luân phiên đúng $n$ lần. Semaphore $f$ và $b$ khởi tạo lần lượt bằng $1$ và $0$ để `foo` chạy trước; sau mỗi lần in, thread đang chạy release semaphore của thread kia rồi chờ đến lượt tiếp theo. Sau $n$ vòng, kết quả là `foobar` lặp lại $n$ lần.

<!-- thinking:end -->

Ta dùng hai semaphore $f$ và $b$ để điều khiển thứ tự chạy của hai thread. Ban đầu, $f$ bằng $1$ và $b$ bằng $0$, nghĩa là thread $A$ chạy trước.

Khi thread $A$ chạy, trước tiên nó thực hiện thao tác $acquire$ trên $f$, khiến giá trị của $f$ chuyển thành $0$. Sau đó thread $A$ được phép sử dụng $f$ và chạy hàm $foo$. Tiếp theo, nó thực hiện thao tác $release$ trên $b$, khiến giá trị của $b$ thành $1$. Nhờ vậy, thread $B$ có thể sử dụng $b$ và chạy hàm $bar$.

Khi thread $B$ chạy, trước tiên nó thực hiện thao tác $acquire$ trên $b$, khiến giá trị của $b$ chuyển thành $0$. Sau đó thread $B$ được phép sử dụng $b$ và chạy hàm $bar$. Tiếp theo, nó thực hiện thao tác $release$ trên $f$, khiến giá trị của $f$ thành $1$. Nhờ vậy, thread $A$ có thể sử dụng $f$ và chạy hàm $foo$.

Vì vậy, chỉ cần lặp $n$ lần; mỗi lần chạy các hàm $foo$ và $bar$, thực hiện $acquire$ trước rồi mới $release$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
from threading import Semaphore


class FooBar:
    def __init__(self, n):
        self.n = n
        self.f = Semaphore(1)
        self.b = Semaphore(0)

    def foo(self, printFoo: "Callable[[], None]") -> None:
        for _ in range(self.n):
            self.f.acquire()
            # printFoo() outputs "foo". Do not change or remove this line.
            printFoo()
            self.b.release()

    def bar(self, printBar: "Callable[[], None]") -> None:
        for _ in range(self.n):
            self.b.acquire()
            # printBar() outputs "bar". Do not change or remove this line.
            printBar()
            self.f.release()
```

#### Java

```java
class FooBar {
    private int n;
    private Semaphore f = new Semaphore(1);
    private Semaphore b = new Semaphore(0);

    public FooBar(int n) {
        this.n = n;
    }

    public void foo(Runnable printFoo) throws InterruptedException {
        for (int i = 0; i < n; i++) {
            f.acquire(1);
            // printFoo.run() outputs "foo". Do not change or remove this line.
            printFoo.run();
            b.release(1);
        }
    }

    public void bar(Runnable printBar) throws InterruptedException {
        for (int i = 0; i < n; i++) {
            b.acquire(1);
            // printBar.run() outputs "bar". Do not change or remove this line.
            printBar.run();
            f.release(1);
        }
    }
}
```

#### C++

```cpp
#include <semaphore.h>

class FooBar {
private:
    int n;
    sem_t f, b;

public:
    FooBar(int n) {
        this->n = n;
        sem_init(&f, 0, 1);
        sem_init(&b, 0, 0);
    }

    void foo(function<void()> printFoo) {
        for (int i = 0; i < n; i++) {
            sem_wait(&f);
            // printFoo() outputs "foo". Do not change or remove this line.
            printFoo();
            sem_post(&b);
        }
    }

    void bar(function<void()> printBar) {
        for (int i = 0; i < n; i++) {
            sem_wait(&b);
            // printBar() outputs "bar". Do not change or remove this line.
            printBar();
            sem_post(&f);
        }
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
