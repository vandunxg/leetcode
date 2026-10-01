---
comments: true
difficulty: Easy
tags:
    - Design
    - Hash Table
    - Data Stream
---

<!-- problem:start -->

# [359. Logger Rate Limiter 🔒](https://leetcode.com/problems/logger-rate-limiter)

[中文文档](/solution/0300-0399/0359.Logger%20Rate%20Limiter/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế hệ thống logger nhận luồng message kèm timestamp. Mỗi message <strong>duy nhất</strong> chỉ được in <strong>nhiều nhất một lần trong mỗi 10 giây</strong> (tức là message được in tại timestamp <code>t</code> sẽ ngăn các message giống hệt được in cho đến timestamp <code>t + 10</code>).</p>

<p>Các message luôn đến theo thứ tự thời gian. Nhiều message có thể đến cùng một timestamp.</p>

<p>Hãy triển khai class <code>Logger</code>:</p>

<ul>
	<li><code>Logger()</code> Khởi tạo đối tượng <code>logger</code>.</li>
	<li><code>bool shouldPrintMessage(int timestamp, string message)</code> Trả về <code>true</code> nếu cần in <code>message</code> tại <code>timestamp</code> đã cho; ngược lại trả về <code>false</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;Logger&quot;, &quot;shouldPrintMessage&quot;, &quot;shouldPrintMessage&quot;, &quot;shouldPrintMessage&quot;, &quot;shouldPrintMessage&quot;, &quot;shouldPrintMessage&quot;, &quot;shouldPrintMessage&quot;]
[[], [1, &quot;foo&quot;], [2, &quot;bar&quot;], [3, &quot;foo&quot;], [8, &quot;bar&quot;], [10, &quot;foo&quot;], [11, &quot;foo&quot;]]
<strong>Đầu ra</strong>
[null, true, true, false, false, false, true]

<strong>Giải thích</strong>
Logger logger = new Logger();
logger.shouldPrintMessage(1, &quot;foo&quot;);  // return true, next allowed timestamp for &quot;foo&quot; is 1 + 10 = 11
logger.shouldPrintMessage(2, &quot;bar&quot;);  // return true, next allowed timestamp for &quot;bar&quot; is 2 + 10 = 12
logger.shouldPrintMessage(3, &quot;foo&quot;);  // 3 &lt; 11, return false
logger.shouldPrintMessage(8, &quot;bar&quot;);  // 8 &lt; 12, return false
logger.shouldPrintMessage(10, &quot;foo&quot;); // 10 &lt; 11, return false
logger.shouldPrintMessage(11, &quot;foo&quot;); // 11 &gt;= 11, return true, next allowed timestamp for &quot;foo&quot; is 11 + 10 = 21
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= timestamp &lt;= 10<sup>9</sup></code></li>
	<li>Mọi <code>timestamp</code> đều được truyền theo thứ tự không giảm (thứ tự thời gian).</li>
	<li><code>1 &lt;= message.length &lt;= 30</code></li>
	<li>Sẽ có nhiều nhất <code>10<sup>4</sup></code> lần gọi <code>shouldPrintMessage</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi message chỉ được in tối đa một lần trong mỗi $10$ giây; timestamp không giảm. Không cần quét lại toàn bộ log mỗi lần.
>
> Map lưu thời điểm tiếp theo được phép in. Nếu `timestamp` vẫn nhỏ hơn thời điểm đó thì từ chối; nếu không, cập nhật thời điểm thành $t+10$ và in message.

<!-- thinking:end -->

Ta dùng hash table $\textit{ts}$ để lưu timestamp tiếp theo mà mỗi message được phép in. Khi gọi method `shouldPrintMessage`, ta kiểm tra timestamp hiện tại có lớn hơn hoặc bằng thời điểm được phép in tiếp theo của message đó hay không. Nếu có, ta cập nhật thời điểm này thành timestamp hiện tại cộng 10 rồi trả về `true`; nếu không thì trả về `false`.

Độ phức tạp thời gian là $O(1)$. Độ phức tạp không gian là $O(m)$, trong đó $m$ là số message khác nhau.

<!-- tabs:start -->

#### Python3

```python
class Logger:

    def __init__(self):
        self.ts = {}

    def shouldPrintMessage(self, timestamp: int, message: str) -> bool:
        t = self.ts.get(message, 0)
        if t > timestamp:
            return False
        self.ts[message] = timestamp + 10
        return True


# Your Logger object will be instantiated and called as such:
# obj = Logger()
# param_1 = obj.shouldPrintMessage(timestamp,message)
```

#### Java

```java
class Logger {
    private Map<String, Integer> ts = new HashMap<>();

    public Logger() {
    }

    public boolean shouldPrintMessage(int timestamp, String message) {
        int t = ts.getOrDefault(message, 0);
        if (timestamp < t) {
            return false;
        }
        ts.put(message, timestamp + 10);
        return true;
    }
}

/**
 * Your Logger object will be instantiated and called as such:
 * Logger obj = new Logger();
 * boolean param_1 = obj.shouldPrintMessage(timestamp,message);
 */
```

#### C++

```cpp
class Logger {
public:
    Logger() {
    }

    bool shouldPrintMessage(int timestamp, string message) {
        if (ts.contains(message) && ts[message] > timestamp) {
            return false;
        }
        ts[message] = timestamp + 10;
        return true;
    }

private:
    unordered_map<string, int> ts;
};

/**
 * Your Logger object will be instantiated and called as such:
 * Logger* obj = new Logger();
 * bool param_1 = obj->shouldPrintMessage(timestamp,message);
 */
```

#### Go

```go
type Logger struct {
	ts map[string]int
}

func Constructor() Logger {
	return Logger{ts: make(map[string]int)}
}

func (this *Logger) ShouldPrintMessage(timestamp int, message string) bool {
	if t, ok := this.ts[message]; ok && timestamp < t {
		return false
	}
	this.ts[message] = timestamp + 10
	return true
}

/**
 * Your Logger object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.ShouldPrintMessage(timestamp,message);
 */
```

#### JavaScript

```js
/**
 * Initialize your data structure here.
 */
var Logger = function () {
    this.limiter = {};
};

/**
 * Returns true if the message should be printed in the given timestamp, otherwise returns false.
        If this method returns false, the message will not be printed.
        The timestamp is in seconds granularity.
 * @param {number} timestamp
 * @param {string} message
 * @return {boolean}
 */
Logger.prototype.shouldPrintMessage = function (timestamp, message) {
    const t = this.limiter[message] || 0;
    if (t > timestamp) {
        return false;
    }
    this.limiter[message] = timestamp + 10;
    return true;
};

/**
 * Your Logger object will be instantiated and called as such:
 * var obj = new Logger()
 * var param_1 = obj.shouldPrintMessage(timestamp,message)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
