---
comments: true
difficulty: Medium
tags:
    - Design
    - Array
    - Counting
    - Iterator
---

<!-- problem:start -->

# [900. RLE Iterator](https://leetcode.com/problems/rle-iterator)

[中文文档](/solution/0900-0999/0900.RLE%20Iterator/README.md)

## Mô tả

<!-- description:start -->

<p>Ta có thể dùng run-length encoding (tức <strong>RLE</strong>) để mã hóa một dãy số nguyên. Trong mảng đã mã hóa bằng run-length encoding có độ dài chẵn <code>encoding</code> (đánh chỉ số từ <strong>0</strong>), với mọi <code>i</code> chẵn, <code>encoding[i]</code> cho biết giá trị nguyên không âm <code>encoding[i + 1]</code> được lặp lại bao nhiêu lần trong dãy.</p>

<ul>
	<li>Ví dụ, dãy <code>arr = [8,8,8,5,5]</code> có thể được mã hóa thành <code>encoding = [3,8,2,5]</code>. <code>encoding = [3,8,0,9,2,5]</code> và <code>encoding = [2,8,1,8,2,5]</code> cũng là các dạng <strong>RLE</strong> hợp lệ của <code>arr</code>.</li>
</ul>

<p>Cho một mảng được mã hóa bằng run-length encoding, hãy thiết kế iterator để duyệt qua mảng đó.</p>

<p>Triển khai class <code>RLEIterator</code>:</p>

<ul>
	<li><code>RLEIterator(int[] encoded)</code> khởi tạo object với mảng đã mã hóa <code>encoded</code>.</li>
	<li><code>int next(int n)</code> tiêu thụ <code>n</code> phần tử tiếp theo và trả về phần tử cuối cùng được tiêu thụ. Nếu không còn đủ phần tử để tiêu thụ, trả về <code>-1</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;RLEIterator&quot;, &quot;next&quot;, &quot;next&quot;, &quot;next&quot;, &quot;next&quot;]
[[[3, 8, 0, 9, 2, 5]], [2], [1], [1], [2]]
<strong>Đầu ra</strong>
[null, 8, 8, 5, -1]

