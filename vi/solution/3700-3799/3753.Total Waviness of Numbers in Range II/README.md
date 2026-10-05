---
comments: true
difficulty: Hard
rating: 2296
source: Biweekly Contest 170 Q4
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [3753. Total Waviness of Numbers in Range II](https://leetcode.com/problems/total-waviness-of-numbers-in-range-ii)

[中文文档](/solution/3700-3799/3753.Total%20Waviness%20of%20Numbers%20in%20Range%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>num1</code> và <code>num2</code> biểu diễn một đoạn <strong>bao gồm cả hai đầu mút</strong> <code>[num1, num2]</code>.</p>

<p><strong>Độ gợn</strong> của một số được định nghĩa là tổng số <strong>đỉnh</strong> và <strong>đáy</strong> của số đó:</p>

<ul>
	<li>Một chữ số là <strong>đỉnh</strong> nếu nó <strong>lớn hơn nghiêm ngặt</strong> cả hai chữ số liền kề.</li>
	<li>Một chữ số là <strong>đáy</strong> nếu nó <strong>nhỏ hơn nghiêm ngặt</strong> cả hai chữ số liền kề.</li>
	<li>Chữ số đầu tiên và chữ số cuối cùng của một số <strong>không thể</strong> là đỉnh hoặc đáy.</li>
	<li>Mọi số có ít hơn 3 chữ số đều có độ gợn bằng 0.</li>
</ul>

Trả về tổng độ gợn của tất cả các số trong đoạn <code>[num1, num2]</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num1 = 120, num2 = 130</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong đoạn <code>[120, 130]</code>:</p>

<ul>
	<li><code>120</code>: chữ số ở giữa 2 là một đỉnh, độ gợn = 1.</li>
	<li><code>121</code>: chữ số ở giữa 2 là một đỉnh, độ gợn = 1.</li>
	<li><code>130</code>: chữ số ở giữa 3 là một đỉnh, độ gợn = 1.</li>
	<li>Mọi số khác trong đoạn đều có độ gợn bằng 0.</li>
</ul>

<p>Vậy tổng độ gợn là <code>1 + 1 + 1 = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num1 = 198, num2 = 202</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong đoạn <code>[198, 202]</code>:</p>

<ul>
	<li><code>198</code>: chữ số ở giữa 9 là một đỉnh, độ gợn = 1.</li>
	<li><code>201</code>: chữ số ở giữa 0 là một đáy, độ gợn = 1.</li>
	<li><code>202</code>: chữ số ở giữa 0 là một đáy, độ gợn = 1.</li>
	<li>Mọi số khác trong đoạn đều có độ gợn bằng 0.</li>
</ul>

<p>Vậy tổng độ gợn là <code>1 + 1 + 1 = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num1 = 4848, num2 = 4848</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số <code>4848</code>: chữ số thứ hai 8 là một đỉnh, còn chữ số thứ ba 4 là một đáy, nên độ gợn bằng 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num1 &lt;= num2 &lt;= 10<sup>15</sup></code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Vì cận trên là $10^{15}$, không thể mô phỏng từng số. Tổng trên đoạn được tính bằng $calc(num2)-calc(num1-1)$. Khi điền các chữ số từ trái sang phải, việc xác định đỉnh và đáy chỉ phụ thuộc vào hai chữ số vừa điền trước đó. Trạng thái DP lưu vị trí, hai chữ số đó, việc số đã bắt đầu hay chưa và việc hiện tại có còn bị giới hạn bởi cận trên hay không; đồng thời tích lũy cả số lượng và tổng độ gợn.

<!-- thinking:end -->

Ta cần tổng độ gợn của mọi số trong $[num1, num2]$. Chuyển truy vấn trên đoạn thành $calc(num2) - calc(num1 - 1)$, trong đó $calc(x)$ là tổng độ gợn trong $[1, x]$.

Dùng digit DP từ chữ số có trọng số cao nhất. Gọi $dfs(pos, prev2, prev1, started, limit)$ là số lượng các số hợp lệ và tổng độ gợn của chúng khi đang điền vị trí $pos$, hai chữ số trước đó là $prev2$ và $prev1$ (dùng $10$ nếu chữ số tương ứng chưa được điền), $started$ cho biết đã đặt một chữ số khác 0 ở đầu số hay chưa, còn $limit$ cho biết ta vẫn đang bị giới hạn bởi cận trên.

Xét chữ số hiện tại $d$. Nếu đã đặt ít nhất hai chữ số và $prev1$ lớn hơn nghiêm ngặt (hoặc nhỏ hơn nghiêm ngặt) cả $prev2$ và $d$, thì $prev1$ là một đỉnh (hoặc đáy) và đóng góp $1$ vào độ gợn, nhân với số cách điền các chữ số còn lại.

Độ phức tạp thời gian là $O(\log x)$, độ phức tạp không gian là $O(\log x)$, trong đó $x$ là cận trên.

Các bài tương tự:

- [3751. Total Waviness of Numbers in Range I](https://github.com/doocs/leetcode/blob/main/solution/3700-3799/3751.Total%20Waviness%20of%20Numbers%20in%20Range%20I/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def totalWaviness(self, num1: int, num2: int) -> int:
        def calc(x: int) -> int:
            if x < 0:
                return 0
            s = str(x)

            @cache
            def dfs(
                pos: int, prev2: int, prev1: int, started: int, limit: bool
            ) -> tuple:
                if pos == len(s):
                    return (started, 0)
                up = int(s[pos]) if limit else 9
                cnt = wav = 0
                for d in range(up + 1):
                    nlimit = limit and d == up
                    add = 0
                    if started == 0:
                        if d == 0:
                            ns, np2, np1 = 0, 10, 10
                        else:
                            ns, np2, np1 = 1, 10, d
                    else:
                        ns, np2, np1 = 1, prev1, d
                        if prev2 != 10 and (
                            (prev1 > prev2 and prev1 > d)
                            or (prev1 < prev2 and prev1 < d)
                        ):
                            add = 1
                    c, w = dfs(pos + 1, np2, np1, ns, nlimit)
                    cnt += c
                    wav += w + c * add
                return cnt, wav

            return dfs(0, 10, 10, 0, True)[1]

        return calc(num2) - calc(num1 - 1)
```

#### Java

```java
class Solution {
    private char[] cs;
    private long[][][][] cnt;
    private long[][][][] wav;

    public long totalWaviness(long num1, long num2) {
        return calc(num2) - calc(num1 - 1);
    }

    private long calc(long x) {
        if (x < 0) {
            return 0;
        }
        cs = Long.toString(x).toCharArray();
        int n = cs.length;
        cnt = new long[n][11][11][2];
        wav = new long[n][11][11][2];
        for (int i = 0; i < n; ++i) {
            for (int a = 0; a < 11; ++a) {
                for (int b = 0; b < 11; ++b) {
                    Arrays.fill(cnt[i][a][b], -1);
                    Arrays.fill(wav[i][a][b], -1);
                }
            }
        }
        return dfs(0, 10, 10, 0, true)[1];
    }

    private long[] dfs(int pos, int prev2, int prev1, int started, boolean limit) {
        if (pos == cs.length) {
            return new long[] {started, 0};
        }
        if (!limit && cnt[pos][prev2][prev1][started] != -1) {
            return new long[] {cnt[pos][prev2][prev1][started], wav[pos][prev2][prev1][started]};
        }
        int up = limit ? cs[pos] - '0' : 9;
        long c = 0, w = 0;
        for (int d = 0; d <= up; ++d) {
            boolean nlimit = limit && d == up;
            int ns, np2, np1, add = 0;
            if (started == 0) {
                if (d == 0) {
                    ns = 0;
                    np2 = 10;
                    np1 = 10;
                } else {
                    ns = 1;
                    np2 = 10;
                    np1 = d;
                }
            } else {
                ns = 1;
                np2 = prev1;
                np1 = d;
                if (prev2 != 10 && ((prev1 > prev2 && prev1 > d) || (prev1 < prev2 && prev1 < d))) {
                    add = 1;
                }
            }
            long[] t = dfs(pos + 1, np2, np1, ns, nlimit);
            c += t[0];
            w += t[1] + t[0] * add;
        }
        if (!limit) {
            cnt[pos][prev2][prev1][started] = c;
            wav[pos][prev2][prev1][started] = w;
        }
        return new long[] {c, w};
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long totalWaviness(long long num1, long long num2) {
        return calc(num2) - calc(num1 - 1);
    }

private:
    string s;
    long long fCnt[20][11][11][2];
    long long fWav[20][11][11][2];
    bool vis[20][11][11][2];

    long long calc(long long x) {
        if (x < 0) {
            return 0;
        }
        s = to_string(x);
        memset(vis, 0, sizeof(vis));
        return dfs(0, 10, 10, 0, true).second;
    }

    pair<long long, long long> dfs(int pos, int prev2, int prev1, int started, bool limit) {
        if (pos == s.size()) {
            return {started, 0};
        }
        if (!limit && vis[pos][prev2][prev1][started]) {
            return {fCnt[pos][prev2][prev1][started], fWav[pos][prev2][prev1][started]};
        }
        int up = limit ? s[pos] - '0' : 9;
        long long c = 0, w = 0;
        for (int d = 0; d <= up; ++d) {
            bool nlimit = limit && d == up;
            int ns, np2, np1, add = 0;
            if (started == 0) {
                if (d == 0) {
                    ns = 0;
                    np2 = 10;
                    np1 = 10;
                } else {
                    ns = 1;
                    np2 = 10;
                    np1 = d;
                }
            } else {
                ns = 1;
                np2 = prev1;
                np1 = d;
                if (prev2 != 10 && ((prev1 > prev2 && prev1 > d) || (prev1 < prev2 && prev1 < d))) {
                    add = 1;
                }
            }
            auto [tc, tw] = dfs(pos + 1, np2, np1, ns, nlimit);
            c += tc;
            w += tw + tc * add;
        }
        if (!limit) {
            vis[pos][prev2][prev1][started] = true;
            fCnt[pos][prev2][prev1][started] = c;
            fWav[pos][prev2][prev1][started] = w;
        }
        return {c, w};
    }
};
```

#### Go

```go
import "strconv"

func totalWaviness(num1 int64, num2 int64) int64 {
	return calc(num2) - calc(num1-1)
}

func calc(x int64) int64 {
	if x < 0 {
		return 0
	}
	s := strconv.FormatInt(x, 10)
	n := len(s)
	var fCnt, fWav [20][11][11][2]int64
	var vis [20][11][11][2]bool
	var dfs func(pos, prev2, prev1, started int, limit bool) (int64, int64)
	dfs = func(pos, prev2, prev1, started int, limit bool) (int64, int64) {
		if pos == n {
			return int64(started), 0
		}
		if !limit && vis[pos][prev2][prev1][started] {
			return fCnt[pos][prev2][prev1][started], fWav[pos][prev2][prev1][started]
		}
		up := 9
		if limit {
			up = int(s[pos] - '0')
		}
		var c, w int64
		for d := 0; d <= up; d++ {
			nlimit := limit && d == up
			ns, np2, np1, add := started, prev1, d, 0
			if started == 0 {
				if d == 0 {
					ns, np2, np1 = 0, 10, 10
				} else {
					ns, np2, np1 = 1, 10, d
				}
			} else if prev2 != 10 && ((prev1 > prev2 && prev1 > d) || (prev1 < prev2 && prev1 < d)) {
				add = 1
			}
			tc, tw := dfs(pos+1, np2, np1, ns, nlimit)
			c += tc
			w += tw + tc*int64(add)
		}
		if !limit {
			vis[pos][prev2][prev1][started] = true
			fCnt[pos][prev2][prev1][started] = c
			fWav[pos][prev2][prev1][started] = w
		}
		return c, w
	}
	_, wav := dfs(0, 10, 10, 0, true)
	return wav
}
```

#### C

```c
static int len, digits[20];
static long long fCnt[20][11][11][2];
static long long fWav[20][11][11][2];
static char vis[20][11][11][2];
static long long cnt, wav;

static void dfs(int pos, int prev2, int prev1, int started, int limit) {
    if (pos == len) {
        cnt = started;
        wav = 0;
        return;
    }
    if (!limit && vis[pos][prev2][prev1][started]) {
        cnt = fCnt[pos][prev2][prev1][started];
        wav = fWav[pos][prev2][prev1][started];
        return;
    }
    int up = limit ? digits[pos] : 9;
    long long c = 0, w = 0;
    for (int d = 0; d <= up; ++d) {
        int nlimit = limit && d == up;
        int ns, np2, np1, add = 0;
        if (started == 0) {
            if (d == 0) {
                ns = 0;
                np2 = 10;
                np1 = 10;
            } else {
                ns = 1;
                np2 = 10;
                np1 = d;
            }
        } else {
            ns = 1;
            np2 = prev1;
            np1 = d;
            if (prev2 != 10 && ((prev1 > prev2 && prev1 > d) || (prev1 < prev2 && prev1 < d))) {
                add = 1;
            }
        }
        dfs(pos + 1, np2, np1, ns, nlimit);
        c += cnt;
        w += wav + add * cnt;
    }
    if (!limit) {
        vis[pos][prev2][prev1][started] = 1;
        fCnt[pos][prev2][prev1][started] = c;
        fWav[pos][prev2][prev1][started] = w;
    }
    cnt = c;
    wav = w;
}

static long long calc(long long x) {
    if (x < 0) {
        return 0;
    }
    len = 0;
    if (x == 0) {
        digits[len++] = 0;
    } else {
        int buf[20];
        int l = 0;
        while (x) {
            buf[l++] = x % 10;
            x /= 10;
        }
        for (int i = l - 1; i >= 0; --i) {
            digits[len++] = buf[i];
        }
    }
    memset(vis, 0, sizeof(vis));
    dfs(0, 10, 10, 0, 1);
    return wav;
}

long long totalWaviness(long long num1, long long num2) {
    return calc(num2) - calc(num1 - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
