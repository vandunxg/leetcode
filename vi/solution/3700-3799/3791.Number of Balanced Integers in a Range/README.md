---
comments: true
difficulty: Hard
rating: 2132
source: Weekly Contest 482 Q4
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [3791. Number of Balanced Integers in a Range](https://leetcode.com/problems/number-of-balanced-integers-in-a-range)

[中文文档](/solution/3700-3799/3791.Number%20of%20Balanced%20Integers%20in%20a%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>low</code> và <code>high</code>.</p>

<p>Một số nguyên được gọi là <strong>cân bằng</strong> nếu thỏa mãn <strong>cả hai</strong> điều kiện sau:</p>

<ul>
	<li>Có <strong>ít nhất</strong> hai chữ số.</li>
	<li><strong>Tổng các chữ số ở vị trí chẵn</strong> bằng <strong>tổng các chữ số ở vị trí lẻ</strong> (chữ số ngoài cùng bên trái có vị trí 1).</li>
</ul>

<p>Trả về một số nguyên biểu diễn số lượng số nguyên cân bằng trong đoạn <code>[low, high]</code> (bao gồm cả hai đầu mút).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">low = 1, high = 100</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>9 số cân bằng trong đoạn từ 1 đến 100 là 11, 22, 33, 44, 55, 66, 77, 88 và 99.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">low = 120, high = 129</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có 121 là số cân bằng vì tổng các chữ số ở vị trí chẵn và lẻ đều bằng 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">low = 1234, high = 1234</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>1234 không cân bằng vì tổng các chữ số ở vị trí lẻ <code>(1 + 3 = 4)</code> không bằng tổng các chữ số ở vị trí chẵn <code>(2 + 4 = 6)</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= low &lt;= high &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DP chữ số

<!-- thinking:start -->

> **Tư duy**
>
> Một số cân bằng có ít nhất hai chữ số và tổng chữ số ở vị trí lẻ/chẵn bằng nhau. Để đếm trong một đoạn, ta dùng digit DP $calc(high)-calc(low-1)$ với trạng thái gồm vị trí, hiệu hiện tại và giới hạn; các số có ít hơn hai chữ số được loại bỏ bằng cách nâng cận dưới lên $11$.

<!-- thinking:end -->

Trước hết, nếu $\textit{high} < 11$, trong đoạn không có số nguyên cân bằng nào, nên ta trả về trực tiếp $0$. Nếu không, ta cập nhật $\textit{low}$ thành $\max(\textit{low}, 11)$.

Sau đó, ta xây dựng hàm $\textit{dfs}(\textit{pos}, \textit{diff}, \textit{lim})$, biểu diễn việc xử lý chữ số thứ $\textit{pos}$ của số, trong đó $\textit{diff}$ là hiệu giữa tổng các chữ số ở vị trí lẻ và tổng các chữ số ở vị trí chẵn, còn $\textit{lim}$ cho biết chữ số hiện tại có bị giới hạn bởi cận trên hay không. Hàm trả về số lượng số nguyên cân bằng có thể tạo thành từ trạng thái hiện tại.

Logic thực thi của hàm như sau:

- Nếu $\textit{pos}$ vượt quá độ dài của số, nghĩa là mọi chữ số đã được xử lý. Nếu $\textit{diff} = 0$, số hiện tại là một số nguyên cân bằng, trả về $1$; ngược lại, trả về $0$.
- Tính cận trên $\textit{up}$ cho chữ số hiện tại. Nếu đang bị giới hạn, nó bằng chữ số hiện tại của số; nếu không, nó bằng $9$.
- Duyệt qua mọi chữ số khả dĩ $i$ tại vị trí hiện tại. Với mỗi chữ số $i$, gọi đệ quy $\textit{dfs}(\textit{pos} + 1, \textit{diff} + i \times (\text{1 if pos \% 2 == 0 else -1}), \textit{lim} \&\& i == \textit{up})$, rồi cộng dồn kết quả.
- Trả về kết quả đã cộng dồn.

Đầu tiên, ta tính số lượng số nguyên cân bằng $\textit{a}$ trong đoạn $[1, \textit{low} - 1]$, sau đó tính số lượng số nguyên cân bằng $\textit{b}$ trong đoạn $[1, \textit{high}]$, cuối cùng trả về $\textit{b} - \textit{a}$.

Để tránh tính toán dư thừa, ta dùng memoization để lưu các trạng thái đã tính.

Độ phức tạp thời gian là $O(\log^2 M \times D^2)$, và độ phức tạp không gian là $O(\log^2 M \times D)$. Trong đó, $M$ là giá trị của $\textit{high}$, và $D = 10$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countBalanced(self, low: int, high: int) -> int:
        @cache
        def dfs(pos: int, diff: int, lim: int) -> int:
            if pos >= len(num):
                return 1 if diff == 0 else 0
            res = 0
            up = int(num[pos]) if lim else 9
            for i in range(up + 1):
                res += dfs(
                    pos + 1, diff + i * (1 if pos % 2 == 0 else -1), lim and i == up
                )
            return res

        if high < 11:
            return 0
        low = max(low, 11)
        num = str(low - 1)
        a = dfs(0, 0, True)
        dfs.cache_clear()
        num = str(high)
        b = dfs(0, 0, True)
        return b - a
```

#### Java

```java
class Solution {
    private char[] num;
    private Long[][] f;
    private final int base = 90;

    public long countBalanced(long low, long high) {
        if (high < 11) {
            return 0;
        }
        low = Math.max(low, 11);
        num = String.valueOf(low - 1).toCharArray();
        f = new Long[num.length][base << 1 | 1];
        long a = dfs(0, 0, true);
        num = String.valueOf(high).toCharArray();
        f = new Long[num.length][base << 1 | 1];
        long b = dfs(0, 0, true);
        return b - a;
    }

    private long dfs(int pos, int diff, boolean lim) {
        if (pos >= num.length) {
            return diff == 0 ? 1 : 0;
        }
        if (!lim && f[pos][diff + base] != null) {
            return f[pos][diff + base];
        }
        int up = lim ? num[pos] - '0' : 9;
        long res = 0;
        for (int i = 0; i <= up; ++i) {
            res += dfs(pos + 1, diff + i * (pos % 2 == 0 ? 1 : -1), lim && i == up);
        }
        if (!lim) {
            f[pos][diff + base] = res;
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string num;
    long long f[20][181];
    static constexpr int base = 90;

    long long dfs(int pos, int diff, bool lim) {
        if (pos >= (int) num.size()) {
            return diff == 0 ? 1LL : 0LL;
        }
        if (!lim && f[pos][diff + base] != -1) {
            return f[pos][diff + base];
        }
        int up = lim ? num[pos] - '0' : 9;
        long long res = 0;
        for (int i = 0; i <= up; ++i) {
            res += dfs(pos + 1, diff + i * (pos % 2 == 0 ? 1 : -1), lim && i == up);
        }
        if (!lim) {
            f[pos][diff + base] = res;
        }
        return res;
    }

    long long countBalanced(long long low, long long high) {
        if (high < 11) {
            return 0;
        }
        low = max(low, 11LL);

        num = to_string(low - 1);
        memset(f, -1, sizeof(f));
        long long a = dfs(0, 0, true);

        num = to_string(high);
        memset(f, -1, sizeof(f));
        long long b = dfs(0, 0, true);

        return b - a;
    }
};
```

#### Go

```go
func countBalanced(low int64, high int64) int64 {
	if high < 11 {
		return 0
	}
	if low < 11 {
		low = 11
	}
	const base = 90

	var num []byte
	var f [20][181]int64

	var dfs func(pos int, diff int, lim bool) int64
	dfs = func(pos int, diff int, lim bool) int64 {
		if pos >= len(num) {
			if diff == 0 {
				return 1
			}
			return 0
		}
		if !lim && f[pos][diff+base] != -1 {
			return f[pos][diff+base]
		}
		up := 9
		if lim {
			up = int(num[pos] - '0')
		}
		var res int64 = 0
		for i := 0; i <= up; i++ {
			if pos%2 == 0 {
				res += dfs(pos+1, diff+i, lim && i == up)
			} else {
				res += dfs(pos+1, diff-i, lim && i == up)
			}
		}
		if !lim {
			f[pos][diff+base] = res
		}
		return res
	}

	num = []byte(fmt.Sprint(low - 1))
	for i := range f {
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	a := dfs(0, 0, true)

	num = []byte(fmt.Sprint(high))
	for i := range f {
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	b := dfs(0, 0, true)

	return b - a
}
```

#### TypeScript

```ts
function countBalanced(low: number, high: number): number {
    if (high < 11) {
        return 0;
    }
    if (low < 11) {
        low = 11;
    }
    const base = 90;

    let num: string;
    let f: number[][];

    function dfs(pos: number, diff: number, lim: boolean): number {
        if (pos >= num.length) {
            return diff === 0 ? 1 : 0;
        }
        if (!lim && f[pos][diff + base] !== -1) {
            return f[pos][diff + base];
        }
        const up = lim ? num.charCodeAt(pos) - 48 : 9;
        let res = 0;
        for (let i = 0; i <= up; ++i) {
            res += dfs(pos + 1, diff + i * (pos % 2 === 0 ? 1 : -1), lim && i === up);
        }
        if (!lim) {
            f[pos][diff + base] = res;
        }
        return res;
    }

    num = String(low - 1);
    f = Array.from({ length: num.length }, () => Array((base << 1) | 1).fill(-1));
    const a = dfs(0, 0, true);

    num = String(high);
    f = Array.from({ length: num.length }, () => Array((base << 1) | 1).fill(-1));
    const b = dfs(0, 0, true);

    return b - a;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
