---
comments: true
difficulty: Easy
rating: 1305
source: Biweekly Contest 160 Q1
tags:
    - Math
    - String
---

<!-- problem:start -->

# [3602. Hexadecimal and Hexatrigesimal Conversion](https://leetcode.com/problems/hexadecimal-and-hexatrigesimal-conversion)

[Tài liệu tiếng Trung](/solution/3600-3699/3602.Hexadecimal%20and%20Hexatrigesimal%20Conversion/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>.</p>

<p>Hãy trả về chuỗi nối giữa biểu diễn <strong>thập lục phân</strong> của <code>n<sup>2</sup></code> và biểu diễn <strong>hệ cơ số 36</strong> của <code>n<sup>3</sup></code>.</p>

<p>Một số <strong>thập lục phân</strong> là hệ thống số cơ số 16, sử dụng các chữ số <code>0 &ndash; 9</code> và các chữ cái in hoa <code>A - F</code> để biểu diễn các giá trị từ 0 đến 15.</p>

<p>Một số <strong>hệ cơ số 36</strong> là hệ thống số cơ số 36, sử dụng các chữ số <code>0 &ndash; 9</code> và các chữ cái in hoa <code>A - Z</code> để biểu diễn các giá trị từ 0 đến 35.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 13</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;A91P1&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><code>n<sup>2</sup> = 13 * 13 = 169</code>. Ở hệ thập lục phân, số này được chuyển thành <code>(10 * 16) + 9 = 169</code>, tương ứng với <code>&quot;A9&quot;</code>.</li>
    <li><code>n<sup>3</sup> = 13 * 13 * 13 = 2197</code>. Ở hệ cơ số 36, số này được chuyển thành <code>(1 * 36<sup>2</sup>) + (25 * 36) + 1 = 2197</code>, tương ứng với <code>&quot;1P1&quot;</code>.</li>
    <li>Nối hai kết quả ta được <code>&quot;A9&quot; + &quot;1P1&quot; = &quot;A91P1&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 36</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;5101000&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><code>n<sup>2</sup> = 36 * 36 = 1296</code>. Ở hệ thập lục phân, số này được chuyển thành <code>(5 * 16<sup>2</sup>) + (1 * 16) + 0 = 1296</code>, tương ứng với <code>&quot;510&quot;</code>.</li>
    <li><code>n<sup>3</sup> = 36 * 36 * 36 = 46656</code>. Ở hệ cơ số 36, số này được chuyển thành <code>(1 * 36<sup>3</sup>) + (0 * 36<sup>2</sup>) + (0 * 36) + 0 = 46656</code>, tương ứng với <code>&quot;1000&quot;</code>.</li>
    <li>Nối hai kết quả ta được <code>&quot;510&quot; + &quot;1000&quot; = &quot;5101000&quot;</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần biểu diễn $n^2$ ở cơ số $16$ và $n^3$ ở cơ số $36$. Các bộ chuyển đổi tích hợp thường chỉ hỗ trợ hệ thập lục phân, nên nếu không tự xử lý thì hai cơ số sẽ được chuyển đổi theo cách khác nhau.
>
> Mọi cơ số đều có thể được chuyển đổi bằng cách liên tục lấy phần dư và chia. Ta xây dựng hàm $f(x,k)$: nếu phần dư không quá $9$ thì ghi một chữ số, ngược lại ghi một chữ cái, sau đó đảo chuỗi các chữ số từ hàng thấp lên.
>
> Với $x=n^2$ và $y=n^3$, ta trả về $f(x,16)+f(y,36)$. Số vòng lặp có độ dài logarit theo $n$.

<!-- thinking:end -->

Ta định nghĩa hàm $\textit{f}(x, k)$ để chuyển một số nguyên $x$ thành biểu diễn chuỗi ở cơ số $k$. Hàm này xây dựng chuỗi kết quả bằng cách liên tục lấy phần dư và chia.

Với số nguyên $n$, ta tính $n^2$ và $n^3$, sau đó lần lượt chuyển chúng thành chuỗi hệ thập lục phân và chuỗi hệ cơ số 36. Cuối cùng, ta nối hai chuỗi này và trả về kết quả.

Độ phức tạp thời gian là $O(\log n)$, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def concatHex36(self, n: int) -> str:
        def f(x: int, k: int) -> str:
            res = []
            while x:
                v = x % k
                if v <= 9:
                    res.append(str(v))
                else:
                    res.append(chr(ord("A") + v - 10))
                x //= k
            return "".join(res[::-1])

        x, y = n**2, n**3
        return f(x, 16) + f(y, 36)
```

#### Java

```java
class Solution {
    public String concatHex36(int n) {
        int x = n * n;
        int y = n * n * n;
        return f(x, 16) + f(y, 36);
    }

    private String f(int x, int k) {
        StringBuilder res = new StringBuilder();
        while (x > 0) {
            int v = x % k;
            if (v <= 9) {
                res.append((char) ('0' + v));
            } else {
                res.append((char) ('A' + v - 10));
            }
            x /= k;
        }
        return res.reverse().toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string concatHex36(int n) {
        int x = n * n;
        int y = n * n * n;
        return f(x, 16) + f(y, 36);
    }

private:
    string f(int x, int k) {
        string res;
        while (x > 0) {
            int v = x % k;
            if (v <= 9) {
                res += char('0' + v);
            } else {
                res += char('A' + v - 10);
            }
            x /= k;
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

#### Go

```go
func concatHex36(n int) string {
    x := n * n
    y := n * n * n
    return f(x, 16) + f(y, 36)
}

func f(x, k int) string {
    res := []byte{}
    for x > 0 {
        v := x % k
        if v <= 9 {
            res = append(res, byte('0'+v))
        } else {
            res = append(res, byte('A'+v-10))
        }
        x /= k
    }
    for i, j := 0, len(res)-1; i < j; i, j = i+1, j-1 {
        res[i], res[j] = res[j], res[i]
    }
    return string(res)
}
```

#### TypeScript

```ts
function concatHex36(n: number): string {
    function f(x: number, k: number): string {
        const digits = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ';
        let res = '';
        while (x > 0) {
            const v = x % k;
            res = digits[v] + res;
            x = Math.floor(x / k);
        }
        return res;
    }

    const x = n * n;
    const y = n * n * n;
    return f(x, 16) + f(y, 36);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
