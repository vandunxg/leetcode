---
comments: true
difficulty: Medium
rating: 2260
source: Weekly Contest 257 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1997. First Day Where You Have Been in All the Rooms](https://leetcode.com/problems/first-day-where-you-have-been-in-all-the-rooms)

[中文文档](/solution/1900-1999/1997.First%20Day%20Where%20You%20Have%20Been%20in%20All%20the%20Rooms/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> phòng cần ghé thăm, được đánh số từ <code>0</code> đến <code>n - 1</code>. Mỗi ngày được đánh số, bắt đầu từ <code>0</code>. Mỗi ngày bạn sẽ đi vào một phòng.</p>

<p>Ban đầu, vào ngày <code>0</code>, bạn ghé thăm phòng <code>0</code>. <strong>Thứ tự</strong> ghé thăm các phòng trong những ngày tiếp theo được xác định bởi các <strong>quy tắc</strong> sau và một mảng <code>nextVisit</code> có độ dài <code>n</code>, được <strong>đánh chỉ số từ 0</strong>:</p>

<ul>
	<li>Giả sử trong một ngày, bạn ghé thăm phòng <code>i</code>,</li>
	<li>nếu bạn đã ở trong phòng <code>i</code> một số lần <strong>lẻ</strong> (<strong>tính cả</strong> lần ghé thăm hiện tại), thì vào ngày <strong>tiếp theo</strong>, bạn sẽ ghé thăm phòng có số hiệu <strong>nhỏ hơn hoặc bằng</strong> được chỉ định bởi <code>nextVisit[i]</code>, trong đó <code>0 &lt;= nextVisit[i] &lt;= i</code>;</li>
	<li>nếu bạn đã ở trong phòng <code>i</code> một số lần <strong>chẵn</strong> (<strong>tính cả</strong> lần ghé thăm hiện tại), thì vào ngày <strong>tiếp theo</strong>, bạn sẽ ghé thăm phòng <code>(i + 1) mod n</code>.</li>
</ul>

<p>Hãy trả về <em>số hiệu của <strong>ngày đầu tiên</strong> mà bạn đã ghé thăm <strong>tất cả</strong> các phòng</em>. Có thể chứng minh rằng ngày đó luôn tồn tại. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>lấy modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nextVisit = [0,0]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
- Vào ngày 0, bạn ghé thăm phòng 0. Tổng số lần bạn đã ở trong phòng 0 là 1, tức là số lẻ.
&nbsp; Ngày tiếp theo, bạn sẽ ghé thăm phòng nextVisit[0] = 0
- Vào ngày 1, bạn ghé thăm phòng 0. Tổng số lần bạn đã ở trong phòng 0 là 2, tức là số chẵn.
&nbsp; Ngày tiếp theo, bạn sẽ ghé thăm phòng (0 + 1) mod 2 = 1
- Vào ngày 2, bạn ghé thăm phòng 1. Đây là ngày đầu tiên bạn đã ghé thăm tất cả các phòng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nextVisit = [0,0,2]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
Thứ tự ghé thăm các phòng trong từng ngày là: [0,0,1,0,0,1,2,...].
Ngày 6 là ngày đầu tiên bạn đã ghé thăm tất cả các phòng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nextVisit = [0,1,2,0]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
Thứ tự ghé thăm các phòng trong từng ngày là: [0,0,1,1,2,2,3,...].
Ngày 6 là ngày đầu tiên bạn đã ghé thăm tất cả các phòng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nextVisit.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nextVisit[i] &lt;= i</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Tính chẵn lẻ của số lần ghé thăm sẽ đưa ta đến $nextVisit[i]$ hoặc $i+1$, còn số hiệu ngày có thể lên đến $10^9$. Ngày đầu tiên $f[i]$ ta bước vào phòng $i$ chỉ phụ thuộc vào các chỉ số nhỏ hơn.
>
> Sau lần đầu tiên ghé thăm phòng $i-1$, ta quay lại $nextVisit[i-1]$; khoảng thời gian ở giữa lặp lại timeline trước đó, tốn $f[i-1]-f[nextVisit[i-1]]$ ngày cộng thêm hai ngày.
>
> Từ đó, ta có một công thức truy hồi tuyến tính lấy modulo $10^9+7$ để tính $f[n-1]$.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là số hiệu ngày ghé thăm phòng thứ $i$ lần đầu tiên, nên đáp án là $f[n - 1]$.

Xét số hiệu ngày lần đầu tiên đến phòng thứ $(i-1)$, ký hiệu là $f[i-1]$. Lúc này, ta mất một ngày để quay lại phòng thứ $nextVisit[i-1]$. Tại sao phải quay lại? Vì bài toán giới hạn $0 \leq nextVisit[i] \leq i$.

Sau khi quay lại, phòng thứ $nextVisit[i-1]$ được ghé thăm một số lần lẻ, còn các phòng từ $nextVisit[i-1]+1$ đến $i-1$ được ghé thăm một số lần chẵn. Lúc này, ta đi đến phòng thứ $(i-1)$ một lần nữa từ phòng thứ $nextVisit[i-1]$, mất $f[i-1] - f[nextVisit[i-1]]$ ngày, sau đó mất thêm một ngày để đến phòng thứ $i$. Do đó, $f[i] = f[i-1] + 1 + f[i-1] - f[nextVisit[i-1]] + 1$. Vì $f[i]$ có thể rất lớn, ta cần lấy phần dư của $10^9 + 7$ và để tránh số âm, ta cần cộng thêm $10^9 + 7$.

Cuối cùng, trả về $f[n-1]$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng phòng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def firstDayBeenInAllRooms(self, nextVisit: List[int]) -> int:
        n = len(nextVisit)
        f = [0] * n
        mod = 10**9 + 7
        for i in range(1, n):
            f[i] = (f[i - 1] + 1 + f[i - 1] - f[nextVisit[i - 1]] + 1) % mod
        return f[-1]
```

#### Java

```java
class Solution {
    public int firstDayBeenInAllRooms(int[] nextVisit) {
        int n = nextVisit.length;
        long[] f = new long[n];
        final int mod = (int) 1e9 + 7;
        for (int i = 1; i < n; ++i) {
            f[i] = (f[i - 1] + 1 + f[i - 1] - f[nextVisit[i - 1]] + 1 + mod) % mod;
        }
        return (int) f[n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int firstDayBeenInAllRooms(vector<int>& nextVisit) {
        int n = nextVisit.size();
        vector<long long> f(n);
        const int mod = 1e9 + 7;
        for (int i = 1; i < n; ++i) {
            f[i] = (f[i - 1] + 1 + f[i - 1] - f[nextVisit[i - 1]] + 1 + mod) % mod;
        }
        return f[n - 1];
    }
};
```

#### Go

```go
func firstDayBeenInAllRooms(nextVisit []int) int {
	n := len(nextVisit)
	f := make([]int, n)
	const mod = 1e9 + 7
	for i := 1; i < n; i++ {
		f[i] = (f[i-1] + 1 + f[i-1] - f[nextVisit[i-1]] + 1 + mod) % mod
	}
	return f[n-1]
}
```

#### TypeScript

```ts
function firstDayBeenInAllRooms(nextVisit: number[]): number {
    const n = nextVisit.length;
    const mod = 1e9 + 7;
    const f: number[] = new Array<number>(n).fill(0);
    for (let i = 1; i < n; ++i) {
        f[i] = (f[i - 1] + 1 + f[i - 1] - f[nextVisit[i - 1]] + 1 + mod) % mod;
    }
    return f[n - 1];
}
```

#### C#

```cs
public class Solution {
    public int FirstDayBeenInAllRooms(int[] nextVisit) {
        int n = nextVisit.Length;
        long[] f = new long[n];
        int mod = (int)1e9 + 7;
        for (int i = 1; i < n; ++i) {
            f[i] = (f[i - 1] + 1 + f[i - 1] - f[nextVisit[i - 1]] + 1 + mod) % mod;
        }
        return (int)f[n - 1];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
