---
comments: true
difficulty: Medium
rating: 1848
source: Weekly Contest 476 Q3
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [3747. Count Distinct Integers After Removing Zeros](https://leetcode.com/problems/count-distinct-integers-after-removing-zeros)

[中文文档](/solution/3700-3799/3747.Count%20Distinct%20Integers%20After%20Removing%20Zeros/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <strong>dương</strong> <code>n</code>.</p>

<p>Với mỗi số nguyên <code>x</code> từ 1 đến <code>n</code>, ta viết số nguyên nhận được bằng cách xóa tất cả các chữ số 0 khỏi biểu diễn thập phân của <code>x</code>.</p>

<p>Trả về một số nguyên biểu thị số lượng số nguyên <strong>khác nhau</strong> đã viết.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số nguyên ta viết ra là 1, 2, 3, 4, 5, 6, 7, 8, 9, 1. Có 9 số nguyên khác nhau (1, 2, 3, 4, 5, 6, 7, 8, 9).</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số nguyên ta viết ra là 1, 2, 3. Có 3 số nguyên khác nhau (1, 2, 3).</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 10^{15}$ khiến việc xóa các chữ số 0 khỏi mọi $x$ là không thể. Sau khi xóa các chữ số 0, kết quả không chứa $0$ và không lớn hơn số ban đầu, nên các ảnh khác nhau trong $[1,n]$ chính xác là các số nguyên trong phạm vi đó không chứa chữ số 0, và ta có thể đếm chúng bằng digit DP.

<!-- thinking:end -->

Bài toán thực chất yêu cầu ta đếm số lượng số nguyên trong phạm vi $[1, n]$ không chứa chữ số 0. Ta có thể giải bài toán này bằng digit DP.

Ta thiết kế một hàm $\text{dfs}(i, \text{zero}, \text{lead}, \text{limit})$, biểu thị số lượng nghiệm hợp lệ khi đang xử lý chữ số thứ $i$ của số. Ta dùng $\text{zero}$ để cho biết chữ số khác 0 đã xuất hiện trong số hiện tại hay chưa, $\text{lead}$ để cho biết ta vẫn đang xử lý các số 0 ở đầu hay không, và $\text{limit}$ để cho biết số hiện tại có bị giới hạn bởi cận trên hay không. Đáp án là $\text{dfs}(0, 0, 1, 1)$.

Trong hàm $\text{dfs}(i, \text{zero}, \text{lead}, \text{limit})$, nếu $i$ lớn hơn hoặc bằng độ dài của số, ta có thể kiểm tra $\text{zero}$ và $\text{lead}$. Nếu $\text{zero}$ là false và $\text{lead}$ là false, điều đó có nghĩa là số hiện tại không chứa 0, nên ta trả về $1$; ngược lại, ta trả về $0$.

Với $\text{dfs}(i, \text{zero}, \text{lead}, \text{limit})$, ta có thể liệt kê giá trị của chữ số hiện tại $d$, sau đó đệ quy tính $\text{dfs}(i+1, \text{nxt\_zero}, \text{nxt\_lead}, \text{nxt\_limit})$, trong đó $\text{nxt\_zero}$ cho biết chữ số khác 0 đã xuất hiện trong số hiện tại hay chưa, $\text{nxt\_lead}$ cho biết ta vẫn đang xử lý các số 0 ở đầu hay không, và $\text{nxt\_limit}$ cho biết số hiện tại có bị giới hạn bởi cận trên hay không. Nếu $\text{limit}$ là true, thì $up$ là cận trên của chữ số hiện tại; nếu không, $up$ là $9$.

Độ phức tạp thời gian là $O(\log_{10} n \times D)$ và độ phức tạp không gian là $O(\log_{10} n)$, trong đó $D$ là số lượng chữ số từ 0 đến 9.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countDistinct(self, n: int) -> int:
        @cache
        def dfs(i: int, zero: bool, lead: bool, lim: bool) -> int:
            if i >= len(s):
                return 1 if (not zero and not lead) else 0
            up = int(s[i]) if lim else 9
            ans = 0
            for j in range(up + 1):
                nxt_zero = zero or (j == 0 and not lead)
                nxt_lead = lead and j == 0
                nxt_lim = lim and j == up
                ans += dfs(i + 1, nxt_zero, nxt_lead, nxt_lim)
            return ans

        s = str(n)
        return dfs(0, False, True, True)
```

#### Java

```java
class Solution {
    private char[] s;
    private Long[][][][] f;

    public long countDistinct(long n) {
        s = String.valueOf(n).toCharArray();
        f = new Long[s.length][2][2][2];
        return dfs(0, 0, 1, 1);
    }

    private long dfs(int i, int zero, int lead, int limit) {
        if (i == s.length) {
            return (zero == 0 && lead == 0) ? 1 : 0;
        }

        if (limit == 0 && f[i][zero][lead][limit] != null) {
            return f[i][zero][lead][limit];
        }

        int up = limit == 1 ? s[i] - '0' : 9;
        long ans = 0;
        for (int d = 0; d <= up; d++) {
            int nxtZero = zero == 1 || (d == 0 && lead == 0) ? 1 : 0;
            int nxtLead = (lead == 1 && d == 0) ? 1 : 0;
            int nxtLimit = (limit == 1 && d == up) ? 1 : 0;
            ans += dfs(i + 1, nxtZero, nxtLead, nxtLimit);
        }

        if (limit == 0) {
            f[i][zero][lead][limit] = ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countDistinct(long long n) {
        string s = to_string(n);
        int m = s.size();
        static long long f[20][2][2][2];
        memset(f, -1, sizeof(f));

        auto dfs = [&](this auto&& dfs, int i, int zero, int lead, int limit) -> long long {
            if (i == m) {
                return (zero == 0 && lead == 0) ? 1 : 0;
            }
            if (!limit && f[i][zero][lead][limit] != -1) {
                return f[i][zero][lead][limit];
            }

            int up = limit ? (s[i] - '0') : 9;
            long long ans = 0;
            for (int d = 0; d <= up; d++) {
                int nxtZero = zero || (d == 0 && !lead);
                int nxtLead = lead && d == 0;
                int nxtLimit = limit && d == up;
                ans += dfs(i + 1, nxtZero, nxtLead, nxtLimit);
            }

            if (!limit) f[i][zero][lead][limit] = ans;
            return ans;
        };

        return dfs(0, 0, 1, 1);
    }
};
```

#### Go

```go
func countDistinct(n int64) int64 {
	s := []byte(fmt.Sprint(n))
	m := len(s)
	var f [20][2][2][2]int64
	for i := range f {
		for j := range f[i] {
			for k := range f[i][j] {
				for t := range f[i][j][k] {
					f[i][j][k][t] = -1
				}
			}
		}
	}

	var dfs func(i, zero, lead, limit int) int64
	dfs = func(i, zero, lead, limit int) int64 {
		if i == m {
			if zero == 0 && lead == 0 {
				return 1
			}
			return 0
		}

		if limit == 0 && f[i][zero][lead][limit] != -1 {
			return f[i][zero][lead][limit]
		}

		up := 9
		if limit == 1 {
			up = int(s[i] - '0')
		}

		var ans int64 = 0
		for d := 0; d <= up; d++ {
			nxtZero := zero
			if d == 0 && lead == 0 {
				nxtZero = 1
			}
			nxtLead := 0
			if lead == 1 && d == 0 {
				nxtLead = 1
			}
			nxtLimit := 0
			if limit == 1 && d == up {
				nxtLimit = 1
			}
			ans += dfs(i+1, nxtZero, nxtLead, nxtLimit)
		}

		if limit == 0 {
			f[i][zero][lead][limit] = ans
		}
		return ans
	}

	return dfs(0, 0, 1, 1)
}
```

#### TypeScript

```ts
function countDistinct(n: number): number {
    const s = n.toString();
    const m = s.length;

    const f: number[][][][] = Array.from({ length: m }, () =>
        Array.from({ length: 2 }, () => Array.from({ length: 2 }, () => Array(2).fill(-1))),
    );

    const dfs = (i: number, zero: number, lead: number, limit: number): number => {
        if (i === m) {
            return zero === 0 && lead === 0 ? 1 : 0;
        }

        if (limit === 0 && f[i][zero][lead][limit] !== -1) {
            return f[i][zero][lead][limit];
        }

        const up = limit === 1 ? parseInt(s[i]) : 9;
        let ans = 0;
        for (let d = 0; d <= up; d++) {
            const nxtZero = zero === 1 || (d === 0 && lead === 0) ? 1 : 0;
            const nxtLead = lead === 1 && d === 0 ? 1 : 0;
            const nxtLimit = limit === 1 && d === up ? 1 : 0;
            ans += dfs(i + 1, nxtZero, nxtLead, nxtLimit);
        }

        if (limit === 0) {
            f[i][zero][lead][limit] = ans;
        }
        return ans;
    };

    return dfs(0, 0, 1, 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
