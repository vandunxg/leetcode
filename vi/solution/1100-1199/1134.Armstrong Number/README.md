---
comments: true
difficulty: Easy
rating: 1231
source: Biweekly Contest 5 Q2
tags:
    - Math
---

<!-- problem:start -->

# [1134. Armstrong Number 🔒](https://leetcode.com/problems/armstrong-number)

[中文文档](/solution/1100-1199/1134.Armstrong%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, trả về <code>true</code> <em>khi và chỉ khi đó là một <strong>số Armstrong</strong></em>.</p>

<p>Số có <code>k</code> chữ số <code>n</code> là số Armstrong khi và chỉ khi tổng lũy thừa bậc <code>k<sup>th</sup></code> của từng chữ số bằng <code>n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 153
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 153 là số có 3 chữ số, và 153 = 1<sup>3</sup> + 5<sup>3</sup> + 3<sup>3</sup>.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 123
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> 123 là số có 3 chữ số, và 123 != 1<sup>3</sup> + 2<sup>3</sup> + 3<sup>3</sup> = 36.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một số Armstrong bằng tổng từng chữ số được nâng lên lũy thừa $k$, với $k$ là số chữ số. Lấy $k$ từ độ dài biểu diễn thập phân, lần lượt tách chữ số bằng phép modulo và phép chia nguyên, rồi so sánh tổng lũy thừa với $n$. Số chữ số rất ít nên chỉ cần một vòng lặp trực tiếp.

<!-- thinking:end -->

Trước tiên, ta tính số chữ số $k$, sau đó tính tổng $s$ của lũy thừa bậc $k$ của từng chữ số, cuối cùng kiểm tra xem $s$ có bằng $n$ hay không.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là số đã cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isArmstrong(self, n: int) -> bool:
        k = len(str(n))
        s, x = 0, n
        while x:
            s += (x % 10) ** k
            x //= 10
        return s == n
```

#### Java

```java
class Solution {
    public boolean isArmstrong(int n) {
        int k = (n + "").length();
        int s = 0;
        for (int x = n; x > 0; x /= 10) {
            s += Math.pow(x % 10, k);
        }
        return s == n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isArmstrong(int n) {
        int k = to_string(n).size();
        int s = 0;
        for (int x = n; x; x /= 10) {
            s += pow(x % 10, k);
        }
        return s == n;
    }
};
```

#### Go

```go
func isArmstrong(n int) bool {
	k := 0
	for x := n; x > 0; x /= 10 {
		k++
	}
	s := 0
	for x := n; x > 0; x /= 10 {
		s += int(math.Pow(float64(x%10), float64(k)))
	}
	return s == n
}
```

#### TypeScript

```ts
function isArmstrong(n: number): boolean {
    const k = String(n).length;
    let s = 0;
    for (let x = n; x; x = Math.floor(x / 10)) {
        s += Math.pow(x % 10, k);
    }
    return s == n;
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {boolean}
 */
var isArmstrong = function (n) {
    const k = String(n).length;
    let s = 0;
    for (let x = n; x; x = Math.floor(x / 10)) {
        s += Math.pow(x % 10, k);
    }
    return s == n;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
