---
comments: true
difficulty: Hard
rating: 2324
source: Biweekly Contest 111 Q4
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [2827. Number of Beautiful Integers in the Range](https://leetcode.com/problems/number-of-beautiful-integers-in-the-range)

[中文文档](/solution/2800-2899/2827.Number%20of%20Beautiful%20Integers%20in%20the%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Cho các số nguyên dương <code>low</code>, <code>high</code> và <code>k</code>.</p>

<p>Một số được gọi là <strong>đẹp</strong> nếu thỏa mãn cả hai điều kiện sau:</p>

<ul>
	<li>Số lượng chữ số chẵn trong số đó bằng số lượng chữ số lẻ.</li>
	<li>Số đó chia hết cho <code>k</code>.</li>
</ul>

<p>Trả về <em>số lượng số nguyên đẹp trong đoạn</em> <code>[low, high]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> low = 10, high = 20, k = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có 2 số nguyên đẹp trong đoạn đã cho: [12,18].
- 12 là số đẹp vì nó chứa 1 chữ số lẻ và 1 chữ số chẵn, đồng thời chia hết cho k = 3.
- 18 là số đẹp vì nó chứa 1 chữ số lẻ và 1 chữ số chẵn, đồng thời chia hết cho k = 3.
Ngoài ra, ta có thể thấy rằng:
- 16 không phải số đẹp vì nó không chia hết cho k = 3.
- 15 không phải số đẹp vì số lượng chữ số chẵn và lẻ của nó không bằng nhau.
Có thể chứng minh rằng chỉ có 2 số nguyên đẹp trong đoạn đã cho.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> low = 1, high = 10, k = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có 1 số nguyên đẹp trong đoạn đã cho: [10].
- 10 là số đẹp vì nó chứa 1 chữ số lẻ và 1 chữ số chẵn, đồng thời chia hết cho k = 1.
Có thể chứng minh rằng chỉ có 1 số nguyên đẹp trong đoạn đã cho.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> low = 5, high = 5, k = 2
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có số nguyên đẹp nào trong đoạn đã cho.
- 5 không phải số đẹp vì nó không chia hết cho k = 2 và số lượng chữ số chẵn, lẻ của nó không bằng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt; low &lt;= high &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt; k &lt;= 20</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Một số nguyên đẹp có số lượng chữ số chẵn và lẻ bằng nhau, đồng thời chia hết cho $k$. Vì đoạn số rất lớn, ta tính $F(high)-F(low-1)$ bằng Digit DP trên phần dư $mod$, hiệu giữa số chữ số lẻ và chẵn $diff$ (dịch thêm $10$), các số 0 ở đầu và giới hạn trên. Các số 0 ở đầu không ảnh hưởng đến $diff$.

<!-- thinking:end -->

Ta nhận thấy đề bài yêu cầu đếm số lượng số nguyên đẹp trong đoạn $[low, high]$. Với một bài toán trên đoạn $[l,..r]$ như vậy, thông thường ta có thể chuyển thành việc tìm đáp án cho $[1, r]$ và $[1, l-1]$, sau đó lấy đáp án thứ nhất trừ đáp án thứ hai. Ngoài ra, bài toán chỉ liên quan đến mối quan hệ giữa các chữ số chứ không phụ thuộc vào giá trị cụ thể, nên ta có thể sử dụng Digit DP để giải quyết.

Ta thiết kế hàm $dfs(pos, mod, diff, lead, limit)$, biểu diễn số lượng cách khi đang xử lý chữ số thứ $pos$, phần dư của số hiện tại khi chia cho $k$ là $mod$, hiệu giữa số chữ số lẻ và chẵn của số hiện tại là $diff$, số hiện tại có các số 0 ở đầu hay không là $lead$, và số hiện tại đã chạm giới hạn trên hay chưa là $limit$.

Logic thực thi của hàm $dfs(pos, mod, diff, lead, limit)$ như sau:

Nếu $pos$ vượt quá độ dài của $num$, nghĩa là ta đã xử lý tất cả các chữ số. Nếu lúc này $mod=0$ và $diff=0$, số hiện tại thỏa mãn yêu cầu của bài toán, nên ta trả về $1$, ngược lại trả về $0$.

Nếu chưa, ta tính giới hạn trên $up$ của chữ số hiện tại, sau đó duyệt chữ số $i$ trong đoạn $[0,..up]$:

- Nếu $i=0$ và $lead$ là đúng, nghĩa là số hiện tại chỉ chứa các số 0 ở đầu. Ta đệ quy tính giá trị của $dfs(pos + 1, mod, diff, 1, limit\ and\ i=up)$ rồi cộng vào đáp án.
- Nếu không, ta cập nhật giá trị của $diff$ dựa trên tính chẵn lẻ của $i$, sau đó đệ quy tính giá trị của $dfs(pos + 1, (mod \times 10 + i) \bmod k, diff, 0, limit\ and\ i=up)$ rồi cộng vào đáp án.

Cuối cùng, ta trả về đáp án.

Trong hàm chính, ta tính lần lượt các đáp án $a$ và $b$ cho $[1, high]$ và $[1, low-1]$. Đáp án cuối cùng là $a-b$.

Độ phức tạp thời gian là $O((\log M)^2 \times k \times |\Sigma|)$, và độ phức tạp không gian là $O((\log M)^2 \times k)$, trong đó $M$ biểu diễn kích thước của số $high$, còn $|\Sigma|$ biểu diễn tập chữ số.

Bài toán tương tự:

- [2719. Count of Integers](https://github.com/doocs/leetcode/blob/main/solution/2700-2799/2719.Count%20of%20Integers/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfBeautifulIntegers(self, low: int, high: int, k: int) -> int:
        @cache
        def dfs(pos: int, mod: int, diff: int, lead: int, limit: int) -> int:
            if pos >= len(s):
                return mod == 0 and diff == 10
            up = int(s[pos]) if limit else 9
            ans = 0
            for i in range(up + 1):
                if i == 0 and lead:
                    ans += dfs(pos + 1, mod, diff, 1, limit and i == up)
                else:
                    nxt = diff + (1 if i % 2 == 1 else -1)
                    ans += dfs(pos + 1, (mod * 10 + i) % k, nxt, 0, limit and i == up)
            return ans

        s = str(high)
        a = dfs(0, 0, 10, 1, 1)
        dfs.cache_clear()
        s = str(low - 1)
        b = dfs(0, 0, 10, 1, 1)
        return a - b
```

#### Java

```java
class Solution {
    private String s;
    private int k;
    private Integer[][][] f = new Integer[11][21][21];

    public int numberOfBeautifulIntegers(int low, int high, int k) {
        this.k = k;
        s = String.valueOf(high);
        int a = dfs(0, 0, 10, true, true);
        s = String.valueOf(low - 1);
        f = new Integer[11][21][21];
        int b = dfs(0, 0, 10, true, true);
        return a - b;
    }

    private int dfs(int pos, int mod, int diff, boolean lead, boolean limit) {
        if (pos >= s.length()) {
            return mod == 0 && diff == 10 ? 1 : 0;
        }
        if (!lead && !limit && f[pos][mod][diff] != null) {
            return f[pos][mod][diff];
        }
        int ans = 0;
        int up = limit ? s.charAt(pos) - '0' : 9;
        for (int i = 0; i <= up; ++i) {
            if (i == 0 && lead) {
                ans += dfs(pos + 1, mod, diff, true, limit && i == up);
            } else {
                int nxt = diff + (i % 2 == 1 ? 1 : -1);
                ans += dfs(pos + 1, (mod * 10 + i) % k, nxt, false, limit && i == up);
            }
        }
        if (!lead && !limit) {
            f[pos][mod][diff] = ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfBeautifulIntegers(int low, int high, int k) {
        int f[11][21][21];
        memset(f, -1, sizeof(f));
        string s = to_string(high);

        function<int(int, int, int, bool, bool)> dfs = [&](int pos, int mod, int diff, bool lead, bool limit) {
            if (pos >= s.size()) {
                return mod == 0 && diff == 10 ? 1 : 0;
            }
            if (!lead && !limit && f[pos][mod][diff] != -1) {
                return f[pos][mod][diff];
            }
            int ans = 0;
            int up = limit ? s[pos] - '0' : 9;
            for (int i = 0; i <= up; ++i) {
                if (i == 0 && lead) {
                    ans += dfs(pos + 1, mod, diff, true, limit && i == up);
                } else {
                    int nxt = diff + (i % 2 == 1 ? 1 : -1);
                    ans += dfs(pos + 1, (mod * 10 + i) % k, nxt, false, limit && i == up);
                }
            }
            if (!lead && !limit) {
                f[pos][mod][diff] = ans;
            }
            return ans;
        };

        int a = dfs(0, 0, 10, true, true);
        memset(f, -1, sizeof(f));
        s = to_string(low - 1);
        int b = dfs(0, 0, 10, true, true);
        return a - b;
    }
};
```

#### Go

```go
func numberOfBeautifulIntegers(low int, high int, k int) int {
	s := strconv.Itoa(high)
	f := g(len(s), k, 21)

	var dfs func(pos, mod, diff int, lead, limit bool) int
	dfs = func(pos, mod, diff int, lead, limit bool) int {
		if pos >= len(s) {
			if mod == 0 && diff == 10 {
				return 1
			}
			return 0
		}
		if !lead && !limit && f[pos][mod][diff] != -1 {
			return f[pos][mod][diff]
		}
		up := 9
		if limit {
			up = int(s[pos] - '0')
		}
		ans := 0
		for i := 0; i <= up; i++ {
			if i == 0 && lead {
				ans += dfs(pos+1, mod, diff, true, limit && i == up)
			} else {
				nxt := diff + 1
				if i%2 == 0 {
					nxt -= 2
				}
				ans += dfs(pos+1, (mod*10+i)%k, nxt, false, limit && i == up)
			}
		}
		if !lead && !limit {
			f[pos][mod][diff] = ans
		}
		return ans
	}

	a := dfs(0, 0, 10, true, true)
	s = strconv.Itoa(low - 1)
	f = g(len(s), k, 21)
	b := dfs(0, 0, 10, true, true)
	return a - b
}

func g(m, n, k int) [][][]int {
	f := make([][][]int, m)
	for i := 0; i < m; i++ {
		f[i] = make([][]int, n)
		for j := 0; j < n; j++ {
			f[i][j] = make([]int, k)
			for d := 0; d < k; d++ {
				f[i][j][d] = -1
			}
		}
	}
	return f
}
```

#### TypeScript

```ts
function numberOfBeautifulIntegers(low: number, high: number, k: number): number {
    let s = String(high);
    let f: number[][][] = Array(11)
        .fill(0)
        .map(() =>
            Array(21)
                .fill(0)
                .map(() => Array(21).fill(-1)),
        );
    const dfs = (pos: number, mod: number, diff: number, lead: boolean, limit: boolean): number => {
        if (pos >= s.length) {
            return mod == 0 && diff == 10 ? 1 : 0;
        }
        if (!lead && !limit && f[pos][mod][diff] != -1) {
            return f[pos][mod][diff];
        }
        let ans = 0;
        const up = limit ? Number(s[pos]) : 9;
        for (let i = 0; i <= up; ++i) {
            if (i === 0 && lead) {
                ans += dfs(pos + 1, mod, diff, true, limit && i === up);
            } else {
                const nxt = diff + (i % 2 ? 1 : -1);
                ans += dfs(pos + 1, (mod * 10 + i) % k, nxt, false, limit && i === up);
            }
        }
        if (!lead && !limit) {
            f[pos][mod][diff] = ans;
        }
        return ans;
    };
    const a = dfs(0, 0, 10, true, true);
    s = String(low - 1);
    f = Array(11)
        .fill(0)
        .map(() =>
            Array(21)
                .fill(0)
                .map(() => Array(21).fill(-1)),
        );
    const b = dfs(0, 0, 10, true, true);
    return a - b;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
