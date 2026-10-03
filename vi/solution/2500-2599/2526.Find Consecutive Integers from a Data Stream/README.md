---
comments: true
difficulty: Medium
rating: 1444
source: Biweekly Contest 95 Q2
tags:
    - Design
    - Queue
    - Hash Table
    - Counting
    - Data Stream
---

<!-- problem:start -->

# [2526. Find Consecutive Integers from a Data Stream](https://leetcode.com/problems/find-consecutive-integers-from-a-data-stream)

[Tài liệu tiếng Trung](/solution/2500-2599/2526.Find%20Consecutive%20Integers%20from%20a%20Data%20Stream/README.md)

## Mô tả

<!-- description:start -->

<p>Với một luồng số nguyên, hãy cài đặt một cấu trúc dữ liệu để kiểm tra xem <code>k</code> số nguyên cuối cùng được đọc từ luồng có <strong>bằng</strong> <code>value</code> hay không.</p>

<p>Hãy cài đặt lớp <strong>DataStream</strong>:</p>

<ul>
	<li><code>DataStream(int value, int k)</code> Khởi tạo đối tượng với một luồng số nguyên rỗng và hai số nguyên <code>value</code> và <code>k</code>.</li>
	<li><code>boolean consec(int num)</code> Thêm <code>num</code> vào luồng số nguyên. Trả về <code>true</code> nếu <code>k</code> số nguyên cuối cùng bằng <code>value</code>, ngược lại trả về <code>false</code>. Nếu có ít hơn <code>k</code> số nguyên thì điều kiện không được thỏa mãn, nên trả về <code>false</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;DataStream&quot;, &quot;consec&quot;, &quot;consec&quot;, &quot;consec&quot;, &quot;consec&quot;]
[[4, 3], [4], [4], [4], [3]]
<strong>Đầu ra</strong>
[null, false, false, true, false]

<strong>Giải thích</strong>
DataStream dataStream = new DataStream(4, 3); //value = 4, k = 3
dataStream.consec(4); // Only 1 integer is parsed, so returns False.
dataStream.consec(4); // Only 2 integers are parsed.
                      // Since 2 is less than k, returns False.
dataStream.consec(4); // The 3 integers parsed are all equal to value, so returns True.
dataStream.consec(3); // The last k integers parsed in the stream are [4,4,3].
                      // Since 3 is not equal to value, it returns False.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= value, num &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
	<li>Sẽ có nhiều nhất <code>10<sup>5</sup></code> lời gọi đến <code>consec</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn hỏi xem $k$ số nguyên cuối cùng có đều bằng một $\textit{value}$ cố định hay không. Nếu lưu toàn bộ luồng rồi cắt mảng sau mỗi lần gọi, lượng dữ liệu phải xử lý sẽ tăng theo số lần gọi.
>
> Điều duy nhất cần biết là độ dài đoạn liên tiếp bằng $\textit{value}$. Ta tăng biến đếm khi số tiếp theo khớp, đặt lại về 0 khi không khớp, rồi so sánh với $k$. Mỗi lần gọi chỉ mất $O(1)$.

<!-- thinking:end -->

Ta có thể duy trì một biến đếm $\textit{cnt}$ để ghi nhận số lượng số nguyên liên tiếp hiện tại bằng $\textit{value}$.

Khi gọi phương thức `consec`, nếu $\textit{num}$ bằng $\textit{value}$, ta tăng $\textit{cnt}$ lên 1; nếu không, ta đặt lại $\textit{cnt}$ về 0. Sau đó, ta kiểm tra xem $\textit{cnt}$ có lớn hơn hoặc bằng $\textit{k}$ hay không.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class DataStream:
    def __init__(self, value: int, k: int):
        self.val, self.k = value, k
        self.cnt = 0

    def consec(self, num: int) -> bool:
        self.cnt = 0 if num != self.val else self.cnt + 1
        return self.cnt >= self.k


# Your DataStream object will be instantiated and called as such:
# obj = DataStream(value, k)
# param_1 = obj.consec(num)
```

#### Java

```java
class DataStream {
    private int cnt;
    private int val;
    private int k;

    public DataStream(int value, int k) {
        val = value;
        this.k = k;
    }

    public boolean consec(int num) {
        cnt = num == val ? cnt + 1 : 0;
        return cnt >= k;
    }
}

/**
 * Your DataStream object will be instantiated and called as such:
 * DataStream obj = new DataStream(value, k);
 * boolean param_1 = obj.consec(num);
 */
```

#### C++

```cpp
class DataStream {
public:
    DataStream(int value, int k) {
        val = value;
        this->k = k;
    }

    bool consec(int num) {
        cnt = num == val ? cnt + 1 : 0;
        return cnt >= k;
    }

private:
    int cnt = 0;
    int val, k;
};

/**
 * Your DataStream object will be instantiated and called as such:
 * DataStream* obj = new DataStream(value, k);
 * bool param_1 = obj->consec(num);
 */
```

#### Go

```go
type DataStream struct {
	val, k, cnt int
}

func Constructor(value int, k int) DataStream {
	return DataStream{value, k, 0}
}

func (this *DataStream) Consec(num int) bool {
	if num == this.val {
		this.cnt++
	} else {
		this.cnt = 0
	}
	return this.cnt >= this.k
}

/**
 * Your DataStream object will be instantiated and called as such:
 * obj := Constructor(value, k);
 * param_1 := obj.Consec(num);
 */
```

#### TypeScript

```ts
class DataStream {
    private val: number;
    private k: number;
    private cnt: number;

    constructor(value: number, k: number) {
        this.val = value;
        this.k = k;
        this.cnt = 0;
    }

    consec(num: number): boolean {
        this.cnt = this.val === num ? this.cnt + 1 : 0;
        return this.cnt >= this.k;
    }
}

/**
 * Your DataStream object will be instantiated and called as such:
 * var obj = new DataStream(value, k)
 * var param_1 = obj.consec(num)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
