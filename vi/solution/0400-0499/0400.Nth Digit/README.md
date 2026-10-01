---
comments: true
difficulty: Medium
tags:
    - Math
    - Binary Search
---

<!-- problem:start -->

# [400. Nth Digit](https://leetcode.com/problems/nth-digit)

[中文文档](/solution/0400-0499/0400.Nth%20Digit/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy trả về chữ số thứ <code>n<sup>th</sup></code> trong dãy số nguyên vô hạn <code>[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> 3
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 11
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Chữ số thứ 11 trong dãy 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... là 0, thuộc số 10.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Không thể tạo chuỗi vô hạn bằng cách nối các số rồi truy cập ký tự thứ $n$, vì $n$ có thể lên tới $2^{31}-1$.
>
> Có thể tính trực tiếp số lượng số có $k$ chữ số: có $9\times 10^{k-1}$ số, tương ứng $k\times 9\times 10^{k-1}$ chữ số. Trừ lần lượt số chữ số của từng nhóm và tăng $k$ cho đến khi vị trí $n$ nằm trong nhóm hiện tại, sau đó xác định số nguyên và vị trí chữ số bên trong số đó.
>
> Cần trừ hết các nhóm trước khi xác định số cụ thể; nếu không, vị trí trong toàn dãy chưa thể chuyển thành chỉ số bên trong một số nguyên.

<!-- thinking:end -->

Số nguyên nhỏ nhất và lớn nhất có $k$ chữ số lần lượt là $10^{k-1}$ và $10^k-1$, vì vậy tổng số chữ số của các số có $k$ chữ số là $k \times 9 \times 10^{k-1}$.

Ta dùng $k$ biểu thị số chữ số của nhóm số hiện tại, còn $cnt$ biểu thị số lượng số trong nhóm đó. Ban đầu, $k=1$, $cnt=9$.

Mỗi lần, ta trừ $cnt \times k$ khỏi $n$. Khi $n$ nhỏ hơn hoặc bằng $cnt \times k$, chữ số cần tìm nằm trong nhóm các số có số chữ số hiện tại, và ta có thể xác định số tương ứng.

Cụ thể, trước tiên xác định số thứ mấy trong nhóm hiện tại chứa vị trí $n$, sau đó xác định chữ số ở vị trí nào trong số đó để lấy chữ số cần tìm.

Độ phức tạp thời gian là $O(\log_{10} n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findNthDigit(self, n: int) -> int:
        k, cnt = 1, 9
        while k * cnt < n:
            n -= k * cnt
            k += 1
            cnt *= 10
        num = 10 ** (k - 1) + (n - 1) // k
        idx = (n - 1) % k
        return int(str(num)[idx])
```

#### Java

```java
class Solution {
    public int findNthDigit(int n) {
        int k = 1, cnt = 9;
        while ((long) k * cnt < n) {
            n -= k * cnt;
            ++k;
            cnt *= 10;
        }
        int num = (int) Math.pow(10, k - 1) + (n - 1) / k;
        int idx = (n - 1) % k;
        return String.valueOf(num).charAt(idx) - '0';
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findNthDigit(int n) {
        int k = 1, cnt = 9;
        while (1ll * k * cnt < n) {
            n -= k * cnt;
            ++k;
            cnt *= 10;
        }
        int num = pow(10, k - 1) + (n - 1) / k;
        int idx = (n - 1) % k;
        return to_string(num)[idx] - '0';
    }
};
```

#### Go

```go
func findNthDigit(n int) int {
	k, cnt := 1, 9
	for k*cnt < n {
		n -= k * cnt
		k++
		cnt *= 10
	}
	num := int(math.Pow10(k-1)) + (n-1)/k
	idx := (n - 1) % k
	return int(strconv.Itoa(num)[idx] - '0')
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */
var findNthDigit = function (n) {
    let k = 1,
        cnt = 9;
    while (k * cnt < n) {
        n -= k * cnt;
        ++k;
        cnt *= 10;
    }
    const num = Math.pow(10, k - 1) + (n - 1) / k;
    const idx = (n - 1) % k;
    return num.toString()[idx];
};
```

#### C#

```cs
public class Solution {
    public int FindNthDigit(int n) {
        int k = 1, cnt = 9;
        while ((long) k * cnt < n) {
            n -= k * cnt;
            ++k;
            cnt *= 10;
        }
        int num = (int) Math.Pow(10, k - 1) + (n - 1) / k;
        int idx = (n - 1) % k;
        return num.ToString()[idx] - '0';
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
