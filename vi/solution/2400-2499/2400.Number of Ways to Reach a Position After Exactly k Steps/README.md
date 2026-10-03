---
comments: true
difficulty: Medium
rating: 1751
source: Weekly Contest 309 Q2
tags:
    - Math
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [2400. Number of Ways to Reach a Position After Exactly k Steps](https://leetcode.com/problems/number-of-ways-to-reach-a-position-after-exactly-k-steps)

[中文文档](/solution/2400-2499/2400.Number%20of%20Ways%20to%20Reach%20a%20Position%20After%20Exactly%20k%20Steps/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <strong>dương</strong> <code>startPos</code> và <code>endPos</code>. Ban đầu, bạn đứng tại vị trí <code>startPos</code> trên một trục số <strong>vô hạn</strong>. Với mỗi bước, bạn có thể di chuyển sang trái một vị trí hoặc sang phải một vị trí.</p>

<p>Cho số nguyên dương <code>k</code>, hãy trả về <em>số <strong>cách khác nhau</strong> để đến vị trí </em><code>endPos</code><em> bắt đầu từ </em><code>startPos</code><em>, sao cho bạn thực hiện <strong>chính xác</strong> </em><code>k</code><em> bước</em>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Hai cách được xem là khác nhau nếu thứ tự thực hiện các bước không hoàn toàn giống nhau.</p>

<p><strong>Lưu ý</strong> rằng trục số bao gồm cả các số nguyên âm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> startPos = 1, endPos = 2, k = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể đi từ vị trí 1 đến vị trí 2 trong đúng 3 bước theo ba cách:
- 1 -&gt; 2 -&gt; 3 -&gt; 2.
- 1 -&gt; 2 -&gt; 1 -&gt; 2.
- 1 -&gt; 0 -&gt; 1 -&gt; 2.
Có thể chứng minh rằng không còn cách nào khác, nên ta trả về 3.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> startPos = 2, endPos = 5, k = 10
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không thể đi từ vị trí 2 đến vị trí 5 trong đúng 10 bước.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= startPos, endPos, k &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước di chuyển sang trái hoặc phải một đơn vị. Nếu liệt kê tất cả các đường đi có độ dài $k$ thì chi phí là $2^k$; với $k\le 1000$, cách này là bất khả thi. Dùng memoization với vị trí tuyệt đối và số bước còn lại sẽ tạo ra $O(k^2)$ trạng thái, nhưng có thể tịnh tiến gốc tọa độ nên không cần giữ tọa độ thực.
>
> Số cách chỉ phụ thuộc vào khoảng cách $i$ đến đích và số bước còn lại $j$: đi một bước về phía đích cho khoảng cách $|i-1|$, còn đi xa đích cho khoảng cách $i+1$. Vì vậy, ta dùng $dfs(i,j)$ cùng với memoization. Nếu $i>j$, số bước còn lại không đủ để đi hết khoảng cách, nên trạng thái này có giá trị bằng không.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i, j)$, biểu diễn số cách đến vị trí đích khi vị trí hiện tại cách đích $i$ đơn vị và còn $j$ bước. Đáp án là $dfs(abs(startPos - endPos), k)$.

Cách tính hàm $dfs(i, j)$ như sau:

- Nếu $i \gt j$ hoặc $j \lt 0$, điều đó có nghĩa là khoảng cách hiện tại đến đích lớn hơn số bước còn lại, hoặc số bước còn lại là số âm. Khi đó không thể đến đích, nên trả về $0$;
- Nếu $j = 0$, nghĩa là không còn bước nào. Khi đó chỉ có thể đến đích nếu khoảng cách hiện tại đến đích bằng $0$; nếu không thì không thể đến đích. Trả về $1$ hoặc $0$;
- Nếu không, khoảng cách hiện tại đến đích là $i$ và còn $j$ bước. Có hai cách để đến đích:
    - Di chuyển sang trái một bước, khoảng cách hiện tại đến đích là $i + 1$ và còn $j - 1$ bước. Số cách là $dfs(i + 1, j - 1)$;
    - Di chuyển sang phải một bước, khoảng cách hiện tại đến đích là $abs(i - 1)$ và còn $j - 1$ bước. Số cách là $dfs(abs(i - 1), j - 1)$;
- Cuối cùng, trả về tổng của hai số cách trên modulo $10^9 + 7$.

Để tránh tính toán lặp lại, ta dùng tìm kiếm có nhớ, tức là dùng mảng hai chiều $f$ để lưu kết quả của hàm $dfs(i, j)$. Khi gọi hàm $dfs(i, j)$, nếu $f[i][j]$ khác $-1$ thì trả về trực tiếp $f[i][j]$; nếu không, ta tính giá trị của $f[i][j]$ rồi trả về $f[i][j]$.

Độ phức tạp thời gian là $O(k^2)$ và độ phức tạp không gian là $O(k^2)$. Trong đó, $k$ là số bước được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfWays(self, startPos: int, endPos: int, k: int) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if i > j or j < 0:
                return 0
            if j == 0:
                return 1 if i == 0 else 0
            return (dfs(i + 1, j - 1) + dfs(abs(i - 1), j - 1)) % mod

        mod = 10**9 + 7
        return dfs(abs(startPos - endPos), k)
```

#### Java

```java
class Solution {
    private Integer[][] f;
    private final int mod = (int) 1e9 + 7;

    public int numberOfWays(int startPos, int endPos, int k) {
        f = new Integer[k + 1][k + 1];
        return dfs(Math.abs(startPos - endPos), k);
    }

    private int dfs(int i, int j) {
        if (i > j || j < 0) {
            return 0;
        }
        if (j == 0) {
            return i == 0 ? 1 : 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        int ans = dfs(i + 1, j - 1) + dfs(Math.abs(i - 1), j - 1);
        ans %= mod;
        return f[i][j] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfWays(int startPos, int endPos, int k) {
        const int mod = 1e9 + 7;
        int f[k + 1][k + 1];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i, int j) -> int {
            if (i > j || j < 0) {
                return 0;
            }
            if (j == 0) {
                return i == 0 ? 1 : 0;
            }
            if (f[i][j] != -1) {
                return f[i][j];
            }
            f[i][j] = (dfs(i + 1, j - 1) + dfs(abs(i - 1), j - 1)) % mod;
            return f[i][j];
        };
        return dfs(abs(startPos - endPos), k);
    }
};
```

#### Go

```go
func numberOfWays(startPos int, endPos int, k int) int {
	const mod = 1e9 + 7
	f := make([][]int, k+1)
	for i := range f {
		f[i] = make([]int, k+1)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i > j || j < 0 {
			return 0
		}
		if j == 0 {
			if i == 0 {
				return 1
			}
			return 0
		}
		if f[i][j] != -1 {
			return f[i][j]
		}
		f[i][j] = (dfs(i+1, j-1) + dfs(abs(i-1), j-1)) % mod
		return f[i][j]
	}
	return dfs(abs(startPos-endPos), k)
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function numberOfWays(startPos: number, endPos: number, k: number): number {
    const mod = 10 ** 9 + 7;
    const f = new Array(k + 1).fill(0).map(() => new Array(k + 1).fill(-1));
    const dfs = (i: number, j: number): number => {
        if (i > j || j < 0) {
            return 0;
        }
        if (j === 0) {
            return i === 0 ? 1 : 0;
        }
        if (f[i][j] !== -1) {
            return f[i][j];
        }
        f[i][j] = dfs(i + 1, j - 1) + dfs(Math.abs(i - 1), j - 1);
        f[i][j] %= mod;
        return f[i][j];
    };
    return dfs(Math.abs(startPos - endPos), k);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
