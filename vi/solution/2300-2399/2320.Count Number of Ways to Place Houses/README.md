---
comments: true
difficulty: Medium
rating: 1607
source: Weekly Contest 299 Q2
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [2320. Count Number of Ways to Place Houses](https://leetcode.com/problems/count-number-of-ways-to-place-houses)

[中文文档](/solution/2300-2399/2320.Count%20Number%20of%20Ways%20to%20Place%20Houses/README.md)

## Mô tả

<!-- description:start -->

<p>Có một con đường gồm <code>n * 2</code> <strong>ô đất</strong>, trong đó mỗi bên đường có <code>n</code> ô đất. Các ô đất ở mỗi bên được đánh số từ <code>1</code> đến <code>n</code>. Có thể xây một ngôi nhà trên mỗi ô đất.</p>

<p>Trả về <em>số cách đặt nhà sao cho không có hai ngôi nhà nào nằm cạnh nhau ở cùng một bên đường</em>. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Lưu ý rằng nếu một ngôi nhà được xây trên ô đất thứ <code>i<sup>th</sup></code> ở một bên đường, thì một ngôi nhà cũng có thể được xây trên ô đất thứ <code>i<sup>th</sup></code> ở bên đường còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Các cách sắp xếp có thể:
1. Tất cả các ô đất đều trống.
2. Một ngôi nhà được xây ở một bên đường.
3. Một ngôi nhà được xây ở bên đường còn lại.
4. Hai ngôi nhà được xây, mỗi bên đường một ngôi.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2320.Count%20Number%20of%20Ways%20to%20Place%20Houses/images/arrangements.png" style="width: 500px; height: 500px;" />
<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> 9 cách sắp xếp có thể được minh họa trong hình trên.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Hai bên đường độc lập, và các ô đất liền kề trên cùng một bên không thể đồng thời có nhà. Với $n \le 10^4$, đáp án là bình phương số cách của một bên. Khi duyệt một bên, ta chỉ cần biết ô đất trước đó có nhà hay không.
>
> Gọi $f[i]$ và $g[i]$ lần lượt là số cách lấp đầy $i+1$ ô đất đầu tiên khi ô cuối cùng có nhà hoặc để trống. Nếu ô cuối có nhà thì ô trước đó buộc phải trống; nếu ô cuối để trống thì ô trước đó có thể có nhà hoặc không. Nhân kết quả của hai bên theo modulo đã cho.

<!-- thinking:end -->

Vì cách đặt nhà ở hai bên đường không ảnh hưởng lẫn nhau, ta chỉ cần xét cách đặt nhà ở một bên, sau đó bình phương số cách của một bên để nhận được đáp án cuối cùng theo modulo.

Ta định nghĩa $f[i]$ là số cách đặt nhà trên $i+1$ ô đất đầu tiên, trong đó ô đất cuối cùng có nhà. Ta định nghĩa $g[i]$ là số cách đặt nhà trên $i+1$ ô đất đầu tiên, trong đó ô đất cuối cùng không có nhà. Ban đầu, $f[0] = g[0] = 1$.

Khi đặt nhà trên ô đất thứ $(i+1)$, có hai trường hợp:

- Nếu ô đất thứ $(i+1)$ có nhà, thì ô đất thứ $i$ không được có nhà, nên số cách là $f[i] = g[i-1]$;
- Nếu ô đất thứ $(i+1)$ không có nhà, thì ô đất thứ $i$ có thể có nhà hoặc không, nên số cách là $g[i] = f[i-1] + g[i-1]$.

Cuối cùng, ta bình phương $f[n-1] + g[n-1]$ theo modulo để nhận được đáp án.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó $n$ là độ dài của con đường.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countHousePlacements(self, n: int) -> int:
        mod = 10**9 + 7
        f = [1] * n
        g = [1] * n
        for i in range(1, n):
            f[i] = g[i - 1]
            g[i] = (f[i - 1] + g[i - 1]) % mod
        v = f[-1] + g[-1]
        return v * v % mod
```

#### Java

```java
class Solution {
    public int countHousePlacements(int n) {
        final int mod = (int) 1e9 + 7;
        int[] f = new int[n];
        int[] g = new int[n];
        f[0] = 1;
        g[0] = 1;
        for (int i = 1; i < n; ++i) {
            f[i] = g[i - 1];
            g[i] = (f[i - 1] + g[i - 1]) % mod;
        }
        long v = (f[n - 1] + g[n - 1]) % mod;
        return (int) (v * v % mod);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countHousePlacements(int n) {
        const int mod = 1e9 + 7;
        int f[n], g[n];
        f[0] = g[0] = 1;
        for (int i = 1; i < n; ++i) {
            f[i] = g[i - 1];
            g[i] = (f[i - 1] + g[i - 1]) % mod;
        }
        long v = f[n - 1] + g[n - 1];
        return v * v % mod;
    }
};
```

#### Go

```go
func countHousePlacements(n int) int {
	const mod = 1e9 + 7
	f := make([]int, n)
	g := make([]int, n)
	f[0], g[0] = 1, 1
	for i := 1; i < n; i++ {
		f[i] = g[i-1]
		g[i] = (f[i-1] + g[i-1]) % mod
	}
	v := f[n-1] + g[n-1]
	return v * v % mod
}
```

#### TypeScript

```ts
function countHousePlacements(n: number): number {
    const f = new Array(n);
    const g = new Array(n);
    f[0] = g[0] = 1n;
    const mod = BigInt(10 ** 9 + 7);
    for (let i = 1; i < n; ++i) {
        f[i] = g[i - 1];
        g[i] = (f[i - 1] + g[i - 1]) % mod;
    }
    const v = f[n - 1] + g[n - 1];
    return Number(v ** 2n % mod);
}
```

#### C#

```cs
public class Solution {
    public int CountHousePlacements(int n) {
        const int mod = (int) 1e9 + 7;
        int[] f = new int[n];
        int[] g = new int[n];
        f[0] = g[0] = 1;
        for (int i = 1; i < n; ++i) {
            f[i] = g[i - 1];
            g[i] = (f[i - 1] + g[i - 1]) % mod;
        }
        long v = (f[n - 1] + g[n - 1]) % mod;
        return (int) (v * v % mod);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
