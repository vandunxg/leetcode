---
comments: true
difficulty: Medium
rating: 1479
source: Weekly Contest 201 Q2
tags:
    - Recursion
    - String
    - Simulation
---

<!-- problem:start -->

# [1545. Find Kth Bit in Nth Binary String](https://leetcode.com/problems/find-kth-bit-in-nth-binary-string)

[中文文档](/solution/1500-1599/1545.Find%20Kth%20Bit%20in%20Nth%20Binary%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên dương <code>n</code> và <code>k</code>, chuỗi nhị phân <code>S<sub>n</sub></code> được tạo như sau:</p>

<ul>
	<li><code>S<sub>1</sub> = &quot;0&quot;</code></li>
	<li><code>S<sub>i</sub> = S<sub>i - 1</sub> + &quot;1&quot; + reverse(invert(S<sub>i - 1</sub>))</code> for <code>i &gt; 1</code></li>
</ul>

<p>Trong đó <code>+</code> là phép nối, <code>reverse(x)</code> trả về chuỗi <code>x</code> bị đảo ngược, còn <code>invert(x)</code> đảo tất cả bit trong <code>x</code> (<code>0</code> thành <code>1</code> và <code>1</code> thành <code>0</code>).</p>

<p>For example, the first four strings in the above sequence are:</p>

<ul>
	<li><code>S<sub>1 </sub>= &quot;0&quot;</code></li>
	<li><code>S<sub>2 </sub>= &quot;0<strong>1</strong>1&quot;</code></li>
	<li><code>S<sub>3 </sub>= &quot;011<strong>1</strong>001&quot;</code></li>
	<li><code>S<sub>4</sub> = &quot;0111001<strong>1</strong>0110001&quot;</code></li>
</ul>

<p><em>Trả về</em> <em>bit thứ</em> <code>k<sup>th</sup></code> <em>trong</em> <code>S<sub>n</sub></code>. Đảm bảo <code>k</code> hợp lệ với <code>n</code> đã cho.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 3, k = 1
<strong>Output:</strong> &quot;0&quot;
<strong>Explanation:</strong> S<sub>3</sub> is &quot;<strong><u>0</u></strong>111001&quot;.
Bit thứ 1<sup>st</sup> là &quot;0&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 4, k = 11
<strong>Output:</strong> &quot;1&quot;
<strong>Explanation:</strong> S<sub>4</sub> is &quot;0111001101<strong><u>1</u></strong>0001&quot;.
Bit thứ 11<sup>th</sup> là &quot;1&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 20</code></li>
	<li><code>1 &lt;= k &lt;= 2<sup>n</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp + Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> $S_n$ gồm $S_{n-1}$, rồi một $1$, rồi phần bù đảo ngược của $S_{n-1}$, có độ dài $2^n-1$. Với $n\le 20$, chuỗi đầy đủ có thể dài hàng triệu bit nên không nên tạo trực tiếp.
>
> Bit $k$ bằng $1$ nếu nằm ở giữa ($k$ là lũy thừa của hai); ở nửa trái, nó lấy từ $S_{n-1}[k]$; ở nửa phải, nó là phần bù của chỉ số đối xứng bên trái. Đệ quy theo ba trường hợp này có độ sâu $n$.

<!-- thinking:end -->

Ta nhận thấy với $S_n$, nửa đầu giống $S_{n-1}$, còn nửa sau là phần đảo ngược và phủ định của $S_{n-1}$. Vì vậy, có thể thiết kế hàm $dfs(n, k)$ biểu diễn ký tự thứ $k$ của chuỗi thứ $n$. Đáp án là $dfs(n, k)$.

Quá trình tính hàm $dfs(n, k)$ như sau:

- Nếu $k = 1$, đáp án là $0$;
- Nếu $k$ là lũy thừa của $2$, đáp án là $1$;
- Nếu $k \times 2 < 2^n - 1$, nghĩa là $k$ nằm ở nửa đầu và đáp án là $dfs(n - 1, k)$;
- Ngược lại, đáp án là $dfs(n - 1, 2^n - k) \oplus 1$, trong đó $\oplus$ là phép XOR.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là giá trị $n$ đã cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findKthBit(self, n: int, k: int) -> str:
        def dfs(n: int, k: int) -> int:
            if k == 1:
                return 0
            if (k & (k - 1)) == 0:
                return 1
            m = 1 << n
            if k * 2 < m - 1:
                return dfs(n - 1, k)
            return dfs(n - 1, m - k) ^ 1

        return str(dfs(n, k))
```

#### Java

```java
class Solution {
    public char findKthBit(int n, int k) {
        return (char) ('0' + dfs(n, k));
    }

    private int dfs(int n, int k) {
        if (k == 1) {
            return 0;
        }
        if ((k & (k - 1)) == 0) {
            return 1;
        }
        int m = 1 << n;
        if (k * 2 < m - 1) {
            return dfs(n - 1, k);
        }
        return dfs(n - 1, m - k) ^ 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    char findKthBit(int n, int k) {
        function<int(int, int)> dfs = [&](int n, int k) {
            if (k == 1) {
                return 0;
            }
            if ((k & (k - 1)) == 0) {
                return 1;
            }
            int m = 1 << n;
            if (k * 2 < m - 1) {
                return dfs(n - 1, k);
            }
            return dfs(n - 1, m - k) ^ 1;
        };
        return '0' + dfs(n, k);
    }
};
```

#### Go

```go
func findKthBit(n int, k int) byte {
	var dfs func(n, k int) int
	dfs = func(n, k int) int {
		if k == 1 {
			return 0
		}
		if k&(k-1) == 0 {
			return 1
		}
		m := 1 << n
		if k*2 < m-1 {
			return dfs(n-1, k)
		}
		return dfs(n-1, m-k) ^ 1
	}
	return byte('0' + dfs(n, k))
}
```

#### TypeScript

```ts
function findKthBit(n: number, k: number): string {
    const dfs = (n: number, k: number): number => {
        if (k === 1) {
            return 0;
        }
        if ((k & (k - 1)) === 0) {
            return 1;
        }
        const m = 1 << n;
        if (k * 2 < m - 1) {
            return dfs(n - 1, k);
        }
        return dfs(n - 1, m - k) ^ 1;
    };
    return dfs(n, k).toString();
}
```

#### Rust

```rust
impl Solution {
    pub fn find_kth_bit(n: i32, k: i32) -> char {
        fn dfs(n: i32, k: i32) -> i32 {
            if k == 1 {
                return 0;
            }
            if (k & (k - 1)) == 0 {
                return 1;
            }
            let m: i32 = 1 << n;
            if k * 2 < m - 1 {
                dfs(n - 1, k)
            } else {
                dfs(n - 1, m - k) ^ 1
            }
        }

        if dfs(n, k) == 0 { '0' } else { '1' }
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @param {number} k
 * @return {character}
 */
var findKthBit = function (n, k) {
    const dfs = function (n, k) {
        if (k === 1) {
            return 0;
        }
        if ((k & (k - 1)) === 0) {
            return 1;
        }
        const m = 1 << n;
        if (k * 2 < m - 1) {
            return dfs(n - 1, k);
        }
        return dfs(n - 1, m - k) ^ 1;
    };
    return dfs(n, k).toString();
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
