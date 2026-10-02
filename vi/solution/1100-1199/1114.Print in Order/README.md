---
comments: true
difficulty: Easy
tags:
    - Concurrency
---

<!-- problem:start -->

# [1114. Print in Order](https://leetcode.com/problems/print-in-order)

[中文文档](/solution/1100-1199/1114.Print%20in%20Order/README.md)

## Mô tả

<!-- description:start -->

<p>Giả sử ta có class sau:</p>

<pre>
public class Foo {
  public void first() { print(&quot;first&quot;); }
  public void second() { print(&quot;second&quot;); }
  public void third() { print(&quot;third&quot;); }
}
</pre>

<p>Cùng một instance của <code>Foo</code> sẽ được truyền cho ba thread khác nhau. Thread A gọi <code>first()</code>, thread B gọi <code>second()</code>, và thread C gọi <code>third()</code>. Hãy thiết kế cơ chế và sửa chương trình để đảm bảo <code>second()</code> được thực thi sau <code>first()</code>, còn <code>third()</code> được thực thi sau <code>second()</code>.</p>

<p><strong>Lưu ý:</strong></p>

<p>Ta không biết hệ điều hành sẽ lập lịch cho các thread theo thứ tự nào, dù các con số trong input có vẻ ngụ ý thứ tự thực thi. Định dạng input được đưa ra chủ yếu để đảm bảo các test bao quát đầy đủ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3]
<strong>Đầu ra:</strong> &quot;firstsecondthird&quot;
<strong>Giải thích:</strong> Có ba thread được chạy async. Input [1,2,3] nghĩa là thread A gọi first(), thread B gọi second(), và thread C gọi third(). &quot;firstsecondthird&quot; là output đúng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,2]
<strong>Đầu ra:</strong> &quot;firstsecondthird&quot;
<strong>Giải thích:</strong> Input [1,3,2] nghĩa là thread A gọi first(), thread B gọi third(), và thread C gọi second(). &quot;firstsecondthird&quot; là output đúng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>nums</code> là một hoán vị của <code>[1, 2, 3]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Multithreading + Lock or Semaphore

<!-- thinking:start -->

> **Tư duy**
>
> `first`, `second`, và `third` có thể được gọi trên các thread theo bất kỳ thứ tự nào, nhưng các thao tác in phải diễn ra tuần tự. Hai lock (hoặc semaphore có số permit ban đầu bằng 0) được khóa lúc đầu để chặn `second` và `third`: sau khi in, `first` mở khóa thứ hai, rồi `second` mở khóa thứ ba, tạo thành một chuỗi một chiều.

<!-- thinking:end -->

Ta có thể dùng ba semaphore $a$, $b$ và $c$ để kiểm soát thứ tự thực thi của ba thread. Ban đầu, semaphore $a$ có số permit là $1$, còn $b$ và $c$ có số permit bằng $0$.

Khi thread $A$ thực thi method `first()`, trước tiên nó phải acquire semaphore $a$. Sau khi acquire thành công, thread thực thi `first()` rồi release semaphore $b$. Nhờ vậy, thread $B$ có thể acquire semaphore $b$ và thực thi method `second()`.

Khi thread $B$ thực thi method `second()`, trước tiên nó phải acquire semaphore $b$. Sau khi acquire thành công, thread thực thi `second()` rồi release semaphore $c$. Nhờ vậy, thread $C$ có thể acquire semaphore $c$ và thực thi method `third()`.

Khi thread $C$ thực thi method `third()`, trước tiên nó phải acquire semaphore $c$. Sau khi acquire thành công, thread thực thi `third()` rồi release semaphore $a$. Nhờ vậy, thread $A$ có thể acquire semaphore $a$ và thực thi method `first()`.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Foo:
    def __init__(self):
        self.l2 = threading.Lock()
        self.l3 = threading.Lock()
        self.l2.acquire()
        self.l3.acquire()

    def first(self, printFirst: 'Callable[[], None]') -> None:
        printFirst()
        self.l2.release()

    def second(self, printSecond: 'Callable[[], None]') -> None:
        self.l2.acquire()
        printSecond()
        self.l3.release()

    def third(self, printThird: 'Callable[[], None]') -> None:
        self.l3.acquire()
        printThird()
```

#### Java

```java
class Foo {
    private Semaphore a = new Semaphore(1);
    private Semaphore b = new Semaphore(0);
    private Semaphore c = new Semaphore(0);

    public Foo() {
    }

    public void first(Runnable printFirst) throws InterruptedException {
        a.acquire(1);
        // printFirst.run() outputs "first". Do not change or remove this line.
        printFirst.run();
        b.release(1);
    }

    public void second(Runnable printSecond) throws InterruptedException {
        b.acquire(1);
        // printSecond.run() outputs "second". Do not change or remove this line.
        printSecond.run();
        c.release(1);
    }

    public void third(Runnable printThird) throws InterruptedException {
        c.acquire(1);
        // printThird.run() outputs "third". Do not change or remove this line.
        printThird.run();
        a.release(1);
    }
}
```

#### C++

```cpp
class Foo {
private:
    mutex m2, m3;

public:
    Foo() {
        m2.lock();
        m3.lock();
    }

    void first(function<void()> printFirst) {
        printFirst();
        m2.unlock();
    }

    void second(function<void()> printSecond) {
        m2.lock();
        printSecond();
        m3.unlock();
    }

    void third(function<void()> printThird) {
        m3.lock();
        printThird();
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 mô phỏng permit bằng mutex. Cách 2 dùng ba semaphore có số permit ban đầu lần lượt là $1,0,0$, tương ứng với thứ tự “`first`, rồi `second`, rồi `third`” thông qua cơ chế đếm permit.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
from threading import Semaphore


class Foo:
    def __init__(self):
        self.a = Semaphore(1)
        self.b = Semaphore(0)
        self.c = Semaphore(0)

    def first(self, printFirst: 'Callable[[], None]') -> None:
        self.a.acquire()
        # printFirst() outputs "first". Do not change or remove this line.
        printFirst()
        self.b.release()

    def second(self, printSecond: 'Callable[[], None]') -> None:
        self.b.acquire()
        # printSecond() outputs "second". Do not change or remove this line.
        printSecond()
        self.c.release()

    def third(self, printThird: 'Callable[[], None]') -> None:
        self.c.acquire()
        # printThird() outputs "third". Do not change or remove this line.
        printThird()
        self.a.release()
```

#### C++

```cpp
#include <semaphore.h>

class Foo {
private:
    sem_t a, b, c;

public:
    Foo() {
        sem_init(&a, 0, 1);
        sem_init(&b, 0, 0);
        sem_init(&c, 0, 0);
    }

    void first(function<void()> printFirst) {
        sem_wait(&a);
        // printFirst() outputs "first". Do not change or remove this line.
        printFirst();
        sem_post(&b);
    }

    void second(function<void()> printSecond) {
        sem_wait(&b);
        // printSecond() outputs "second". Do not change or remove this line.
        printSecond();
        sem_post(&c);
    }

    void third(function<void()> printThird) {
        sem_wait(&c);
        // printThird() outputs "third". Do not change or remove this line.
        printThird();
        sem_post(&a);
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