<strong>Giải thích</strong>
RLEIterator rLEIterator = new RLEIterator([3, 8, 0, 9, 2, 5]); // This maps to the sequence [8,8,8,5,5].
rLEIterator.next(2); // exhausts 2 terms of the sequence, returning 8. The remaining sequence is now [8, 5, 5].
rLEIterator.next(1); // exhausts 1 term of the sequence, returning 8. The remaining sequence is now [5, 5].
rLEIterator.next(1); // exhausts 1 term of the sequence, returning 5. The remaining sequence is now [5].
rLEIterator.next(2); // exhausts 2 terms, returning -1. This is because the first term exhausted was 5,
but the second term did not exist. Since the last term exhausted does not exist, we return -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= encoding.length &lt;= 1000</code></li>
	<li><code>encoding.length</code> là số chẵn.</li>
	<li><code>0 &lt;= encoding[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
	<li>Sẽ có tối đa <code>1000</code> lần gọi <code>next</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duy trì hai pointer

<!-- thinking:start -->

> **Tư duy**
>
> Số lần lặp có thể lên đến $10^9$, nên không thể giải mã encoding thành một danh sách đầy đủ; tiêu thụ từng phần tử một khi $n$ lớn như vậy cũng không khả thi.
>
> Vì vậy, ta tiêu thụ trực tiếp trên dạng nén. Số phần tử còn lại trong nhóm hiện tại là $\textit{encoding}[i]-j$; nếu nhỏ hơn $n$, bỏ qua cả nhóm và tăng $i$ thêm $2$, nếu không thì chỉ tăng $j$. Cả hai pointer đều chỉ tiến về phía trước, nên tổng cộng các query chỉ duyệt encoding một lần.

<!-- thinking:end -->

Ta định nghĩa hai pointer $i$ và $j$: $i$ trỏ đến nhóm run-length encoding hiện tại, còn $j$ cho biết đã đọc bao nhiêu phần tử trong nhóm đó. Ban đầu, $i = 0$, $j = 0$.

Mỗi lần gọi `next(n)`, ta kiểm tra số phần tử còn lại trong nhóm hiện tại, $encoding[i] - j$, có nhỏ hơn $n$ hay không. Nếu có, trừ số phần tử này khỏi $n$, tăng $i$ thêm $2$, đặt $j$ về $0$, rồi tiếp tục kiểm tra nhóm kế tiếp. Nếu không, tăng $j$ thêm $n$ và trả về $encoding[i + 1]$.

Nếu $i$ vượt quá độ dài của encoding mà vẫn chưa có giá trị trả về, nghĩa là không còn phần tử nào để tiêu thụ; khi đó ta trả về $-1$.

Độ phức tạp thời gian là $O(n + q)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của encoding, còn $q$ là số lần gọi `next(n)`.

<!-- tabs:start -->

#### Python3

```python
class RLEIterator:
    def __init__(self, encoding: List[int]):
        self.encoding = encoding
        self.i = 0
        self.j = 0

    def next(self, n: int) -> int:
        while self.i < len(self.encoding):
            if self.encoding[self.i] - self.j < n:
                n -= self.encoding[self.i] - self.j
                self.i += 2
                self.j = 0
            else:
                self.j += n
                return self.encoding[self.i + 1]
        return -1


# Your RLEIterator object will be instantiated and called as such:
# obj = RLEIterator(encoding)
# param_1 = obj.next(n)
```

#### Java

```java
class RLEIterator {
    private int[] encoding;
    private int i;
    private int j;

    public RLEIterator(int[] encoding) {
        this.encoding = encoding;
    }

    public int next(int n) {
        while (i < encoding.length) {
            if (encoding[i] - j < n) {
                n -= (encoding[i] - j);
                i += 2;
                j = 0;
            } else {
                j += n;
                return encoding[i + 1];
            }
        }
        return -1;
    }
}

/**
 * Your RLEIterator object will be instantiated and called as such:
 * RLEIterator obj = new RLEIterator(encoding);
 * int param_1 = obj.next(n);
 */
```

#### C++

```cpp
class RLEIterator {
public:
    RLEIterator(vector<int>& encoding) {
        this->encoding = encoding;
    }

    int next(int n) {
        while (i < encoding.size()) {
            if (encoding[i] - j < n) {
                n -= (encoding[i] - j);
                i += 2;
                j = 0;
            } else {
                j += n;
                return encoding[i + 1];
            }
        }
        return -1;
    }

private:
    vector<int> encoding;
    int i = 0;
    int j = 0;
};

/**
 * Your RLEIterator object will be instantiated and called as such:
 * RLEIterator* obj = new RLEIterator(encoding);
 * int param_1 = obj->next(n);
 */
```

#### Go

```go
type RLEIterator struct {
	encoding []int
	i, j     int
}

func Constructor(encoding []int) RLEIterator {
	return RLEIterator{encoding, 0, 0}
}

func (this *RLEIterator) Next(n int) int {
	for this.i < len(this.encoding) {
		if this.encoding[this.i]-this.j < n {
			n -= (this.encoding[this.i] - this.j)
			this.i += 2
			this.j = 0
		} else {
			this.j += n
			return this.encoding[this.i+1]
		}
	}
	return -1
}

/**
 * Your RLEIterator object will be instantiated and called as such:
 * obj := Constructor(encoding);
 * param_1 := obj.Next(n);
 */
```

#### TypeScript

```ts
class RLEIterator {
    private encoding: number[];
    private i: number;
    private j: number;

    constructor(encoding: number[]) {
        this.encoding = encoding;
        this.i = 0;
        this.j = 0;
    }

    next(n: number): number {
        while (this.i < this.encoding.length) {
            if (this.encoding[this.i] - this.j < n) {
                n -= this.encoding[this.i] - this.j;
                this.i += 2;
                this.j = 0;
            } else {
                this.j += n;
                return this.encoding[this.i + 1];
            }
        }
        return -1;
    }
}

/**
 * Your RLEIterator object will be instantiated and called as such:
 * var obj = new RLEIterator(encoding)
 * var param_1 = obj.next(n)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
