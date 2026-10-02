---
comments: true
difficulty: Medium
tags:
    - Concurrency
---

<!-- problem:start -->

# [1117. Building H2O](https://leetcode.com/problems/building-h2o)

[中文文档](/solution/1100-1199/1117.Building%20H2O/README.md)

## Mô tả

<!-- description:start -->

<p>Có hai loại thread: <code>oxygen</code> và <code>hydrogen</code>. Mục tiêu của bạn là nhóm các thread này để tạo thành các phân tử nước.</p>

<p>Có một barrier mà mỗi thread phải chờ cho đến khi có thể tạo thành một phân tử hoàn chỉnh. Các thread hydrogen và oxygen lần lượt được cung cấp các phương thức <code>releaseHydrogen</code> và <code>releaseOxygen</code> để đi qua barrier. Các thread phải đi qua barrier theo nhóm ba thread và ngay lập tức liên kết với nhau để tạo thành một phân tử nước. Bạn phải đảm bảo mọi thread thuộc một phân tử liên kết xong trước khi bất kỳ thread nào thuộc phân tử tiếp theo thực hiện việc đó.</p>

<p>Nói cách khác:</p>

<ul>
	<li>Nếu một thread oxygen đến barrier khi chưa có thread hydrogen nào, nó phải chờ hai thread hydrogen.</li>
	<li>Nếu một thread hydrogen đến barrier khi chưa có thread nào khác, nó phải chờ một thread oxygen và một thread hydrogen khác.</li>
</ul>

<p>Ta không cần ghép cặp cụ thể các thread với nhau; chúng không nhất thiết phải biết mình được ghép với thread nào. Điều quan trọng là các thread đi qua barrier theo từng nhóm đầy đủ. Vì vậy, nếu xét thứ tự các thread liên kết rồi chia thành từng nhóm ba, mỗi nhóm phải có một thread oxygen và hai thread hydrogen.</p>

<p>Hãy viết mã đồng bộ hóa cho các thread oxygen và hydrogen để đảm bảo những ràng buộc này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> water = &quot;HOH&quot;
<strong>Đầu ra:</strong> &quot;HHO&quot;
<strong>Giải thích:</strong> &quot;HOH&quot; và &quot;OHH&quot; cũng là các đáp án hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> water = &quot;OOHHHH&quot;
<strong>Đầu ra:</strong> &quot;HHOHHO&quot;
<strong>Giải thích:</strong> &quot;HOHHHO&quot;, &quot;OHHHHO&quot;, &quot;HHOHOH&quot;, &quot;HOHHOH&quot;, &quot;OHHHOH&quot;, &quot;HHOOHH&quot;, &quot;HOHOHH&quot; và &quot;OHHOHH&quot; cũng là các đáp án hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 * n == water.length</code></li>
	<li><code>1 &lt;= n &lt;= 20</code></li>
	<li><code>water[i]</code> là <code>&#39;H&#39;</code> hoặc <code>&#39;O&#39;</code>.</li>
	<li>Trong <code>water</code> có đúng <code>2 * n</code> ký tự <code>&#39;H&#39;</code>.</li>
	<li>Trong <code>water</code> có đúng <code>n</code> ký tự <code>&#39;O&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các phân tử phải được xuất ra theo nhóm $H_2O$; không được để ba hydrogen hoặc hai oxygen đi qua trước. Semaphore hydrogen bắt đầu ở giá trị $2$, còn oxygen bắt đầu ở $0$: hai hydrogen mở khóa cho oxygen đi qua, rồi một oxygen khôi phục hai lượt cho phép hydrogen, nhờ vậy mỗi vòng luôn gồm đúng hai H và một O.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
from threading import Semaphore


class H2O:
    def __init__(self):
        self.h = Semaphore(2)
        self.o = Semaphore(0)

    def hydrogen(self, releaseHydrogen: "Callable[[], None]") -> None:
        self.h.acquire()
        # releaseHydrogen() outputs "H". Do not change or remove this line.
        releaseHydrogen()
        if self.h._value == 0:
            self.o.release()

    def oxygen(self, releaseOxygen: "Callable[[], None]") -> None:
        self.o.acquire()
        # releaseOxygen() outputs "O". Do not change or remove this line.
        releaseOxygen()
        self.h.release(2)
```

#### Java

```java
class H2O {
    private Semaphore h = new Semaphore(2);
    private Semaphore o = new Semaphore(0);

    public H2O() {
    }

    public void hydrogen(Runnable releaseHydrogen) throws InterruptedException {
        h.acquire();
        // releaseHydrogen.run() outputs "H". Do not change or remove this line.
        releaseHydrogen.run();
        o.release();
    }

    public void oxygen(Runnable releaseOxygen) throws InterruptedException {
        o.acquire(2);
        // releaseOxygen.run() outputs "O". Do not change or remove this line.
        releaseOxygen.run();
        h.release(2);
    }
}
```

#### C++

```cpp
#include <semaphore.h>

class H2O {
private:
    sem_t h, o;
    int st;

public:
    H2O() {
        sem_init(&h, 0, 2);
        sem_init(&o, 0, 0);
        st = 0;
    }

    void hydrogen(function<void()> releaseHydrogen) {
        sem_wait(&h);
        // releaseHydrogen() outputs "H". Do not change or remove this line.
        releaseHydrogen();
        ++st;
        if (st == 2) {
            sem_post(&o);
        }
    }

    void oxygen(function<void()> releaseOxygen) {
        sem_wait(&o);
        // releaseOxygen() outputs "O". Do not change or remove this line.
        releaseOxygen();
        st = 0;
        sem_post(&h);
        sem_post(&h);
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
