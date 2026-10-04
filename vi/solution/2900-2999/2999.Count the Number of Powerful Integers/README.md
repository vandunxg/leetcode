---
comments: true
difficulty: Hard
rating: 2351
source: Biweekly Contest 121 Q4
tags:
    - Math
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2999. Count the Number of Powerful Integers](https://leetcode.com/problems/count-the-number-of-powerful-integers)

[中文文档](/solution/2900-2999/2999.Count%20the%20Number%20of%20Powerful%20Integers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho ba số nguyên <code>start</code>, <code>finish</code> và <code>limit</code>. Bạn cũng được cho một chuỗi <code>s</code> được đánh chỉ số từ <strong>0</strong>, biểu diễn một số nguyên <strong>dương</strong>.</p>

<p>Một số nguyên <strong>dương</strong> <code>x</code> được gọi là <strong>số mạnh</strong> nếu nó kết thúc bằng <code>s</code> (nói cách khác, <code>s</code> là <strong>hậu tố</strong> của <code>x</code>) và mỗi chữ số trong <code>x</code> không lớn hơn <code>limit</code>.</p>

<p>Trả về <em><strong>tổng số</strong> số mạnh trong đoạn</em> <code>[start..finish]</code>.</p>

<p>Chuỗi <code>x</code> là hậu tố của chuỗi <code>y</code> khi và chỉ khi <code>x</code> là một chuỗi con của <code>y</code> bắt đầu từ một chỉ số nào đó (<strong>bao gồm </strong><code>0</code>) trong <code>y</code> và kéo dài đến chỉ số <code>y.length - 1</code>. Ví dụ, <code>25</code> là hậu tố của <code>5125</code>, còn <code>512</code> thì không.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> start = 1, finish = 6000, limit = 4, s = &quot;124&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Các số mạnh trong đoạn [1..6000] là 124, 1124, 2124, 3124 và 4124. Tất cả các số này đều có mỗi chữ số &lt;= 4 và có &quot;124&quot; làm hậu tố. Lưu ý rằng 5124 không phải là số mạnh vì chữ số đầu tiên là 5, lớn hơn 4.
Có thể chứng minh rằng chỉ có 5 số mạnh trong đoạn này.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> start = 15, finish = 215, limit = 6, s = &quot;10&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các số mạnh trong đoạn [15..215] là 110 và 210. Tất cả các số này đều có mỗi chữ số &lt;= 6 và có &quot;10&quot; làm hậu tố.
Có thể chứng minh rằng chỉ có 2 số mạnh trong đoạn này.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> start = 1000, finish = 2000, limit = 4, s = &quot;3000&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mọi số trong đoạn [1000..2000] đều nhỏ hơn 3000, vì vậy &quot;3000&quot; không thể là hậu tố của bất kỳ số nào trong đoạn này.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= start &lt;= finish &lt;= 10<sup>15</sup></code></li>
	<li><code>1 &lt;= limit &lt;= 9</code></li>
	<li><code>1 &lt;= s.length &lt;= floor(log<sub>10</sub>(finish)) + 1</code></li>
	<li><code>s</code> chỉ gồm các chữ số không lớn hơn <code>limit</code>.</li>
	<li><code>s</code> không có số 0 ở đầu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các số trong $[start,finish]$ có các chữ số không lớn hơn $limit$ và kết thúc bằng $s$. Số lượng trên đoạn là $f(finish)-f(start-1)$. Digit DP điền từ trái sang phải: các chữ số đầu phải thỏa $limit$ và giới hạn chặt; $|s|$ chữ số cuối phải bằng $s$ (và khi đang ở giới hạn chặt, $s$ không được lớn hơn phần còn lại của giới hạn).
>
> Nếu $t$ ngắn hơn $s$ thì số lượng là $0$. Memoize theo $(pos,lim)$; giới hạn có nhiều nhất $16$ chữ số.

<!-- thinking:end -->

Bài toán này về cơ bản yêu cầu tìm số lượng các số trong đoạn $[l, .., r]$ thỏa mãn các điều kiện. Số lượng phụ thuộc vào số chữ số và giá trị của từng chữ số. Ta có thể giải quyết bài toán bằng Digit DP, trong đó kích thước của số ảnh hưởng không đáng kể đến độ phức tạp.

Với đoạn $[l, .., r]$, ta thường chuyển thành hai bài toán con: $[1, .., r]$ và $[1, .., l - 1]$, tức là:

$$
ans = \sum_{i=1}^{r} ans_i - \sum_{i=1}^{l-1} ans_i
$$

Trong bài toán này, ta tính số lượng các số trong $[1, \textit{finish}]$ thỏa mãn các điều kiện, rồi trừ đi số lượng các số trong $[1, \textit{start} - 1]$ thỏa mãn các điều kiện để nhận được đáp án cuối cùng.

Ta dùng memoization để triển khai Digit DP. Bắt đầu từ chữ số cao nhất, ta đệ quy tính số lượng các số hợp lệ, cộng dồn kết quả theo từng lớp, rồi cuối cùng trả về đáp án từ điểm bắt đầu.

Các bước cơ bản như sau:

1. Chuyển $\textit{start}$ và $\textit{finish}$ thành chuỗi để dễ thao tác hơn trong Digit DP.
2. Thiết kế hàm $\textit{dfs}(\textit{pos}, \textit{lim})$, biểu thị số lượng các số hợp lệ bắt đầu từ chữ số thứ $\textit{pos}$, với điều kiện giới hạn hiện tại là $\textit{lim}$.
3. Nếu số chữ số tối đa nhỏ hơn độ dài của $\textit{s}$, trả về 0.
4. Nếu số chữ số còn lại bằng độ dài của $\textit{s}$, kiểm tra xem số hiện tại có thỏa mãn điều kiện hay không rồi trả về 1 hoặc 0.
5. Nếu không, tính giới hạn trên của chữ số hiện tại là $\textit{up} = \min(\textit{lim} ? \textit{t}[\textit{pos}] : 9, \textit{limit})$. Sau đó duyệt các chữ số $i$ từ 0 đến $\textit{up}$, gọi đệ quy $\textit{dfs}(\textit{pos} + 1, \textit{lim} \&\& i == \textit{t}[\textit{pos}])$ và cộng dồn kết quả.
6. Nếu $\textit{lim}$ là false, lưu kết quả hiện tại vào cache để tránh tính toán lặp lại.
7. Cuối cùng, trả về kết quả.

Đáp án là số lượng các số hợp lệ trong $[1, \textit{finish}]$ trừ đi số lượng các số hợp lệ trong $[1, \textit{start} - 1]$.

Độ phức tạp thời gian là $O(\log M \times D)$ và độ phức tạp không gian là $O(\log M)$, trong đó $M$ là giới hạn trên của số cần xét và $D = 10$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfPowerfulInt(self, start: int, finish: int, limit: int, s: str) -> int:
        @cache
        def dfs(pos: int, lim: int) -> int:
            if len(t) < n:
                return 0
            if len(t) - pos == n:
                return int(s <= t[pos:]) if lim else 1
            up = min(int(t[pos]) if lim else 9, limit)
            ans = 0
            for i in range(up + 1):
                ans += dfs(pos + 1, lim and i == int(t[pos]))
            return ans

        n = len(s)
        t = str(start - 1)
        a = dfs(0, True)
        dfs.cache_clear()
        t = str(finish)
        b = dfs(0, True)
        return b - a
```

#### Java

```java
class Solution {
    private String s;
    private String t;
    private Long[] f;
    private int limit;

    public long numberOfPowerfulInt(long start, long finish, int limit, String s) {
        this.s = s;
        this.limit = limit;
        t = String.valueOf(start - 1);
        f = new Long[20];
        long a = dfs(0, true);
        t = String.valueOf(finish);
        f = new Long[20];
        long b = dfs(0, true);
        return b - a;
    }

    private long dfs(int pos, boolean lim) {
        if (t.length() < s.length()) {
            return 0;
        }
        if (!lim && f[pos] != null) {
            return f[pos];
        }
        if (t.length() - pos == s.length()) {
            return lim ? (s.compareTo(t.substring(pos)) <= 0 ? 1 : 0) : 1;
        }
        int up = lim ? t.charAt(pos) - '0' : 9;
        up = Math.min(up, limit);
        long ans = 0;
        for (int i = 0; i <= up; ++i) {
            ans += dfs(pos + 1, lim && i == (t.charAt(pos) - '0'));
        }
        if (!lim) {
            f[pos] = ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long numberOfPowerfulInt(long long start, long long finish, int limit, string s) {
        string t = to_string(start - 1);
        long long f[20];
        memset(f, -1, sizeof(f));

        auto dfs = [&](this auto&& dfs, int pos, int lim) -> long long {
            if (t.size() < s.size()) {
                return 0;
            }
            if (!lim && f[pos] != -1) {
                return f[pos];
            }
            if (t.size() - pos == s.size()) {
                return lim ? s <= t.substr(pos) : 1;
            }
            long long ans = 0;
            int up = min(lim ? t[pos] - '0' : 9, limit);
            for (int i = 0; i <= up; ++i) {
                ans += dfs(pos + 1, lim && i == (t[pos] - '0'));
            }
            if (!lim) {
                f[pos] = ans;
            }
            return ans;
        };

        long long a = dfs(0, true);
        t = to_string(finish);
        memset(f, -1, sizeof(f));
        long long b = dfs(0, true);
        return b - a;
    }
};
```

#### Go

```go
func numberOfPowerfulInt(start, finish int64, limit int, s string) int64 {
	t := strconv.FormatInt(start-1, 10)
	f := make([]int64, 20)
	for i := range f {
		f[i] = -1
	}

	var dfs func(int, bool) int64
	dfs = func(pos int, lim bool) int64 {
		if len(t) < len(s) {
			return 0
		}
		if !lim && f[pos] != -1 {
			return f[pos]
		}
		if len(t)-pos == len(s) {
			if lim {
				if s <= t[pos:] {
					return 1
				}
				return 0
			}
			return 1
		}

		ans := int64(0)
		up := 9
		if lim {
			up = int(t[pos] - '0')
		}
		up = min(up, limit)
		for i := 0; i <= up; i++ {
			ans += dfs(pos+1, lim && i == int(t[pos]-'0'))
		}
		if !lim {
			f[pos] = ans
		}
		return ans
	}

	a := dfs(0, true)
	t = strconv.FormatInt(finish, 10)
	for i := range f {
		f[i] = -1
	}
	b := dfs(0, true)
	return b - a
}
```

#### TypeScript

```ts
function numberOfPowerfulInt(start: number, finish: number, limit: number, s: string): number {
    let t: string = (start - 1).toString();
    let f: number[] = Array(20).fill(-1);

    const dfs = (pos: number, lim: boolean): number => {
        if (t.length < s.length) {
            return 0;
        }
        if (!lim && f[pos] !== -1) {
            return f[pos];
        }
        if (t.length - pos === s.length) {
            if (lim) {
                return s <= t.substring(pos) ? 1 : 0;
            }
            return 1;
        }

        let ans: number = 0;
        const up: number = Math.min(lim ? +t[pos] : 9, limit);
        for (let i = 0; i <= up; i++) {
            ans += dfs(pos + 1, lim && i === +t[pos]);
        }

        if (!lim) {
            f[pos] = ans;
        }
        return ans;
    };

    const a: number = dfs(0, true);
    t = finish.toString();
    f = Array(20).fill(-1);
    const b: number = dfs(0, true);

    return b - a;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_powerful_int(start: i64, finish: i64, limit: i32, s: String) -> i64 {
        fn count(x: i64, limit: i32, s: &str) -> i64 {
            let t = x.to_string();
            if t.len() < s.len() {
                return 0;
            }

            let t_bytes: Vec<u8> = t.bytes().collect();
            let mut f = [-1_i64; 20];

            fn dfs(
                pos: usize,
                lim: bool,
                t: &[u8],
                s: &str,
                limit: i32,
                f: &mut [i64; 20],
            ) -> i64 {
                if t.len() < s.len() {
                    return 0;
                }

                if !lim && f[pos] != -1 {
                    return f[pos];
                }

                if t.len() - pos == s.len() {
                    if lim {
                        let suffix = &t[pos..];
                        let suffix_str = String::from_utf8_lossy(suffix);
                        return if suffix_str.as_ref() >= s { 1 } else { 0 };
                    } else {
                        return 1;
                    }
                }

                let mut ans = 0;
                let up = if lim {
                    (t[pos] - b'0').min(limit as u8)
                } else {
                    limit as u8
                };

                for i in 0..=up {
                    let next_lim = lim && i == t[pos] - b'0';
                    ans += dfs(pos + 1, next_lim, t, s, limit, f);
                }

                if !lim {
                    f[pos] = ans;
                }

                ans
            }

            dfs(0, true, &t_bytes, s, limit, &mut f)
        }

        let a = count(start - 1, limit, &s);
        let b = count(finish, limit, &s);
        b - a
    }
}
```

#### C#

```cs
public class Solution {
    private string s;
    private string t;
    private long?[] f;
    private int limit;

    public long NumberOfPowerfulInt(long start, long finish, int limit, string s) {
        this.s = s;
        this.limit = limit;
        t = (start - 1).ToString();
        f = new long?[20];
        long a = Dfs(0, true);
        t = finish.ToString();
        f = new long?[20];
        long b = Dfs(0, true);
        return b - a;
    }

    private long Dfs(int pos, bool lim) {
        if (t.Length < s.Length) {
            return 0;
        }
        if (!lim && f[pos].HasValue) {
            return f[pos].Value;
        }
        if (t.Length - pos == s.Length) {
            return lim ? (string.Compare(s, t.Substring(pos)) <= 0 ? 1 : 0) : 1;
        }
        int up = lim ? t[pos] - '0' : 9;
        up = Math.Min(up, limit);
        long ans = 0;
        for (int i = 0; i <= up; ++i) {
            ans += Dfs(pos + 1, lim && i == (t[pos] - '0'));
        }
        if (!lim) {
            f[pos] = ans;
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
