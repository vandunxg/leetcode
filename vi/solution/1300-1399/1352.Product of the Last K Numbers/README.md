---
comments: true
difficulty: Medium
rating: 1473
source: Weekly Contest 176 Q2
tags:
    - Design
    - Array
    - Math
    - Data Stream
    - Prefix Sum
---

<!-- problem:start -->

# [1352. Product of the Last K Numbers](https://leetcode.com/problems/product-of-the-last-k-numbers)

[中文文档](/solution/1300-1399/1352.Product%20of%20the%20Last%20K%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế thuật toán nhận một luồng số nguyên và truy vấn tích của <code>k</code> số nguyên cuối cùng trong luồng.</p>

<p>Cài đặt class <code>ProductOfNumbers</code>:</p>

<ul>
	<li><code>ProductOfNumbers()</code> khởi tạo đối tượng với một luồng rỗng.</li>
	<li><code>void add(int num)</code> thêm số nguyên <code>num</code> vào cuối luồng.</li>
	<li><code>int getProduct(int k)</code> trả về tích của <code>k</code> số cuối cùng trong danh sách hiện tại. Có thể giả định danh sách hiện tại luôn có ít nhất <code>k</code> số.</li>
</ul>

<p>Test case được tạo sao cho tại mọi thời điểm, tích của bất kỳ dãy số liên tiếp nào cũng vừa trong một số nguyên 32-bit, không bị tràn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;ProductOfNumbers&quot;,&quot;add&quot;,&quot;add&quot;,&quot;add&quot;,&quot;add&quot;,&quot;add&quot;,&quot;getProduct&quot;,&quot;getProduct&quot;,&quot;getProduct&quot;,&quot;add&quot;,&quot;getProduct&quot;]
[[],[3],[0],[2],[5],[4],[2],[3],[4],[8],[2]]

<strong>Đầu ra</strong>
[null,null,null,null,null,null,20,40,0,null,32]

<strong>Giải thích</strong>
ProductOfNumbers productOfNumbers = new ProductOfNumbers();
productOfNumbers.add(3);        // [3]
productOfNumbers.add(0);        // [3,0]
productOfNumbers.add(2);        // [3,0,2]
productOfNumbers.add(5);        // [3,0,2,5]
productOfNumbers.add(4);        // [3,0,2,5,4]
productOfNumbers.getProduct(2); // return 20. The product of the last 2 numbers is 5 * 4 = 20
productOfNumbers.getProduct(3); // return 40. The product of the last 3 numbers is 2 * 5 * 4 = 40
productOfNumbers.getProduct(4); // return 0. The product of the last 4 numbers is 0 * 2 * 5 * 4 = 0
productOfNumbers.add(8);        // [3,0,2,5,4,8]
productOfNumbers.getProduct(2); // return 32. The product of the last 2 numbers is 4 * 8 = 32 
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= num &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 4 * 10<sup>4</sup></code></li>
	<li>Tổng cộng có nhiều nhất <code>4 * 10<sup>4</sup></code> lần gọi <code>add</code> và <code>getProduct</code>.</li>
	<li>Tích của các số trong luồng tại mọi thời điểm sẽ vừa trong một số nguyên <strong>32-bit</strong>.</li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng: </strong>Bạn có thể cài đặt cả <code>GetProduct</code> và <code>Add</code> sao cho độ phức tạp thời gian là <code>O(1)</code> thay vì <code>O(k)</code> không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tích tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Thêm số vào luồng và truy vấn tích của $k$ số cuối. Tính tích mỗi lần truy vấn mất $O(k)$, trong khi có thể có tới $4 \times 10^4$ lần gọi. Tích tiền tố giúp tính tích một đoạn cuối bằng phép chia hai giá trị. Khi gặp $0$, mọi đoạn sau đó có chứa nó đều có tích bằng $0$, nên ta đặt lại mảng tích tiền tố thành $[1]$; nếu mảng có độ dài không quá $k$ thì đoạn đang truy vấn có chứa $0$.

<!-- thinking:end -->

Ta khởi tạo mảng $s$, trong đó $s[i]$ biểu thị tích của $i$ số đầu tiên.

