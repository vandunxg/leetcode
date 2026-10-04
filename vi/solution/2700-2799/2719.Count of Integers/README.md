---
comments: true
difficulty: Hard
rating: 2354
source: Weekly Contest 348 Q4
tags:
    - Math
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2719. Count of Integers](https://leetcode.com/problems/count-of-integers)

[中文文档](/solution/2700-2799/2719.Count%20of%20Integers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi số <code>num1</code> và <code>num2</code>, cùng hai số nguyên <code>max_sum</code> và <code>min_sum</code>. Một số nguyên <code>x</code> được gọi là <em>tốt</em> nếu:</p>

<ul>
	<li><code>num1 &lt;= x &lt;= num2</code></li>
	<li><code>min_sum &lt;= digit_sum(x) &lt;= max_sum</code>.</li>
</ul>

<p>Trả về <em>số lượng số nguyên tốt</em>. Vì đáp án có thể rất lớn, hãy trả về kết quả theo modulo <code>10<sup>9</sup> + 7</code>.</p>

<p>Lưu ý rằng <code>digit_sum(x)</code> là tổng các chữ số của <code>x</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num1 = &quot;1&quot;, num2 = &quot;12&quot;, <code>min_sum</code> = 1, max_sum = 8
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Có 11 số nguyên có tổng chữ số nằm trong khoảng từ 1 đến 8 là 1,2,3,4,5,6,7,8,10,11 và 12. Do đó, ta trả về 11.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num1 = &quot;1&quot;, num2 = &quot;5&quot;, <code>min_sum</code> = 1, max_sum = 5
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> 5 số nguyên có tổng chữ số nằm trong khoảng từ 1 đến 5 là 1,2,3,4 và 5. Do đó, ta trả về 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num1 &lt;= num2 &lt;= 10<sup>22</sup></code></li>
	<li><code>1 &lt;= min_sum &lt;= max_sum &lt;= 400</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các số nguyên trong $[num1,num2]$ có tổng chữ số thuộc $[min\_sum,max\_sum]$. Khoảng này có thể dài tới $10^{22}$, nên không thể duyệt từng số.
>
> Tính đáp án cho $[0,num2]$ rồi trừ đi đáp án cho $[0,num1-1]$. Digit DP duyệt từ chữ số cao xuống với trạng thái $(pos,s,limit)$ — vị trí, tổng chữ số và việc ta có đang bám theo cận trên hay không. Khi kết thúc, ta kiểm tra tổng chữ số có thuộc khoảng cần tìm hay không và lấy modulo $10^9+7$.

<!-- thinking:end -->

Bài toán thực chất yêu cầu đếm số lượng số nguyên trong khoảng $[num1,..num2]$ có tổng chữ số nằm trong khoảng $[min\_sum,..max\_sum]$. Với dạng bài về khoảng $[l,..r]$ này, ta có thể chuyển thành việc tìm đáp án cho $[1,..r]$ và $[1,..l-1]$, sau đó lấy đáp án đầu trừ đáp án sau.

Để tìm đáp án cho $[1,..r]$, ta có thể dùng digit DP. Ta thiết kế hàm $dfs(pos, s, limit)$, biểu diễn số cách khi đang xử lý chữ số thứ $pos$, tổng chữ số hiện tại là $s$ và số hiện tại có bị giới hạn trên bởi $limit$ hay không. Ở đây, $pos$ được duyệt từ cao xuống thấp.

Với $dfs(pos, s, limit)$, ta có thể duyệt giá trị của chữ số hiện tại $i$, sau đó đệ quy tính $dfs(pos+1, s+i, limit \bigcap i==up)$, trong đó $up$ là cận trên của chữ số hiện tại. Nếu $limit$ là true, thì $up$ là cận trên của chữ số hiện tại; nếu không, $up$ là $9$. Nếu $pos$ lớn hơn hoặc bằng độ dài của $num$, ta kiểm tra xem $s$ có nằm trong khoảng $[min\_sum,..max\_sum]$ hay không. Nếu có, trả về $1$, ngược lại trả về $0$.

Độ phức tạp thời gian là $O(10 \times n \times max\_sum)$, độ phức tạp không gian là $O(n \times max\_sum)$. Trong đó, $n$ là độ dài của $num$.

Bài tương tự:

- [2801. Count Stepping Numbers in Range](https://github.com/doocs/leetcode/blob/main/solution/2800-2899/2801.Count%20Stepping%20Numbers%20in%20Range/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def count(self, num1: str, num2: str, min_sum: int, max_sum: int) -> int:
        @cache
        def dfs(pos: int, s: int, limit: bool) -> int:
            if pos >= len(num):
                return int(min_sum <= s <= max_sum)
            up = int(num[pos]) if limit else 9
            return (
                sum(dfs(pos + 1, s + i, limit and i == up) for i in range(up + 1)) % mod
            )

        mod = 10**9 + 7
        num = num2
        a = dfs(0, 0, True)
        dfs.cache_clear()
        num = str(int(num1) - 1)
        b = dfs(0, 0, True)
        return (a - b) % mod
```

#### Java

```java
import java.math.BigInteger;

class Solution {
    private final int mod = (int) 1e9 + 7;
    private Integer[][] f;
    private String num;
    private int min;
    private int max;

    public int count(String num1, String num2, int min_sum, int max_sum) {
        min = min_sum;
        max = max_sum;
        num = num2;
        f = new Integer[23][220];
        int a = dfs(0, 0, true);
        num = new BigInteger(num1).subtract(BigInteger.ONE).toString();
        f = new Integer[23][220];
        int b = dfs(0, 0, true);
        return (a - b + mod) % mod;
    }

    private int dfs(int pos, int s, boolean limit) {
        if (pos >= num.length()) {
            return s >= min && s <= max ? 1 : 0;
        }
        if (!limit && f[pos][s] != null) {
            return f[pos][s];
        }
        int ans = 0;
        int up = limit ? num.charAt(pos) - '0' : 9;
        for (int i = 0; i <= up; ++i) {
            ans = (ans + dfs(pos + 1, s + i, limit && i == up)) % mod;
        }
        if (!limit) {
            f[pos][s] = ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int count(string num1, string num2, int min_sum, int max_sum) {
        const int mod = 1e9 + 7;
        int f[23][220];
        memset(f, -1, sizeof(f));
        string num = num2;

        function<int(int, int, bool)> dfs = [&](int pos, int s, bool limit) -> int {
            if (pos >= num.size()) {
                return s >= min_sum && s <= max_sum ? 1 : 0;
            }
            if (!limit && f[pos][s] != -1) {
                return f[pos][s];
            }
            int up = limit ? num[pos] - '0' : 9;
            int ans = 0;
            for (int i = 0; i <= up; ++i) {
                ans += dfs(pos + 1, s + i, limit && i == up);
                ans %= mod;
            }
            if (!limit) {
                f[pos][s] = ans;
            }
            return ans;
        };

        int a = dfs(0, 0, true);
        for (int i = num1.size() - 1; ~i; --i) {
            if (num1[i] == '0') {
                num1[i] = '9';
            } else {
                num1[i] -= 1;
                break;
            }
        }
        num = num1;
        memset(f, -1, sizeof(f));
        int b = dfs(0, 0, true);
        return (a - b + mod) % mod;
    }
};
```

#### Go

```go
func count(num1 string, num2 string, min_sum int, max_sum int) int {
	const mod = 1e9 + 7
	f := [23][220]int{}
	for i := range f {
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	num := num2
	var dfs func(int, int, bool) int
	dfs = func(pos, s int, limit bool) int {
		if pos >= len(num) {
			if s >= min_sum && s <= max_sum {
				return 1
			}
			return 0
		}
		if !limit && f[pos][s] != -1 {
			return f[pos][s]
		}
		var ans int
		up := 9
		if limit {
			up = int(num[pos] - '0')
		}
		for i := 0; i <= up; i++ {
			ans = (ans + dfs(pos+1, s+i, limit && i == up)) % mod
		}
		if !limit {
			f[pos][s] = ans
		}
		return ans
	}
	a := dfs(0, 0, true)
	t := []byte(num1)
	for i := len(t) - 1; i >= 0; i-- {
		if t[i] != '0' {
			t[i]--
			break
		}
		t[i] = '9'
	}
	num = string(t)
	f = [23][220]int{}
	for i := range f {
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	b := dfs(0, 0, true)
	return (a - b + mod) % mod
}
```

#### TypeScript

```ts
function count(num1: string, num2: string, min_sum: number, max_sum: number): number {
    const mod = 1e9 + 7;
    const f: number[][] = Array.from({ length: 23 }, () => Array(220).fill(-1));
    let num = num2;
    const dfs = (pos: number, s: number, limit: boolean): number => {
        if (pos >= num.length) {
            return s >= min_sum && s <= max_sum ? 1 : 0;
        }
        if (!limit && f[pos][s] !== -1) {
            return f[pos][s];
        }
        let ans = 0;
        const up = limit ? +num[pos] : 9;
        for (let i = 0; i <= up; i++) {
            ans = (ans + dfs(pos + 1, s + i, limit && i === up)) % mod;
        }
        if (!limit) {
            f[pos][s] = ans;
        }
        return ans;
    };
    const a = dfs(0, 0, true);
    num = (BigInt(num1) - 1n).toString();
    f.forEach(v => v.fill(-1));
    const b = dfs(0, 0, true);
    return (a - b + mod) % mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
