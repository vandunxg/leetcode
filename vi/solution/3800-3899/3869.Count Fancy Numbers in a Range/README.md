---
comments: true
difficulty: Hard
rating: 2254
source: Biweekly Contest 178 Q4
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [3869. Count Fancy Numbers in a Range](https://leetcode.com/problems/count-fancy-numbers-in-a-range)

[中文文档](/solution/3800-3899/3869.Count%20Fancy%20Numbers%20in%20a%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>l</code> và <code>r</code>.</p>

<p>Một số nguyên được gọi là <strong>tốt</strong> nếu các chữ số của nó tạo thành một dãy <strong>đơn điệu nghiêm ngặt</strong>, nghĩa là các chữ số <strong>tăng nghiêm ngặt</strong> hoặc <strong>giảm nghiêm ngặt</strong>. Mọi số nguyên có một chữ số đều được xem là tốt.</p>

<p>Một số nguyên được gọi là <strong>fancy</strong> nếu nó là số tốt hoặc <strong>tổng các chữ số</strong> của nó là một số tốt.</p>

<p>Hãy trả về một số nguyên biểu thị số lượng số nguyên fancy trong đoạn <code>[l, r]</code> (bao gồm cả hai đầu mút).</p>

<p>Một dãy được gọi là <strong>tăng nghiêm ngặt</strong> nếu mỗi phần tử <strong>lớn hơn</strong> phần tử trước đó (nếu có).</p>

<p>Một dãy được gọi là <strong>giảm nghiêm ngặt</strong> nếu mỗi phần tử <strong>nhỏ hơn</strong> phần tử trước đó (nếu có).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 8, r = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>8 và 9 là các số nguyên có một chữ số, nên chúng là số tốt và do đó là số fancy.</li>
	<li>10 có các chữ số <code>[1, 0]</code>, tạo thành một dãy giảm nghiêm ngặt, nên 10 là số tốt và do đó là số fancy.</li>
</ul>

<p>Vì vậy, đáp án là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 12340, r = 12341</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>12340
	<ul>
		<li>12340 không phải là số tốt vì <code>[1, 2, 3, 4, 0]</code> không tạo thành dãy đơn điệu nghiêm ngặt.</li>
		<li>Tổng các chữ số là <code>1 + 2 + 3 + 4 + 0 = 10</code>.</li>
		<li>10 là số tốt vì có các chữ số <code>[1, 0]</code>, tạo thành một dãy giảm nghiêm ngặt. Do đó, 12340 là số fancy.</li>
	</ul>
	</li>
	<li>12341
	<ul>
		<li>12341 không phải là số tốt vì <code>[1, 2, 3, 4, 1]</code> không tạo thành dãy đơn điệu nghiêm ngặt.</li>
		<li>Tổng các chữ số là <code>1 + 2 + 3 + 4 + 1 = 11</code>.</li>
		<li>11 không phải là số tốt vì có các chữ số <code>[1, 1]</code>, không tạo thành dãy đơn điệu nghiêm ngặt. Do đó, 12341 không phải là số fancy.</li>
	</ul>
	</li>
</ul>

<p>Vì vậy, đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 123456788, r = 123456788</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>123456788 không phải là số tốt vì các chữ số của nó không tạo thành dãy đơn điệu nghiêm ngặt.</li>
	<li>Tổng các chữ số là <code>1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 8 = 44</code>.</li>
	<li>44 không phải là số tốt vì có các chữ số <code>[4, 4]</code>, không tạo thành dãy đơn điệu nghiêm ngặt. Do đó, 123456788 không phải là số fancy.</li>
</ul>

<p>Vì vậy, đáp án là 0.</p>
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
> Một số fancy có các chữ số tạo thành dãy đơn điệu nghiêm ngặt, hoặc tổng các chữ số của nó là một số tốt. Vì $r \le 10^{15}$, ta không thể duyệt toàn bộ đoạn.
>
> Dùng Digit DP với hiệu tiền tố để đếm các số như vậy trong $[l,r]$.
>
> Trạng thái lưu vị trí, tổng các chữ số, chữ số trước đó, tính đơn điệu (chưa xác định / tăng / giảm / bị phá vỡ), và cờ giới hạn trên. Tại nút lá, nếu dãy vẫn đơn điệu thì số đó được tính; nếu không, ta kiểm tra tổng các chữ số có phải là số tốt hay không.
>
> Tính chất tốt của tổng các chữ số (nhiều nhất là $9 \times 16$) có thể được kiểm tra nhanh. Lấy số lượng với $l-1$ trừ số lượng với $r$.

<!-- thinking:end -->

Trước tiên, ta định nghĩa hàm $\text{check}(s)$ để xác định một số nguyên $s$ có phải là số tốt hay không. Với $s < 100$, ta chỉ cần kiểm tra xem $s$ có là bội của 11 hay không; nếu có thì $s$ không phải là số tốt. Với $s \geq 100$, ta cần kiểm tra các chữ số của $s$ có tạo thành một dãy đơn điệu nghiêm ngặt hay không, tức là tăng nghiêm ngặt hoặc giảm nghiêm ngặt. Vì miền giá trị của tổng các chữ số nhỏ, khi tổng các chữ số lớn hơn $100$, ta chỉ cần kiểm tra quan hệ giữa chữ số hàng chục và chữ số hàng đơn vị của tổng các chữ số.

Tiếp theo, ta dùng Digit DP để đếm số lượng số fancy trong đoạn $[l, r]$. Ta định nghĩa hàm đệ quy $\text{dfs}(pos, s, prev, st, lim)$, trong đó:

- Số nguyên $pos$ là vị trí chữ số hiện tại đang được xử lý, từ cao xuống thấp.
- Số nguyên $s$ là tổng các chữ số hiện tại.
- Số nguyên $prev$ là giá trị của chữ số trước đó.
- Số nguyên $st$ là trạng thái của dãy chữ số hiện tại, trong đó trạng thái $0$ nghĩa là hiện có nhiều nhất một chữ số, trạng thái $1$ nghĩa là tăng nghiêm ngặt, trạng thái $2$ nghĩa là giảm nghiêm ngặt, và trạng thái $3$ nghĩa là không còn đơn điệu nghiêm ngặt.
- Boolean $lim$ cho biết chữ số hiện tại có bị giới hạn bởi cận trên hay không.

Trong hàm đệ quy, nếu $pos$ vượt quá độ dài của số, ta đã xử lý xong một số. Nếu trạng thái $st$ khác $3$, số đó là số tốt và ta trả về $1$; nếu không, ta gọi $\text{check}(s)$ để xác định tổng các chữ số có phải là số tốt hay không, trả về $1$ nếu đúng và $0$ nếu sai.

Trong quá trình đệ quy, ta liệt kê miền giá trị của chữ số hiện tại: nếu $lim$ là true, miền là $[0, \text{num}[pos]]$; nếu không, miền là $[0, 9]$. Với mỗi giá trị, ta cập nhật trạng thái $st$ rồi gọi đệ quy $\text{dfs}$ để xử lý chữ số tiếp theo.

Cuối cùng, ta tính riêng số lượng trong $[0, r]$ và $[0, l-1]$, rồi lấy hiệu của chúng.

Độ phức tạp thời gian là $O(D^3 \times \log^2 r)$ và độ phức tạp không gian là $O(D^2 \times \log^2 r)$, trong đó $D = 10$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countFancy(self, l: int, r: int) -> int:
        def check(s: int) -> bool:
            if s < 100:
                return s % 11 != 0
            return 1 < s // 10 % 10 < s % 10

        @cache
        def dfs(pos: int, s: int, prev: int, st: int, lim: bool) -> int:
            if pos >= len(num):
                if st != 3:
                    return 1
                return int(check(s))
            up = int(num[pos]) if lim else 9
            res = 0
            for i in range(up + 1):
                nxt_st = st
                if st == 0:
                    if prev == 0:
                        nxt_st = 0
                    elif i > prev:
                        nxt_st = 1
                    elif i < prev:
                        nxt_st = 2
                    else:
                        nxt_st = 3
                elif st == 1:
                    if i > prev:
                        nxt_st = 1
                    else:
                        nxt_st = 3
                elif st == 2:
                    if i < prev:
                        nxt_st = 2
                    else:
                        nxt_st = 3
                else:
                    nxt_st = 3
                res += dfs(pos + 1, s + i, i, nxt_st, lim and i == up)
            return res

        num = str(l - 1)
        a = dfs(0, 0, 0, 0, True)
        dfs.cache_clear()
        num = str(r)
        b = dfs(0, 0, 0, 0, True)
        return b - a
```

#### Java

```java
class Solution {
    private String num;
    private Long[][][][] f;

    public long countFancy(long l, long r) {
        num = String.valueOf(l - 1);
        init();
        long a = dfs(0, 0, 0, 0, true);

        num = String.valueOf(r);
        init();
        long b = dfs(0, 0, 0, 0, true);

        return b - a;
    }

    private void init() {
        int n = num.length();
        f = new Long[n][9 * n + 1][10][4];
    }

    private boolean check(int s) {
        if (s < 100) {
            return s % 11 != 0;
        }
        int mid = (s / 10) % 10;
        int last = s % 10;
        return mid > 1 && mid < last;
    }

    private long dfs(int pos, int s, int prev, int st, boolean lim) {
        if (pos >= num.length()) {
            if (st != 3) {
                return 1;
            }
            return check(s) ? 1 : 0;
        }

        if (!lim && f[pos][s][prev][st] != null) {
            return f[pos][s][prev][st];
        }

        int up = lim ? num.charAt(pos) - '0' : 9;
        long res = 0;

        for (int i = 0; i <= up; i++) {
            int nxtSt = st;

            if (st == 0) {
                if (prev == 0) {
                    nxtSt = 0;
                } else if (i > prev) {
                    nxtSt = 1;
                } else if (i < prev) {
                    nxtSt = 2;
                } else {
                    nxtSt = 3;
                }
            } else if (st == 1) {
                if (i > prev) {
                    nxtSt = 1;
                } else {
                    nxtSt = 3;
                }
            } else if (st == 2) {
                if (i < prev) {
                    nxtSt = 2;
                } else {
                    nxtSt = 3;
                }
            } else {
                nxtSt = 3;
            }

            res += dfs(pos + 1, s + i, i, nxtSt, lim && i == up);
        }

        if (!lim) {
            f[pos][s][prev][st] = res;
        }

        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countFancy(long long l, long long r) {
        auto check = [&](int s) -> bool {
            if (s < 100) {
                return s % 11 != 0;
            }
            int mid = (s / 10) % 10;
            int last = s % 10;
            return mid > 1 && mid < last;
        };

        string num = to_string(l - 1);
        int n = num.size();
        vector f(n, vector(9 * n + 1, vector(10, vector<long long>(4, -1))));

        auto dfs = [&](this auto&& dfs, int pos, int s, int prev, int st, bool lim) -> long long {
            if (pos >= n) {
                if (st != 3) return 1;
                return check(s) ? 1LL : 0LL;
            }

            if (!lim && f[pos][s][prev][st] != -1) {
                return f[pos][s][prev][st];
            }

            int up = lim ? num[pos] - '0' : 9;
            long long res = 0;

            for (int i = 0; i <= up; i++) {
                int nxtSt = st;

                if (st == 0) {
                    if (prev == 0)
                        nxtSt = 0;
                    else if (i > prev)
                        nxtSt = 1;
                    else if (i < prev)
                        nxtSt = 2;
                    else
                        nxtSt = 3;
                } else if (st == 1) {
                    if (i > prev)
                        nxtSt = 1;
                    else
                        nxtSt = 3;
                } else if (st == 2) {
                    if (i < prev)
                        nxtSt = 2;
                    else
                        nxtSt = 3;
                } else {
                    nxtSt = 3;
                }

                res += dfs(pos + 1, s + i, i, nxtSt, lim && i == up);
            }

            if (!lim) {
                f[pos][s][prev][st] = res;
            }

            return res;
        };

        long long a = dfs(0, 0, 0, 0, true);

        num = to_string(r);
        n = num.size();
        f.assign(n, vector(9 * n + 1, vector(10, vector<long long>(4, -1))));

        long long b = dfs(0, 0, 0, 0, true);

        return b - a;
    }
};
```

#### Go

```go
func countFancy(l int64, r int64) int64 {
	check := func(s int) bool {
		if s < 100 {
			return s%11 != 0
		}
		mid := (s / 10) % 10
		last := s % 10
		return mid > 1 && mid < last
	}

	var num string
	var n int
	var f [][][][]int64

	var dfs func(pos, s, prev, st int, lim bool) int64
	dfs = func(pos, s, prev, st int, lim bool) int64 {
		if pos >= n {
			if st != 3 {
				return 1
			}
			if check(s) {
				return 1
			}
			return 0
		}

		if !lim && f[pos][s][prev][st] != -1 {
			return f[pos][s][prev][st]
		}

		up := 9
		if lim {
			up = int(num[pos] - '0')
		}

		var res int64 = 0

		for i := 0; i <= up; i++ {
			nxtSt := st

			if st == 0 {
				if prev == 0 {
					nxtSt = 0
				} else if i > prev {
					nxtSt = 1
				} else if i < prev {
					nxtSt = 2
				} else {
					nxtSt = 3
				}
			} else if st == 1 {
				if i > prev {
					nxtSt = 1
				} else {
					nxtSt = 3
				}
			} else if st == 2 {
				if i < prev {
					nxtSt = 2
				} else {
					nxtSt = 3
				}
			} else {
				nxtSt = 3
			}

			res += dfs(pos+1, s+i, i, nxtSt, lim && i == up)
		}

		if !lim {
			f[pos][s][prev][st] = res
		}

		return res
	}

	calc := func(x int64) int64 {
		num = strconv.FormatInt(x, 10)
		n = len(num)

		f = make([][][][]int64, n)
		for i := 0; i < n; i++ {
			f[i] = make([][][]int64, 9*n+1)
			for j := 0; j <= 9*n; j++ {
				f[i][j] = make([][]int64, 10)
				for k := 0; k < 10; k++ {
					f[i][j][k] = make([]int64, 4)
					for t := 0; t < 4; t++ {
						f[i][j][k][t] = -1
					}
				}
			}
		}

		return dfs(0, 0, 0, 0, true)
	}

	return calc(r) - calc(l-1)
}
```

#### TypeScript

```ts
function countFancy(l: number, r: number): number {
    const check = (s: number): boolean => {
        if (s < 100) {
            return s % 11 !== 0;
        }
        const mid = Math.floor(s / 10) % 10;
        const last = s % 10;
        return mid > 1 && mid < last;
    };

    let num: string;
    let n: number;
    let f: number[][][][];

    const dfs = (pos: number, s: number, prev: number, st: number, lim: boolean): number => {
        if (pos >= n) {
            if (st !== 3) return 1;
            return check(s) ? 1 : 0;
        }

        if (!lim && f[pos][s][prev][st] !== -1) {
            return f[pos][s][prev][st];
        }

        const up = lim ? Number(num[pos]) : 9;
        let res = 0;

        for (let i = 0; i <= up; i++) {
            let nxtSt = st;

            if (st === 0) {
                if (prev === 0) nxtSt = 0;
                else if (i > prev) nxtSt = 1;
                else if (i < prev) nxtSt = 2;
                else nxtSt = 3;
            } else if (st === 1) {
                if (i > prev) nxtSt = 1;
                else nxtSt = 3;
            } else if (st === 2) {
                if (i < prev) nxtSt = 2;
                else nxtSt = 3;
            } else {
                nxtSt = 3;
            }

            res += dfs(pos + 1, s + i, i, nxtSt, lim && i === up);
        }

        if (!lim) {
            f[pos][s][prev][st] = res;
        }

        return res;
    };

    const calc = (x: number): number => {
        num = x.toString();
        n = num.length;

        f = Array.from({ length: n }, () =>
            Array.from({ length: 9 * n + 1 }, () =>
                Array.from({ length: 10 }, () => Array(4).fill(-1)),
            ),
        );

        return dfs(0, 0, 0, 0, true);
    };

    return calc(r) - calc(l - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
