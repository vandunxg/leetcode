---
comments: true
difficulty: Hard
---

<!-- problem:start -->

# [4060. Count Evenly Good Integers 🔒](https://leetcode.com/problems/count-evenly-good-integers)

[中文文档](/solution/4000-4099/4060.Count%20Evenly%20Good%20Integers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>l</code> và <code>r</code>.</p>

<p>Một số nguyên được gọi là <strong>đều tốt</strong> nếu nó chứa một số lượng chữ số chẵn là số chẵn.</p>

<p>Hãy trả về số lượng số nguyên đều tốt trong đoạn bao gồm cả hai đầu mút <code>[l, r]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 18, r = 22</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số nguyên đều tốt trong đoạn <code>[18, 22]</code> là:</p>

<ul>
	<li>19, vì nó chứa 0 chữ số chẵn.</li>
	<li>20, vì nó chứa 2 chữ số chẵn.</li>
	<li>22, vì nó chứa 2 chữ số chẵn.</li>
</ul>

<p>Do đó, đáp án là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 98, r = 101</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số nguyên đều tốt trong đoạn <code>[98, 101]</code> là:</p>

<ul>
	<li>99, vì nó chứa 0 chữ số chẵn.</li>
	<li>100, vì nó chứa 2 chữ số chẵn.</li>
</ul>

<p>Do đó, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 1, r = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số nguyên đều tốt trong đoạn <code>[1, 10]</code> là 1, 3, 5, 7 và 9, vì mỗi số đều chứa 0 chữ số chẵn. Do đó, đáp án là 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= l &lt;= r &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Vì $r$ có thể lớn tới $10^{15}$, việc kiểm tra từng số nguyên trong đoạn $[l, r]$ sẽ không thể hoàn thành trong thời gian cho phép. Số lượng trong đoạn bằng $F(r)-F(l-1)$, nên chỉ cần đếm số nguyên không vượt quá một cận trên.
>
> Một số nguyên đều tốt khi và chỉ khi số chữ số chẵn của nó là số chẵn, vì vậy ta có thể điền các chữ số từ cao xuống thấp.
>
> Khi $x$ có $d$ chữ số, mọi số nguyên ngắn hơn đều xuất hiện với các số 0 ở đầu, và các số 0 đó là chữ số chẵn. Trên $[0, 10^{d-1}-1]$, số lượng tính cả các số 0 ở đầu bằng số lượng trong biểu diễn thập phân thông thường, còn một số nguyên có đủ $d$ chữ số không có số 0 ở đầu, nên hai cách đếm trùng khớp trên $[0, x]$.
>
> Trạng thái tìm kiếm gồm vị trí, số lượng chữ số chẵn modulo $2$ và việc tiền tố hiện tại có đang bám sát hay không. Mỗi chữ số được chọn từ $0$ đến cận hiện tại, chữ số chẵn sẽ đảo parity, và một số hoàn chỉnh được tính khi parity là chẵn.

<!-- thinking:end -->

Gọi $F(x)$ là số lượng số nguyên đều tốt trong $[0, x]$. Đáp án là $F(r)-F(l-1)$. Biểu diễn thập phân của $0$ là chữ số đơn $0$, chứa một số lượng chữ số chẵn là số lẻ, nên $F(0)=0$.

Viết $x$ dưới dạng chuỗi thập phân $s$ và tìm kiếm từ chữ số cao xuống chữ số thấp với memoization. $dfs(pos, st, lim)$ là số cách điền vị trí $pos$ khi số chữ số chẵn đã đặt có $st$ modulo $2$, còn $lim$ cho biết tiền tố hiện tại có đang bám sát $x$ hay không.

Khi đi hết chuỗi, trả về $1$ nếu $st=0$ và trả về $0$ trong trường hợp ngược lại. Chữ số trên $up$ bằng $s[pos]$ khi tiền tố đang bám sát, và bằng $9$ nếu không. Với chữ số $i$ từ $0$ đến $up$, parity tiếp theo là $(st+1)\bmod 2$ khi $i$ chẵn và vẫn là $st$ khi $i$ lẻ. Vị trí tiếp theo vẫn bám sát chỉ khi $lim$ là true và $i=up$.

Trong các chữ số từ $0$ đến $9$, có năm chữ số chẵn và năm chữ số lẻ, nên một dãy chữ số tự do được chia đều giữa hai parity. Khi $x$ có $d\ge 2$ chữ số, mọi số nguyên có ít chữ số hơn đều xuất hiện trong quá trình tìm kiếm với các số 0 ở đầu, và đó chính là các số trong $[0, 10^{d-1}-1]$. Sau khi thêm các số 0 ở đầu để có $d$ chữ số, chữ số đầu là chữ số chẵn $0$ và các chữ số còn lại là tự do, tạo ra $10^{d-1}/2$ chuỗi có số lượng chữ số chẵn là số chẵn. Trong biểu diễn thập phân thông thường, có $5$ số nguyên đều tốt có một chữ số, và mỗi độ dài $k\ge 2$ đóng góp chính xác một nửa số nguyên của độ dài đó. Cộng các độ dài từ $1$ đến $d-1$ ta được

$$
5+\sum_{k=2}^{d-1}\frac{9}{2}\times 10^{k-1}=\frac{10^{d-1}}{2}.
$$

Một số nguyên vốn đã có $d$ chữ số và không vượt quá $x$ không có số 0 ở đầu, nên hai cách viết trùng khớp. Vì vậy, quá trình tìm kiếm trả về $F(x)$.

Độ phức tạp thời gian là $O(\log r)$ và độ phức tạp không gian là $O(\log r)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countEvenlyGoodIntegers(self, l: int, r: int) -> int:
        @cache
        def dfs(pos: int, st: int, lim: bool) -> int:
            if pos >= len(s):
                return st ^ 1
            up = int(s[pos]) if lim else 9
            return sum(
                dfs(pos + 1, (st + (i & 1 ^ 1)) % 2, lim and i == up)
                for i in range(up + 1)
            )

        s = str(l - 1)
        a = dfs(0, 0, True)
        dfs.cache_clear()
        s = str(r)
        b = dfs(0, 0, True)
        return b - a
```

#### Java

```java
class Solution {
    String s;
    long[][][] f;

    public long countEvenlyGoodIntegers(long l, long r) {
        return calc(r) - calc(l - 1);
    }

    private long calc(long x) {
        s = String.valueOf(x);
        f = new long[s.length()][2][2];
        for (long[][] a : f) {
            for (long[] b : a) {
                Arrays.fill(b, -1);
            }
        }
        return dfs(0, 0, true);
    }

    private long dfs(int pos, int st, boolean lim) {
        if (pos >= s.length()) {
            return st ^ 1;
        }
        int k = lim ? 1 : 0;
        if (f[pos][st][k] != -1) {
            return f[pos][st][k];
        }
        int up = lim ? s.charAt(pos) - '0' : 9;
        long res = 0;
        for (int i = 0; i <= up; ++i) {
            res += dfs(pos + 1, (st + (i & 1 ^ 1)) % 2, lim && i == up);
        }
        return f[pos][st][k] = res;
    }
}
```

#### C++

```cpp
class Solution {
    string s;
    long long f[20][2][2];

    long long dfs(int pos, int st, bool lim) {
        if (pos >= s.size()) {
            return st ^ 1;
        }
        if (f[pos][st][lim] != -1) {
            return f[pos][st][lim];
        }
        int up = lim ? s[pos] - '0' : 9;
        long long res = 0;
        for (int i = 0; i <= up; ++i) {
            res += dfs(pos + 1, (st + (i & 1 ^ 1)) % 2, lim && i == up);
        }
        return f[pos][st][lim] = res;
    }

    long long calc(long long x) {
        s = to_string(x);
        memset(f, -1, sizeof(f));
        return dfs(0, 0, true);
    }

public:
    long long countEvenlyGoodIntegers(long long l, long long r) {
        return calc(r) - calc(l - 1);
    }
};
```

#### Go

```go
func countEvenlyGoodIntegers(l int64, r int64) int64 {
	var s string
	var f [20][2][2]int64

	calc := func(x int64) int64 {
		s = strconv.FormatInt(x, 10)
		for i := range f {
			for j := range f[i] {
				for k := range f[i][j] {
					f[i][j][k] = -1
				}
			}
		}

		var dfs func(pos, st int, lim bool) int64
		dfs = func(pos, st int, lim bool) int64 {
			if pos >= len(s) {
				return int64(st ^ 1)
			}
			k := 0
			if lim {
				k = 1
			}
			if f[pos][st][k] != -1 {
				return f[pos][st][k]
			}
			up := 9
			if lim {
				up = int(s[pos] - '0')
			}
			var res int64
			for i := 0; i <= up; i++ {
				res += dfs(pos+1, (st+(i&1^1))%2, lim && i == up)
			}
			f[pos][st][k] = res
			return res
		}

		return dfs(0, 0, true)
	}

	return calc(r) - calc(l-1)
}
```

#### TypeScript

```ts
function countEvenlyGoodIntegers(l: number, r: number): number {
    let s: string;
    let f: number[][][];

    const calc = (x: number): number => {
        s = String(x);
        f = Array.from({ length: s.length }, () =>
            Array.from({ length: 2 }, () => Array(2).fill(-1)),
        );

        const dfs = (pos: number, st: number, lim: boolean): number => {
            if (pos >= s.length) {
                return st ^ 1;
            }
            const k = lim ? 1 : 0;
            if (f[pos][st][k] !== -1) {
                return f[pos][st][k];
            }
            const up = lim ? Number(s[pos]) : 9;
            let res = 0;
            for (let i = 0; i <= up; ++i) {
                res += dfs(pos + 1, (st + ((i & 1) ^ 1)) % 2, lim && i === up);
            }
            return (f[pos][st][k] = res);
        };

        return dfs(0, 0, true);
    };

    return calc(r) - calc(l - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