When calling `add(num)`, we judge whether `num` is $0$. If it is, we set $s$ to `[1]`. Otherwise, we multiply the last element of $s$ by `num` and add the result to the end of $s$.

Khi gọi `getProduct(k)`, ta kiểm tra độ dài của $s$ có nhỏ hơn hoặc bằng $k$ hay không. Nếu có, trả về $0$. Nếu không, lấy phần tử cuối của $s$ chia cho phần tử thứ $k + 1$ tính từ cuối mảng, tức là $s[-1] / s[-k - 1]$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lần gọi `add`.

<!-- tabs:start -->

#### Python3

```python
class ProductOfNumbers:
    def __init__(self):
        self.s = [1]

    def add(self, num: int) -> None:
        if num == 0:
            self.s = [1]
            return
        self.s.append(self.s[-1] * num)

    def getProduct(self, k: int) -> int:
        return 0 if len(self.s) <= k else self.s[-1] // self.s[-k - 1]


# Your ProductOfNumbers object will be instantiated and called as such:
# obj = ProductOfNumbers()
# obj.add(num)
# param_2 = obj.getProduct(k)
```

#### Java

```java
class ProductOfNumbers {
    private List<Integer> s = new ArrayList<>();

    public ProductOfNumbers() {
        s.add(1);
    }

    public void add(int num) {
        if (num == 0) {
            s.clear();
            s.add(1);
            return;
        }
        s.add(s.get(s.size() - 1) * num);
    }

    public int getProduct(int k) {
        int n = s.size();
        return n <= k ? 0 : s.get(n - 1) / s.get(n - k - 1);
    }
}

/**
 * Your ProductOfNumbers object will be instantiated and called as such:
 * ProductOfNumbers obj = new ProductOfNumbers();
 * obj.add(num);
 * int param_2 = obj.getProduct(k);
 */
```

#### C++

```cpp
class ProductOfNumbers {
public:
    ProductOfNumbers() {
        s.push_back(1);
    }

    void add(int num) {
        if (num == 0) {
            s.clear();
            s.push_back(1);
            return;
        }
        s.push_back(s.back() * num);
    }

    int getProduct(int k) {
        int n = s.size();
        return n <= k ? 0 : s.back() / s[n - k - 1];
    }

private:
    vector<int> s;
};

/**
 * Your ProductOfNumbers object will be instantiated and called as such:
 * ProductOfNumbers* obj = new ProductOfNumbers();
 * obj->add(num);
 * int param_2 = obj->getProduct(k);
 */
```

#### Go

```go
type ProductOfNumbers struct {
	s []int
}

func Constructor() ProductOfNumbers {
	return ProductOfNumbers{[]int{1}}
}

func (this *ProductOfNumbers) Add(num int) {
	if num == 0 {
		this.s = []int{1}
		return
	}
	this.s = append(this.s, this.s[len(this.s)-1]*num)
}

func (this *ProductOfNumbers) GetProduct(k int) int {
	n := len(this.s)
	if n <= k {
		return 0
	}
	return this.s[len(this.s)-1] / this.s[len(this.s)-k-1]
}

/**
 * Your ProductOfNumbers object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Add(num);
 * param_2 := obj.GetProduct(k);
 */
```

#### TypeScript

```ts
class ProductOfNumbers {
    s = [1];

    add(num: number): void {
        if (num === 0) {
            this.s = [1];
        } else {
            const i = this.s.length;
            this.s[i] = this.s[i - 1] * num;
        }
    }

    getProduct(k: number): number {
        const i = this.s.length;
        if (k > i - 1) return 0;
        return this.s[i - 1] / this.s[i - k - 1];
    }
}
```

#### JavaScript

```js
class ProductOfNumbers {
    s = [1];

    add(num) {
        if (num === 0) {
            this.s = [1];
        } else {
            const i = this.s.length;
            this.s[i] = this.s[i - 1] * num;
        }
    }

    getProduct(k) {
        const i = this.s.length;
        if (k > i - 1) return 0;
        return this.s[i - 1] / this.s[i - k - 1];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
