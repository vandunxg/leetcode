---
comments: true
difficulty: Medium
rating: 1694
source: Biweekly Contest 91 Q2
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [2466. Count Ways To Build Good Strings](https://leetcode.com/problems/count-ways-to-build-good-strings)

[中文文档](/solution/2400-2499/2466.Count%20Ways%20To%20Build%20Good%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Với các số nguyên <code>zero</code>, <code>one</code>, <code>low</code> và <code>high</code>, ta có thể tạo một chuỗi bằng cách bắt đầu với chuỗi rỗng, sau đó ở mỗi bước thực hiện một trong các thao tác sau:</p>

<ul>
	<li>Nối ký tự <code>&#39;0&#39;</code> <code>zero</code> lần.</li>
	<li>Nối ký tự <code>&#39;1&#39;</code> <code>one</code> lần.</li>
</ul>

<p>Ta có thể thực hiện các thao tác này với số lần tùy ý.</p>

<p>Một chuỗi <strong>hợp lệ</strong> là chuỗi được tạo bởi quy trình trên và có <strong>độ dài</strong> nằm trong khoảng từ <code>low</code> đến <code>high</code> (<strong>bao gồm cả hai đầu mút</strong>).</p>

<p>Hãy trả về <em>số <strong>chuỗi hợp lệ khác nhau</strong> có thể tạo ra và thỏa mãn các điều kiện trên.</em> Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> low = 3, high = 3, zero = 1, one = 1
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong>
Một chuỗi hợp lệ có thể là &quot;011&quot;.
Chuỗi này được tạo như sau: &quot;&quot; -&gt; &quot;0&quot; -&gt; &quot;01&quot; -&gt; &quot;011&quot;.
Trong ví dụ này, tất cả các chuỗi nhị phân từ &quot;000&quot; đến &quot;111&quot; đều là chuỗi hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> low = 2, high = 3, zero = 1, one = 2
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Các chuỗi hợp lệ là &quot;00&quot;, &quot;11&quot;, &quot;000&quot;, &quot;110&quot; và &quot;011&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= low&nbsp;&lt;= high&nbsp;&lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= zero, one &lt;= low</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước nối thêm $zero$ ký tự 0 hoặc $one$ ký tự 1; một chuỗi là hợp lệ nếu độ dài của nó nằm trong $[low,high]$. Với $high\le 10^5$, $dfs(i)$ là số cách sau khi đạt độ dài $i$: tính cả $1$ nếu $i$ đã nằm trong khoảng, sau đó cộng $dfs(i+zero)$ và $dfs(i+one)$.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i)$ biểu diễn số chuỗi hợp lệ được tạo khi bắt đầu từ vị trí $i$. Đáp án là $dfs(0)$.

Cách tính hàm $dfs(i)$ như sau:

- Nếu $i > high$, trả về $0$;
- Nếu $low \leq i \leq high$, tăng đáp án lên $1$. Sau đó, từ vị trí $i$, ta có thể nối thêm `zero` ký tự $0$ hoặc `one` ký tự $1$. Do đó, đáp án được tăng thêm $dfs(i + zero) + dfs(i + one)$.

Trong quá trình tính, ta cần lấy modulo cho đáp án và có thể dùng tìm kiếm có nhớ để giảm các phép tính trùng lặp.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n = high$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countGoodStrings(self, low: int, high: int, zero: int, one: int) -> int:
        @cache
        def dfs(i):
            if i > high:
                return 0
            ans = 0
            if low <= i <= high:
                ans += 1
            ans += dfs(i + zero) + dfs(i + one)
            return ans % mod

        mod = 10**9 + 7
        return dfs(0)
```

#### Java

```java
class Solution {
    private static final int MOD = (int) 1e9 + 7;
    private int[] f;
    private int lo;
    private int hi;
    private int zero;
    private int one;

    public int countGoodStrings(int low, int high, int zero, int one) {
        f = new int[high + 1];
        Arrays.fill(f, -1);
        lo = low;
        hi = high;
        this.zero = zero;
        this.one = one;
        return dfs(0);
    }

    private int dfs(int i) {
        if (i > hi) {
            return 0;
        }
        if (f[i] != -1) {
            return f[i];
        }
        long ans = 0;
        if (i >= lo && i <= hi) {
            ++ans;
        }
        ans += dfs(i + zero) + dfs(i + one);
        ans %= MOD;
        f[i] = (int) ans;
        return f[i];
    }
}
```

#### C++

```cpp
class Solution {
public:
    const int mod = 1e9 + 7;

    int countGoodStrings(int low, int high, int zero, int one) {
        vector<int> f(high + 1, -1);
        function<int(int)> dfs = [&](int i) -> int {
            if (i > high) return 0;
            if (f[i] != -1) return f[i];
            long ans = i >= low && i <= high;
            ans += dfs(i + zero) + dfs(i + one);
            ans %= mod;
            f[i] = ans;
            return ans;
        };
        return dfs(0);
    }
};
```

#### Go

```go
func countGoodStrings(low int, high int, zero int, one int) int {
	f := make([]int, high+1)
	for i := range f {
		f[i] = -1
	}
	const mod int = 1e9 + 7
	var dfs func(i int) int
	dfs = func(i int) int {
		if i > high {
			return 0
		}
		if f[i] != -1 {
			return f[i]
		}
		ans := 0
		if i >= low && i <= high {
			ans++
		}
		ans += dfs(i+zero) + dfs(i+one)
		ans %= mod
		f[i] = ans
		return ans
	}
	return dfs(0)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 đệ quy theo độ dài hiện tại. Gọi $f[i]$ là số cách đạt độ dài $i$, với $f[0]=1$; ta tính từ $f[i-zero]$ và $f[i-one]$, sau đó cộng các giá trị $f$ trong $[low,high]$. Cách này không sử dụng stack đệ quy.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function countGoodStrings(low: number, high: number, zero: number, one: number): number {
    const mod = 10 ** 9 + 7;
    const f: number[] = new Array(high + 1).fill(0);
    f[0] = 1;

    for (let i = 1; i <= high; i++) {
        if (i >= zero) f[i] += f[i - zero];
        if (i >= one) f[i] += f[i - one];
        f[i] %= mod;
    }

    const ans = f.slice(low, high + 1).reduce((acc, cur) => acc + cur, 0);

    return ans % mod;
}
```

#### JavaScript

```js
/**
 * @param {number} low
 * @param {number} high
 * @param {number} zero
 * @param {number} one
 * @return {number}
 */
function countGoodStrings(low, high, zero, one) {
    const mod = 10 ** 9 + 7;
    const f = Array(high + 1).fill(0);
    f[0] = 1;

    for (let i = 1; i <= high; i++) {
        if (i >= zero) f[i] += f[i - zero];
        if (i >= one) f[i] += f[i - one];
        f[i] %= mod;
    }

    const ans = f.slice(low, high + 1).reduce((acc, cur) => acc + cur, 0);

    return ans % mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
