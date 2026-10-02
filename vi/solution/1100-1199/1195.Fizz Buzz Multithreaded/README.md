---
comments: true
difficulty: Medium
tags:
    - Concurrency
---

<!-- problem:start -->

# [1195. Fizz Buzz Multithreaded](https://leetcode.com/problems/fizz-buzz-multithreaded)

[中文文档](/solution/1100-1199/1195.Fizz%20Buzz%20Multithreaded/README.md)

## Mô tả

<!-- description:start -->

<p>Có bốn hàm sau:</p>

<ul>
	<li><code>printFizz</code> in từ <code>&quot;fizz&quot;</code> ra console,</li>
	<li><code>printBuzz</code> in từ <code>&quot;buzz&quot;</code> ra console,</li>
	<li><code>printFizzBuzz</code> in từ <code>&quot;fizzbuzz&quot;</code> ra console, và</li>
	<li><code>printNumber</code> in một số nguyên cho trước ra console.</li>
</ul>

<p>Bạn được cung cấp một instance của class <code>FizzBuzz</code> có bốn hàm: <code>fizz</code>, <code>buzz</code>, <code>fizzbuzz</code> và <code>number</code>. Cùng một instance <code>FizzBuzz</code> sẽ được truyền cho bốn thread khác nhau:</p>

<ul>
	<li><strong>Thread A:</strong> gọi <code>fizz()</code> và cần in từ <code>&quot;fizz&quot;</code>.</li>
	<li><strong>Thread B:</strong> gọi <code>buzz()</code> và cần in từ <code>&quot;buzz&quot;</code>.</li>
	<li><strong>Thread C:</strong> gọi <code>fizzbuzz()</code> và cần in từ <code>&quot;fizzbuzz&quot;</code>.</li>
	<li><strong>Thread D:</strong> gọi <code>number()</code> và chỉ in các số nguyên.</li>
</ul>

<p>Hãy sửa class đã cho để in dãy <code>[1, 2, &quot;fizz&quot;, 4, &quot;buzz&quot;, ...]</code>, trong đó phần tử ở vị trí <code>i<sup>th</sup></code> của dãy (đánh số từ <strong>1</strong>) là:</p>

<ul>
	<li><code>&quot;fizzbuzz&quot;</code> nếu <code>i</code> chia hết cho cả <code>3</code> và <code>5</code>,</li>
	<li><code>&quot;fizz&quot;</code> nếu <code>i</code> chia hết cho <code>3</code> nhưng không chia hết cho <code>5</code>,</li>
	<li><code>&quot;buzz&quot;</code> nếu <code>i</code> chia hết cho <code>5</code> nhưng không chia hết cho <code>3</code>, hoặc</li>
	<li><code>i</code> nếu <code>i</code> không chia hết cho <code>3</code> cũng không chia hết cho <code>5</code>.</li>
</ul>

<p>Hãy triển khai class <code>FizzBuzz</code>:</p>

<ul>
	<li><code>FizzBuzz(int n)</code> Khởi tạo object với số <code>n</code>, biểu thị độ dài của dãy cần in.</li>
	<li><code>void fizz(printFizz)</code> Gọi <code>printFizz</code> để in <code>&quot;fizz&quot;</code>.</li>
	<li><code>void buzz(printBuzz)</code> Gọi <code>printBuzz</code> để in <code>&quot;buzz&quot;</code>.</li>
	<li><code>void fizzbuzz(printFizzBuzz)</code> Gọi <code>printFizzBuzz</code> để in <code>&quot;fizzbuzz&quot;</code>.</li>
	<li><code>void number(printNumber)</code> Gọi <code>printnumber</code> để in các số.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Input:</strong> n = 15
<strong>Output:</strong> [1,2,"fizz",4,"buzz","fizz",7,8,"fizz","buzz",11,"fizz",13,14,"fizzbuzz"]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Input:</strong> n = 5
<strong>Output:</strong> [1,2,"fizz",4,"buzz"]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bốn thread lần lượt in `fizz`, `buzz`, `fizzbuzz` và các số theo thứ tự $1..n$. Thread `number` giữ permit chính và dựa vào việc $i$ chia hết cho $3$ hay $5$ để nhả permit cho thread tương ứng; thread đó in kết quả rồi trả permit lại. Nếu không rơi vào các trường hợp này, `number` tự in $i$, nhờ vậy mỗi thời điểm chỉ có một output được xử lý.

<!-- thinking:end -->

<!-- tabs:start -->

#### Java

```java
class FizzBuzz {
    private int n;

    public FizzBuzz(int n) {
        this.n = n;
    }

    private Semaphore fSema = new Semaphore(0);
    private Semaphore bSema = new Semaphore(0);
    private Semaphore fbSema = new Semaphore(0);
    private Semaphore nSema = new Semaphore(1);

    // printFizz.run() outputs "fizz".
    public void fizz(Runnable printFizz) throws InterruptedException {
        for (int i = 3; i <= n; i = i + 3) {
            if (i % 5 != 0) {
                fSema.acquire();
                printFizz.run();
                nSema.release();
            }
        }
    }

    // printBuzz.run() outputs "buzz".
    public void buzz(Runnable printBuzz) throws InterruptedException {
        for (int i = 5; i <= n; i = i + 5) {
            if (i % 3 != 0) {
                bSema.acquire();
                printBuzz.run();
                nSema.release();
            }
        }
    }

    // printFizzBuzz.run() outputs "fizzbuzz".
    public void fizzbuzz(Runnable printFizzBuzz) throws InterruptedException {
        for (int i = 15; i <= n; i = i + 15) {
            fbSema.acquire();
            printFizzBuzz.run();
            nSema.release();
        }
    }

    // printNumber.accept(x) outputs "x", where x is an integer.
    public void number(IntConsumer printNumber) throws InterruptedException {
        for (int i = 1; i <= n; i++) {
            nSema.acquire();
            if (i % 3 == 0 && i % 5 == 0) {
                fbSema.release();
            } else if (i % 3 == 0) {
                fSema.release();
            } else if (i % 5 == 0) {
                bSema.release();
            } else {
                printNumber.accept(i);
                nSema.release();
            }
        }
    }
}
```

#### C++

```cpp
class FizzBuzz {
private:
    std::mutex mtx;
    atomic<int> index;
    int n;

    // 这里主要运用到了C++11中的RAII锁(lock_guard)的知识。
    // 需要强调的一点是，在进入循环后，要时刻不忘加入index <= n的逻辑
public:
    FizzBuzz(int n) {
        this->n = n;
        index = 1;
    }

    void fizz(function<void()> printFizz) {
        while (index <= n) {
            std::lock_guard<std::mutex> lk(mtx);
            if (0 == index % 3 && 0 != index % 5 && index <= n) {
                printFizz();
                index++;
            }
        }
    }

    void buzz(function<void()> printBuzz) {
        while (index <= n) {
            std::lock_guard<std::mutex> lk(mtx);
            if (0 == index % 5 && 0 != index % 3 && index <= n) {
                printBuzz();
                index++;
            }
        }
    }

    void fizzbuzz(function<void()> printFizzBuzz) {
        while (index <= n) {
            std::lock_guard<std::mutex> lk(mtx);
            if (0 == index % 15 && index <= n) {
                printFizzBuzz();
                index++;
            }
        }
    }

    void number(function<void(int)> printNumber) {
        while (index <= n) {
            std::lock_guard<std::mutex> lk(mtx);
            if (0 != index % 3 && 0 != index % 5 && index <= n) {
                printNumber(index);
                index++;
            }
        }
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
