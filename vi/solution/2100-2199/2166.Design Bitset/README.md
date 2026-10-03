---
comments: true
difficulty: Medium
rating: 1751
source: Weekly Contest 279 Q3
tags:
    - Design
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [2166. Design Bitset](https://leetcode.com/problems/design-bitset)

[Tài liệu tiếng Trung](/solution/2100-2199/2166.Design%20Bitset/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Bitset</strong> là một cấu trúc dữ liệu dùng để lưu trữ các bit một cách nhỏ gọn.</p>

<p>Hãy cài đặt class <code>Bitset</code>:</p>

<ul>
	<li><code>Bitset(int size)</code> Khởi tạo Bitset với <code>size</code> bit, tất cả đều bằng <code>0</code>.</li>
	<li><code>void fix(int idx)</code> Cập nhật giá trị của bit tại chỉ số <code>idx</code> thành <code>1</code>. Nếu giá trị đã là <code>1</code> thì không thay đổi.</li>
	<li><code>void unfix(int idx)</code> Cập nhật giá trị của bit tại chỉ số <code>idx</code> thành <code>0</code>. Nếu giá trị đã là <code>0</code> thì không thay đổi.</li>
	<li><code>void flip()</code> Đảo giá trị của từng bit trong Bitset. Nói cách khác, tất cả bit có giá trị <code>0</code> sẽ chuyển thành <code>1</code> và ngược lại.</li>
	<li><code>boolean all()</code> Kiểm tra xem giá trị của <strong>từng</strong> bit trong Bitset có phải là <code>1</code> hay không. Trả về <code>true</code> nếu thỏa điều kiện, ngược lại trả về <code>false</code>.</li>
	<li><code>boolean one()</code> Kiểm tra xem trong Bitset có <strong>ít nhất một</strong> bit có giá trị <code>1</code> hay không. Trả về <code>true</code> nếu thỏa điều kiện, ngược lại trả về <code>false</code>.</li>
	<li><code>int count()</code> Trả về <strong>tổng số</strong> bit trong Bitset có giá trị <code>1</code>.</li>
	<li><code>String toString()</code> Trả về trạng thái hiện tại của Bitset. Lưu ý rằng trong chuỗi kết quả, ký tự tại chỉ số <code>i<sup>th</sup></code> phải tương ứng với giá trị của bit thứ <code>i<sup>th</sup></code> trong Bitset.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input</strong>
[&quot;Bitset&quot;, &quot;fix&quot;, &quot;fix&quot;, &quot;flip&quot;, &quot;all&quot;, &quot;unfix&quot;, &quot;flip&quot;, &quot;one&quot;, &quot;unfix&quot;, &quot;count&quot;, &quot;toString&quot;]
[[5], [3], [1], [], [], [0], [], [], [0], [], []]
<strong>Output</strong>
[null, null, null, null, false, null, null, true, null, 2, &quot;01010&quot;]

<strong>Explanation</strong>
Bitset bs = new Bitset(5); // bitset = &quot;00000&quot;.
bs.fix(3);     // the value at idx = 3 is updated to 1, so bitset = &quot;00010&quot;.
bs.fix(1);     // the value at idx = 1 is updated to 1, so bitset = &quot;01010&quot;.
bs.flip();     // the value of each bit is flipped, so bitset = &quot;10101&quot;.
bs.all();      // return False, as not all values of the bitset are 1.
bs.unfix(0);   // the value at idx = 0 is updated to 0, so bitset = &quot;00101&quot;.
bs.flip();     // the value of each bit is flipped, so bitset = &quot;11010&quot;.
bs.one();      // return True, as there is at least 1 index with value 1.
bs.unfix(0);   // the value at idx = 0 is updated to 0, so bitset = &quot;01010&quot;.
bs.count();    // return 2, as there are 2 bits with value 1.
bs.toString(); // return &quot;01010&quot;, which is the composition of bitset.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= size &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= idx &lt;= size - 1</code></li>
	<li>Có nhiều nhất <code>10<sup>5</sup></code> lời gọi <strong>tổng cộng</strong> đến <code>fix</code>, <code>unfix</code>, <code>flip</code>, <code>all</code>, <code>one</code>, <code>count</code> và <code>toString</code>.</li>
	<li>Sẽ có ít nhất một lời gọi đến <code>all</code>, <code>one</code>, <code>count</code> hoặc <code>toString</code>.</li>
	<li>Sẽ có nhiều nhất <code>5</code> lời gọi đến <code>toString</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bitset phải hỗ trợ sửa một bit, đảo tất cả bit, đồng thời trả lời các truy vấn tất cả bit đều bằng 1 / có bit bằng 1 / đếm số bit 1 / chuyển thành chuỗi. Nếu đảo một mảng thông thường thì mất $O(n)$, quá chậm khi cả $n$ và số thao tác đều có thể đạt $10^5$.
>
> Duy trì chuỗi hiện tại $a$ và phần bù của nó $b$. Khi flip, chỉ cần hoán đổi chúng và thay $\textit{cnt}$ bằng $n-\textit{cnt}$. Các thao tác cập nhật từng vị trí sẽ đồng thời duy trì $a$, $b$ và $\textit{cnt}$.
>
> $\texttt{toString}$ nối các phần tử của $a$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Bitset:
    def __init__(self, size: int):
        self.a = ['0'] * size
        self.b = ['1'] * size
        self.cnt = 0

    def fix(self, idx: int) -> None:
        if self.a[idx] == '0':
            self.a[idx] = '1'
            self.cnt += 1
        self.b[idx] = '0'

    def unfix(self, idx: int) -> None:
        if self.a[idx] == '1':
            self.a[idx] = '0'
            self.cnt -= 1
        self.b[idx] = '1'

    def flip(self) -> None:
        self.a, self.b = self.b, self.a
        self.cnt = len(self.a) - self.cnt

    def all(self) -> bool:
        return self.cnt == len(self.a)

    def one(self) -> bool:
        return self.cnt > 0

    def count(self) -> int:
        return self.cnt

    def toString(self) -> str:
        return ''.join(self.a)


# Your Bitset object will be instantiated and called as such:
# obj = Bitset(size)
# obj.fix(idx)
# obj.unfix(idx)
# obj.flip()
# param_4 = obj.all()
# param_5 = obj.one()
# param_6 = obj.count()
# param_7 = obj.toString()
```

#### Java

```java
class Bitset {
    private char[] a;
    private char[] b;
    private int cnt;

    public Bitset(int size) {
        a = new char[size];
        b = new char[size];
        Arrays.fill(a, '0');
        Arrays.fill(b, '1');
    }

    public void fix(int idx) {
        if (a[idx] == '0') {
            a[idx] = '1';
            ++cnt;
        }
        b[idx] = '0';
    }

    public void unfix(int idx) {
        if (a[idx] == '1') {
            a[idx] = '0';
            --cnt;
        }
        b[idx] = '1';
    }

    public void flip() {
        char[] t = a;
        a = b;
        b = t;
        cnt = a.length - cnt;
    }

    public boolean all() {
        return cnt == a.length;
    }

    public boolean one() {
        return cnt > 0;
    }

    public int count() {
        return cnt;
    }

    public String toString() {
        return String.valueOf(a);
    }
}

/**
 * Your Bitset object will be instantiated and called as such:
 * Bitset obj = new Bitset(size);
 * obj.fix(idx);
 * obj.unfix(idx);
 * obj.flip();
 * boolean param_4 = obj.all();
 * boolean param_5 = obj.one();
 * int param_6 = obj.count();
 * String param_7 = obj.toString();
 */
```

#### C++

```cpp
class Bitset {
public:
    string a, b;
    int cnt = 0;

    Bitset(int size) {
        a = string(size, '0');
        b = string(size, '1');
    }

    void fix(int idx) {
        if (a[idx] == '0') a[idx] = '1', ++cnt;
        b[idx] = '0';
    }

    void unfix(int idx) {
        if (a[idx] == '1') a[idx] = '0', --cnt;
        b[idx] = '1';
    }

    void flip() {
        swap(a, b);
        cnt = a.size() - cnt;
    }

    bool all() {
        return cnt == a.size();
    }

    bool one() {
        return cnt > 0;
    }

    int count() {
        return cnt;
    }

    string toString() {
        return a;
    }
};

/**
 * Your Bitset object will be instantiated and called as such:
 * Bitset* obj = new Bitset(size);
 * obj->fix(idx);
 * obj->unfix(idx);
 * obj->flip();
 * bool param_4 = obj->all();
 * bool param_5 = obj->one();
 * int param_6 = obj->count();
 * string param_7 = obj->toString();
 */
```

#### Go

```go
type Bitset struct {
	a   []byte
	b   []byte
	cnt int
}

func Constructor(size int) Bitset {
	a := bytes.Repeat([]byte{'0'}, size)
	b := bytes.Repeat([]byte{'1'}, size)
	return Bitset{a, b, 0}
}

func (this *Bitset) Fix(idx int) {
	if this.a[idx] == '0' {
		this.a[idx] = '1'
		this.cnt++
	}
	this.b[idx] = '0'
}

func (this *Bitset) Unfix(idx int) {
	if this.a[idx] == '1' {
		this.a[idx] = '0'
		this.cnt--
	}
	this.b[idx] = '1'
}

func (this *Bitset) Flip() {
	this.a, this.b = this.b, this.a
	this.cnt = len(this.a) - this.cnt
}

func (this *Bitset) All() bool {
	return this.cnt == len(this.a)
}

func (this *Bitset) One() bool {
	return this.cnt > 0
}

func (this *Bitset) Count() int {
	return this.cnt
}

func (this *Bitset) ToString() string {
	return string(this.a)
}

/**
 * Your Bitset object will be instantiated and called as such:
 * obj := Constructor(size);
 * obj.Fix(idx);
 * obj.Unfix(idx);
 * obj.Flip();
 * param_4 := obj.All();
 * param_5 := obj.One();
 * param_6 := obj.Count();
 * param_7 := obj.ToString();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
