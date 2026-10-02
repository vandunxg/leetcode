---
comments: true
difficulty: Hard
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [1692. Count Ways to Distribute Candies 🔒](https://leetcode.com/problems/count-ways-to-distribute-candies)

[中文文档](/solution/1600-1699/1692.Count%20Ways%20to%20Distribute%20Candies/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> viên kẹo <strong>khác nhau</strong> (được đánh nhãn từ <code>1</code> đến <code>n</code>) và <code>k</code> túi. Bạn cần phân phối <strong>tất cả</strong> kẹo vào các túi sao cho mỗi túi có <strong>ít nhất</strong> một viên.</p>

<p>Có thể có nhiều cách phân phối kẹo. Hai cách được xem là <strong>khác nhau</strong> nếu các viên kẹo trong một túi ở cách thứ nhất không cùng nằm trong một túi ở cách thứ hai. Thứ tự của các túi và thứ tự của các viên kẹo trong mỗi túi không quan trọng.</p>

<p>Ví dụ, <code>(1), (2,3)</code> và <code>(2), (1,3)</code> được xem là khác nhau vì các viên kẹo <code>2</code> và <code>3</code> trong túi <code>(2,3)</code> ở cách thứ nhất không cùng nằm trong một túi ở cách thứ hai (chúng bị tách vào hai túi <code>(<u>2</u>)</code> và <code>(1,<u>3</u>)</code>). Tuy nhiên, <code>(1), (2,3)</code> và <code>(3,2), (1)</code> được xem là giống nhau vì các viên kẹo trong mỗi túi đều nằm cùng nhau ở cả hai cách.</p>

<p>Cho hai số nguyên <code>n</code> và <code>k</code>, hãy trả về <em><strong>số lượng</strong> cách khác nhau để phân phối kẹo</em>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>chia lấy dư</strong> cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1692.Count%20Ways%20to%20Distribute%20Candies/images/candies-1.png" style="height: 248px; width: 600px;" /></p>

<pre>
<strong>Đầu vào:</strong> n = 3, k = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 3 cách phân phối 3 viên kẹo vào 2 túi:
(1), (2,3)
(1,2), (3)
(1,3), (2)
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, k = 2
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Có 7 cách phân phối 4 viên kẹo vào 2 túi:
(1), (2,3,4)
(1,2), (3,4)
(1,3), (2,4)
(1,4), (2,3)
(1,2,3), (4)
(1,2,4), (3)
(1,3,4), (2)
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 20, k = 5
<strong>Đầu ra:</strong> 206085257
<strong>Giải thích:</strong> Có 1881780996 cách phân phối 20 viên kẹo vào 5 túi. 1881780996 chia lấy dư cho 10<sup>9</sup> + 7 bằng 206085257.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Phân phối $n$ viên kẹo vào $k$ túi không rỗng, không có nhãn, tương ứng với số Stirling loại hai. Vì $n,k \le 1000$, quy hoạch động hai chiều là phù hợp.
>
> $f[i][j]$ là số cách đặt $i$ viên kẹo vào $j$ túi: mở một túi mới với $f[i-1][j-1]$, hoặc đặt vào một trong $j$ túi hiện có với $f[i-1][j]\times j$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số cách khác nhau để phân phối $i$ viên kẹo vào $j$ túi. Ban đầu, $f[0][0]=1$, và đáp án là $f[n][k]$.

Ta xét cách phân phối viên kẹo thứ $i$. Nếu viên kẹo thứ $i$ được cho vào một túi mới, thì $f[i][j]=f[i-1][j-1]$. Nếu viên kẹo thứ $i$ được cho vào một túi hiện có, thì $f[i][j]=f[i-1][j]\times j$. Do đó, công thức chuyển trạng thái là:

$$
f[i][j]=f[i-1][j-1]+f[i-1][j]\times j
$$

Đáp án cuối cùng là $f[n][k]$.

Độ phức tạp thời gian là $O(n \times k)$, và độ phức tạp không gian là $O(n \times k)$. Ở đây, $n$ và $k$ lần lượt là số lượng viên kẹo và số lượng túi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def waysToDistribute(self, n: int, k: int) -> int:
        mod = 10**9 + 7
        f = [[0] * (k + 1) for _ in range(n + 1)]
        f[0][0] = 1
        for i in range(1, n + 1):
            for j in range(1, k + 1):
                f[i][j] = (f[i - 1][j] * j + f[i - 1][j - 1]) % mod
        return f[n][k]
```

#### Java

```java
class Solution {
    public int waysToDistribute(int n, int k) {
        final int mod = (int) 1e9 + 7;
        int[][] f = new int[n + 1][k + 1];
        f[0][0] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= k; j++) {
                f[i][j] = (int) ((long) f[i - 1][j] * j % mod + f[i - 1][j - 1]) % mod;
            }
        }
        return f[n][k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int waysToDistribute(int n, int k) {
        const int mod = 1e9 + 7;
        int f[n + 1][k + 1];
        memset(f, 0, sizeof(f));
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= k; ++j) {
                f[i][j] = (1LL * f[i - 1][j] * j + f[i - 1][j - 1]) % mod;
            }
        }
        return f[n][k];
    }
};
```

#### Go

```go
func waysToDistribute(n int, k int) int {
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, k+1)
	}
	f[0][0] = 1
	const mod = 1e9 + 7
	for i := 1; i <= n; i++ {
		for j := 1; j <= k; j++ {
			f[i][j] = (f[i-1][j]*j + f[i-1][j-1]) % mod
		}
	}
	return f[n][k]
}
```

#### TypeScript

```ts
function waysToDistribute(n: number, k: number): number {
    const mod = 10 ** 9 + 7;
    const f: number[][] = Array.from({ length: n + 1 }, () =>
        Array.from({ length: k + 1 }, () => 0),
    );
    f[0][0] = 1;
    for (let i = 1; i <= n; ++i) {
        for (let j = 1; j <= k; ++j) {
            f[i][j] = (f[i - 1][j] * j + f[i - 1][j - 1]) % mod;
        }
    }
    return f[n][k];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
