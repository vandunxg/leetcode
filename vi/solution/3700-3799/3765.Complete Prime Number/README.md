---
comments: true
difficulty: Medium
rating: 1378
source: Biweekly Contest 171 Q1
tags:
    - Math
    - Enumeration
    - Number Theory
---

<!-- problem:start -->

# [3765. Complete Prime Number](https://leetcode.com/problems/complete-prime-number)

[中文文档](/solution/3700-3799/3765.Complete%20Prime%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>num</code>.</p>

<p>Một số <code>num</code> được gọi là <strong>Số nguyên <span data-keyword="prime-number">tố hoàn chỉnh</span></strong> nếu mọi <strong>tiền tố</strong> và mọi <strong>hậu tố</strong> của <code>num</code> đều là <strong>số nguyên tố</strong>.</p>

<p>Trả về <code>true</code> nếu <code>num</code> là một Số nguyên tố hoàn chỉnh, ngược lại trả về <code>false</code>.</p>

<p><strong>Lưu ý</strong>:</p>

<ul>
	<li><strong>Tiền tố</strong> của một số được tạo bởi <code>k</code> chữ số <strong>đầu tiên</strong> của số đó.</li>
	<li><strong>Hậu tố</strong> của một số được tạo bởi <code>k</code> chữ số <strong>cuối cùng</strong> của số đó.</li>
	<li>Các số có một chữ số chỉ được xem là Số nguyên tố hoàn chỉnh nếu chúng là <strong>số nguyên tố</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num = 23</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>​​​​​​​</strong>Các tiền tố của <code>num = 23</code> là 2 và 23, cả hai đều là số nguyên tố.</li>
	<li>Các hậu tố của <code>num = 23</code> là 3 và 23, cả hai đều là số nguyên tố.</li>
	<li>Mọi tiền tố và hậu tố đều là số nguyên tố, nên 23 là một Số nguyên tố hoàn chỉnh và đáp án là <code>true</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num = 39</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các tiền tố của <code>num = 39</code> là 3 và 39. 3 là số nguyên tố, nhưng 39 thì không.</li>
	<li>Các hậu tố của <code>num = 39</code> là 9 và 39. Cả 9 và 39 đều không phải số nguyên tố.</li>
	<li>Có ít nhất một tiền tố hoặc hậu tố không phải số nguyên tố, nên 39 không phải là một Số nguyên tố hoàn chỉnh và đáp án là <code>false</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>7 là số nguyên tố, nên mọi tiền tố và hậu tố của nó đều là số nguyên tố và đáp án là <code>true</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Một số nguyên tố hoàn chỉnh cần có mọi tiền tố và hậu tố đều là số nguyên tố. Với $num\le 10^9$, chỉ có $O(\log n)$ tiền tố và hậu tố, mỗi số được kiểm tra trong $O(\sqrt{x})$. Tiền tố được tạo bằng cách nối các chữ số từ trái sang phải; hậu tố được tạo bằng cách tích lũy từ phải sang trái với giá trị hàng.

<!-- thinking:end -->

Ta định nghĩa hàm $\text{is\_prime}(x)$ để xác định một số $x$ có phải là số nguyên tố hay không. Cụ thể, nếu $x < 2$ thì $x$ không phải số nguyên tố; ngược lại, ta kiểm tra mọi số nguyên $i$ từ $2$ đến $\sqrt{x}$. Nếu tồn tại một $i$ chia hết cho $x$ thì $x$ không phải số nguyên tố; nếu không, $x$ là số nguyên tố.

Tiếp theo, ta chuyển số nguyên $\textit{num}$ thành một chuỗi $s$, rồi lần lượt kiểm tra số nguyên tương ứng với từng tiền tố và hậu tố của $s$ có phải là số nguyên tố hay không. Với tiền tố, ta xây dựng số nguyên $x$ từ trái sang phải; với hậu tố, ta xây dựng số nguyên $x$ từ phải sang trái. Nếu trong quá trình kiểm tra, ta phát hiện số nguyên tương ứng với một tiền tố hoặc hậu tố nào đó không phải là số nguyên tố, ta trả về $\text{false}$; nếu tất cả các số nguyên tương ứng với tiền tố và hậu tố đều là số nguyên tố, ta trả về $\text{true}$.

Độ phức tạp thời gian là $O(\sqrt{n} \times \log n)$, và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là giá trị của số nguyên $\textit{num}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def completePrime(self, num: int) -> bool:
        def is_prime(x: int) -> bool:
            if x < 2:
                return False
            return all(x % i for i in range(2, int(sqrt(x)) + 1))

        s = str(num)
        x = 0
        for c in s:
            x = x * 10 + int(c)
            if not is_prime(x):
                return False
        x, p = 0, 1
        for c in s[::-1]:
            x = p * int(c) + x
            p *= 10
            if not is_prime(x):
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean completePrime(int num) {
        char[] s = String.valueOf(num).toCharArray();
        int x = 0;
        for (int i = 0; i < s.length; i++) {
            x = x * 10 + (s[i] - '0');
            if (!isPrime(x)) {
                return false;
            }
        }
        x = 0;
        int p = 1;
        for (int i = s.length - 1; i >= 0; i--) {
            x = p * (s[i] - '0') + x;
            p *= 10;
            if (!isPrime(x)) {
                return false;
            }
        }
        return true;
    }

    private boolean isPrime(int x) {
        if (x < 2) {
            return false;
        }
        for (int i = 2; i * i <= x; i++) {
            if (x % i == 0) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool completePrime(int num) {
        auto isPrime = [&](int x) {
            if (x < 2) {
                return false;
            }
            for (int i = 2; i * i <= x; ++i) {
                if (x % i == 0) {
                    return false;
                }
            }
            return true;
        };

        string s = to_string(num);

        int x = 0;
        for (char c : s) {
            x = x * 10 + (c - '0');
            if (!isPrime(x)) {
                return false;
            }
        }

        x = 0;
        int p = 1;
        for (int i = (int) s.size() - 1; i >= 0; --i) {
            x = p * (s[i] - '0') + x;
            p *= 10;
            if (!isPrime(x)) {
                return false;
            }
        }

        return true;
    }
};
```

#### Go

```go
func completePrime(num int) bool {
    isPrime := func(x int) bool {
        if x < 2 {
            return false
        }
        for i := 2; i*i <= x; i++ {
            if x%i == 0 {
                return false
            }
        }
        return true
    }

    s := strconv.Itoa(num)

    x := 0
    for i := 0; i < len(s); i++ {
        x = x*10 + int(s[i]-'0')
        if !isPrime(x) {
            return false
        }
    }

    x = 0
    p := 1
    for i := len(s) - 1; i >= 0; i-- {
        x = p*int(s[i]-'0') + x
        p *= 10
        if !isPrime(x) {
            return false
        }
    }

    return true
}
```

#### TypeScript

```ts
function completePrime(num: number): boolean {
    const isPrime = (x: number): boolean => {
        if (x < 2) return false;
        for (let i = 2; i * i <= x; i++) {
            if (x % i === 0) {
                return false;
            }
        }
        return true;
    };

    const s = String(num);

    let x = 0;
    for (let i = 0; i < s.length; i++) {
        x = x * 10 + (s.charCodeAt(i) - 48);
        if (!isPrime(x)) {
            return false;
        }
    }

    x = 0;
    let p = 1;
    for (let i = s.length - 1; i >= 0; i--) {
        x = p * (s.charCodeAt(i) - 48) + x;
        p *= 10;
        if (!isPrime(x)) {
            return false;
        }
    }

    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
