---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Recursion
    - Math
---

<!-- problem:start -->

# [779. K-th Symbol in Grammar](https://leetcode.com/problems/k-th-symbol-in-grammar)

[中文文档](/solution/0700-0799/0779.K-th%20Symbol%20in%20Grammar/README.md)

## Mô tả

<!-- description:start -->

<p>Ta tạo một bảng gồm <code>n</code> hàng (đánh chỉ số từ <strong>1</strong>). Ban đầu, ghi <code>0</code> ở hàng <code>1<sup>st</sup></code>. Với mỗi hàng tiếp theo, dựa trên hàng trước đó và thay mỗi <code>0</code> bằng <code>01</code>, mỗi <code>1</code> bằng <code>10</code>.</p>

<ul>
	<li>Ví dụ, với <code>n = 3</code>, hàng <code>1<sup>st</sup></code> là <code>0</code>, hàng <code>2<sup>nd</sup></code> là <code>01</code>, và hàng <code>3<sup>rd</sup></code> là <code>0110</code>.</li>
</ul>

<p>Cho hai số nguyên <code>n</code> và <code>k</code>, hãy trả về ký hiệu thứ <code>k<sup>th</sup></code> (đánh chỉ số từ <strong>1</strong>) trong hàng <code>n<sup>th</sup></code> của bảng gồm <code>n</code> hàng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, k = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> hàng 1: <u>0</u>
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, k = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> 
hàng 1: 0
hàng 2: <u>0</u>1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, k = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> 
hàng 1: 0
hàng 2: 0<u>1</u>
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 30</code></li>
	<li><code>1 &lt;= k &lt;= 2<sup>n - 1</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Ở hàng $n$, thay $0\to 01$ và $1\to 10$. Không thể tạo toàn bộ $2^{n-1}$ bit.
>
> Nửa đầu sao chép hàng $n-1$; nửa sau là hàng đó sau khi đảo bit. Đệ quy trên nửa chứa $k$; nếu ở nửa sau thì xor với $1$.

<!-- thinking:end -->

Trước tiên, hãy quan sát quy luật của một vài hàng đầu tiên:

```
n = 1: 0
n = 2: 0 1
n = 3: 0 1 1 0
n = 4: 0 1 1 0 1 0 0 1
n = 5: 0 1 1 0 1 0 0 1 1 0 0 1 0 1 1 0
...
```

Ta thấy nửa đầu của mỗi hàng giống hệt hàng trước đó, còn nửa sau là phần đảo của hàng trước. Ở đây, "đảo" nghĩa là đổi $0$ thành $1$ và $1$ thành $0$.

Nếu $k$ nằm ở nửa đầu, ký tự thứ $k$ giống ký tự thứ $k$ của hàng trước, nên ta có thể gọi đệ quy trực tiếp với $kthGrammar(n - 1, k)$.

Nếu $k$ nằm ở nửa sau, ký tự thứ $k$ là phần đảo của ký tự thứ $(k - 2^{n - 2})$ ở hàng trước, tức là $kthGrammar(n - 1, k - 2^{n - 2}) \oplus 1$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthGrammar(self, n: int, k: int) -> int:
        if n == 1:
            return 0
        if k <= (1 << (n - 2)):
            return self.kthGrammar(n - 1, k)
        return self.kthGrammar(n - 1, k - (1 << (n - 2))) ^ 1
```

#### Java

```java
class Solution {
    public int kthGrammar(int n, int k) {
        if (n == 1) {
            return 0;
        }
        if (k <= (1 << (n - 2))) {
            return kthGrammar(n - 1, k);
        }
        return kthGrammar(n - 1, k - (1 << (n - 2))) ^ 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int kthGrammar(int n, int k) {
        if (n == 1) return 0;
        if (k <= (1 << (n - 2))) return kthGrammar(n - 1, k);
        return kthGrammar(n - 1, k - (1 << (n - 2))) ^ 1;
    }
};
```

#### Go

```go
func kthGrammar(n int, k int) int {
	if n == 1 {
		return 0
	}
	if k <= (1 << (n - 2)) {
		return kthGrammar(n-1, k)
	}
	return kthGrammar(n-1, k-(1<<(n-2))) ^ 1
}
```

#### TypeScript

```ts
function kthGrammar(n: number, k: number): number {
    if (n == 1) {
        return 0;
    }
    if (k <= 1 << (n - 2)) {
        return kthGrammar(n - 1, k);
    }
    return kthGrammar(n - 1, k - (1 << (n - 2))) ^ 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thao tác bit + Nhận xét

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 vẫn đi qua $n$ mức. Với chỉ số bắt đầu từ $0$ là $k-1$, mỗi nhánh con có chỉ số lẻ sẽ đảo bit, nên đáp án là tính chẵn lẻ của số bit $1$.
>
> $(k-1).\textit{bit\_count}\bmod 2$, không phụ thuộc vào $n$ miễn là hàng đủ dài.

<!-- thinking:end -->

Trong đề bài, chỉ số bắt đầu từ $1$. Ta đổi $k$ thành $k-1$ để chuyển sang chỉ số bắt đầu từ $0$. Trong phần thảo luận tiếp theo, mọi chỉ số đều bắt đầu từ $0$.

Quan sát kỹ hơn, ký tự thứ $i$ trong một hàng tạo ra hai ký tự ở vị trí $2i$ và $2i+1$ trong hàng tiếp theo.

```
0 1 1 0 1 0 0 1 1 0 0 1 0 1 1 0
```

Nếu ký tự thứ $i$ là $0$, các ký tự được tạo ở vị trí $2i$ và $2i+1$ lần lượt là $0$ và $1$. Nếu ký tự thứ $i$ là $1$, các ký tự được tạo ra lần lượt là $1$ và $0$.

```
0 1 1 0 1 0 0 1 1 0 0 1 0 1 1 0
      ^     * *
```

```
0 1 1 0 1 0 0 1 1 0 0 1 0 1 1 0
        ^       * *
```

Ta thấy ký tự ở vị trí $2i$ (chỉ số chẵn) luôn giống ký tự ở vị trí $i$, còn ký tự ở vị trí $2i+1$ (chỉ số lẻ) là phần đảo của ký tự ở vị trí $i$. Nói cách khác, ký tự tại chỉ số lẻ luôn trải qua một lần đảo. Nếu số lần đảo là chẵn, ký tự giữ nguyên; nếu là lẻ, kết quả tương đương với đảo một lần.

Vì vậy, ta chỉ cần kiểm tra $k$ có lẻ hay không. Nếu có, cộng thêm một lần đảo. Sau đó chia $k$ cho $2$ rồi tiếp tục kiểm tra, tích lũy số lần đảo cho đến khi $k$ bằng $0$.

Cuối cùng, ta xác định số lần đảo là chẵn hay lẻ. Nếu lẻ, đáp án là $1$; nếu không, đáp án là $0$.

Việc tích lũy số lần đảo về bản chất tương đương với đếm số bit $1$ trong biểu diễn nhị phân của $k$.

Độ phức tạp thời gian là $O(\log k)$, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthGrammar(self, n: int, k: int) -> int:
        return (k - 1).bit_count() & 1
```

#### Java

```java
class Solution {
    public int kthGrammar(int n, int k) {
        return Integer.bitCount(k - 1) & 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int kthGrammar(int n, int k) {
        return __builtin_popcount(k - 1) & 1;
    }
};
```

#### Go

```go
func kthGrammar(n int, k int) int {
	return bits.OnesCount(uint(k-1)) & 1
}
```

#### TypeScript

```ts
function kthGrammar(n: number, k: number): number {
    return bitCount(k - 1) & 1;
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
