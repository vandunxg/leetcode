---
comments: true
difficulty: Hard
rating: 2367
source: Weekly Contest 356 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2801. Count Stepping Numbers in Range](https://leetcode.com/problems/count-stepping-numbers-in-range)

[中文文档](/solution/2800-2899/2801.Count%20Stepping%20Numbers%20in%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên dương <code>low</code> và <code>high</code> được biểu diễn dưới dạng chuỗi, hãy tìm số lượng <strong>số bước</strong> trong đoạn đóng <code>[low, high]</code>.</p>

<p><strong>Số bước</strong> là một số nguyên mà mọi cặp chữ số liền kề có độ chênh lệch tuyệt đối <strong>đúng bằng</strong> <code>1</code>.</p>

<p>Trả về <em>một số nguyên biểu thị số lượng số bước trong đoạn đóng</em> <code>[low, high]</code><em>. </em></p>

<p>Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Chú ý:</strong> Số bước không được có số 0 ở đầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> low = &quot;1&quot;, high = &quot;11&quot;
<strong>Đầu ra:</strong> 10
<strong>Giải thích: </strong>Các số bước trong đoạn [1,11] là 1, 2, 3, 4, 5, 6, 7, 8, 9 và 10. Có tổng cộng 10 số bước trong đoạn. Do đó, đầu ra là 10.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> low = &quot;90&quot;, high = &quot;101&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích: </strong>Các số bước trong đoạn [90,101] là 98 và 101. Có tổng cộng 2 số bước trong đoạn. Do đó, đầu ra là 2. </pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= int(low) &lt;= int(high) &lt; 10<sup>100</sup></code></li>
	<li><code>1 &lt;= low.length, high.length &lt;= 100</code></li>
	<li><code>low</code> và <code>high</code> chỉ gồm các chữ số.</li>
	<li><code>low</code> và <code>high</code> không có số 0 ở đầu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> $low$ và $high$ được cho dưới dạng chuỗi chữ số, nên không thể duyệt mọi số trong đoạn. Số lượng trên $[low,high]$ bằng $F(high)-F(low-1)$. Điều kiện của số bước chỉ liên quan đến các chữ số kề nhau, đúng với dạng bài digit DP tiêu chuẩn. Ta memoize theo vị trí $pos$, chữ số trước đó $pre$, cờ số 0 ở đầu $lead$ và cờ giới hạn trên $limit$: các số 0 ở đầu bỏ qua kiểm tra kề nhau, còn chữ số khác 0 $i$ chỉ được chọn khi chưa có chữ số trước đó hoặc $|i-pre|=1$.

<!-- thinking:end -->

Ta nhận thấy bài toán yêu cầu đếm số lượng số bước trong đoạn $[low, high]$. Với bài toán trên đoạn $[l,..r]$, ta thường chuyển thành việc tìm đáp án cho $[1, r]$ và $[1, l-1]$, sau đó lấy đáp án thứ nhất trừ đáp án thứ hai. Ngoài ra, bài toán chỉ liên quan đến mối quan hệ giữa các chữ số khác nhau, không phụ thuộc vào giá trị cụ thể, nên ta có thể dùng Digit DP để giải quyết.

Ta thiết kế hàm $dfs(pos, pre, lead, limit)$, biểu thị số cách khi đang xử lý chữ số thứ $pos$, chữ số trước đó là $pre$, số hiện tại có chỉ gồm các số 0 ở đầu hay không là $lead$, và số hiện tại đã chạm giới hạn trên hay chưa là $limit$. Miền giá trị của $pos$ là $[0, len(num))$.

Logic thực thi của hàm $dfs(pos, pre, lead, limit)$ như sau:

Nếu $pos$ vượt quá độ dài của $num$, nghĩa là ta đã xử lý xong tất cả chữ số. Nếu lúc này $lead$ là true, điều đó có nghĩa số hiện tại chỉ gồm các số 0 ở đầu và không phải là một số hợp lệ. Ta trả về $0$ để biểu thị có $0$ cách; ngược lại, ta trả về $1$ để biểu thị có $1$ cách.

Nếu không, ta tính giới hạn trên $up$ của chữ số hiện tại, rồi duyệt chữ số $i$ trong đoạn $[0,..up]$:

- Nếu $i=0$ và $lead$ là true, điều đó có nghĩa số hiện tại chỉ gồm các số 0 ở đầu. Ta đệ quy tính giá trị của $dfs(pos+1,pre, true, limit\ and\ i=up)$ và cộng vào đáp án.
- Ngược lại, nếu $pre$ là $-1$, hoặc độ chênh lệch tuyệt đối giữa $i$ và $pre$ là $1$, thì số hiện tại là một số bước hợp lệ. Ta đệ quy tính giá trị của $dfs(pos+1,i, false, limit\ and\ i=up)$ và cộng vào đáp án.

Cuối cùng, ta trả về đáp án.

Trong hàm chính, ta tính các đáp án $a$ và $b$ cho $[1, high]$ và $[1, low-1]$ tương ứng. Đáp án cuối cùng là $a-b$. Lưu ý thực hiện phép modulo cho đáp án.

Độ phức tạp thời gian là $O(\log M \times |\Sigma|^2)$, độ phức tạp không gian là $O(\log M \times |\Sigma|)$, trong đó $M$ biểu thị kích thước của số $high$, còn $|\Sigma|$ biểu thị tập chữ số.

Bài toán tương tự:

- [2719. Count of Integers](https://github.com/doocs/leetcode/blob/main/solution/2700-2799/2719.Count%20of%20Integers/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSteppingNumbers(self, low: str, high: str) -> int:
        @cache
        def dfs(pos: int, pre: int, lead: bool, limit: bool) -> int:
            if pos >= len(num):
                return int(not lead)
            up = int(num[pos]) if limit else 9
            ans = 0
            for i in range(up + 1):
                if i == 0 and lead:
                    ans += dfs(pos + 1, pre, True, limit and i == up)
                elif pre == -1 or abs(i - pre) == 1:
                    ans += dfs(pos + 1, i, False, limit and i == up)
            return ans % mod

        mod = 10**9 + 7
        num = high
        a = dfs(0, -1, True, True)
        dfs.cache_clear()
        num = str(int(low) - 1)
        b = dfs(0, -1, True, True)
        return (a - b) % mod
```

#### Java

```java
import java.math.BigInteger;

class Solution {
    private final int mod = (int) 1e9 + 7;
    private String num;
    private Integer[][] f;

    public int countSteppingNumbers(String low, String high) {
        f = new Integer[high.length() + 1][10];
        num = high;
        int a = dfs(0, -1, true, true);
        f = new Integer[high.length() + 1][10];
        num = new BigInteger(low).subtract(BigInteger.ONE).toString();
        int b = dfs(0, -1, true, true);
        return (a - b + mod) % mod;
    }

    private int dfs(int pos, int pre, boolean lead, boolean limit) {
        if (pos >= num.length()) {
            return lead ? 0 : 1;
        }
        if (!lead && !limit && f[pos][pre] != null) {
            return f[pos][pre];
        }
        int ans = 0;
        int up = limit ? num.charAt(pos) - '0' : 9;
        for (int i = 0; i <= up; ++i) {
            if (i == 0 && lead) {
                ans += dfs(pos + 1, pre, true, limit && i == up);
            } else if (pre == -1 || Math.abs(pre - i) == 1) {
                ans += dfs(pos + 1, i, false, limit && i == up);
            }
            ans %= mod;
        }
        if (!lead && !limit) {
            f[pos][pre] = ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countSteppingNumbers(string low, string high) {
        const int mod = 1e9 + 7;
        int m = high.size();
        int f[m + 1][10];
        memset(f, -1, sizeof(f));
        string num = high;

        function<int(int, int, bool, bool)> dfs = [&](int pos, int pre, bool lead, bool limit) {
            if (pos >= num.size()) {
                return lead ? 0 : 1;
            }
            if (!lead && !limit && f[pos][pre] != -1) {
                return f[pos][pre];
            }
            int up = limit ? num[pos] - '0' : 9;
            int ans = 0;
            for (int i = 0; i <= up; ++i) {
                if (i == 0 && lead) {
                    ans += dfs(pos + 1, pre, true, limit && i == up);
                } else if (pre == -1 || abs(pre - i) == 1) {
                    ans += dfs(pos + 1, i, false, limit && i == up);
                }
                ans %= mod;
            }
            if (!lead && !limit) {
                f[pos][pre] = ans;
            }
            return ans;
        };

        int a = dfs(0, -1, true, true);
        memset(f, -1, sizeof(f));
        for (int i = low.size() - 1; i >= 0; --i) {
            if (low[i] == '0') {
                low[i] = '9';
            } else {
                low[i] -= 1;
                break;
            }
        }
        num = low;
        int b = dfs(0, -1, true, true);
        return (a - b + mod) % mod;
    }
};
```

#### Go

```go
func countSteppingNumbers(low string, high string) int {
	const mod = 1e9 + 7
	f := [110][10]int{}
	for i := range f {
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	num := high
	var dfs func(int, int, bool, bool) int
	dfs = func(pos, pre int, lead bool, limit bool) int {
		if pos >= len(num) {
			if lead {
				return 0
			}
			return 1
		}
		if !lead && !limit && f[pos][pre] != -1 {
			return f[pos][pre]
		}
		var ans int
		up := 9
		if limit {
			up = int(num[pos] - '0')
		}
		for i := 0; i <= up; i++ {
			if i == 0 && lead {
				ans += dfs(pos+1, pre, true, limit && i == up)
			} else if pre == -1 || abs(pre-i) == 1 {
				ans += dfs(pos+1, i, false, limit && i == up)
			}
			ans %= mod
		}
		if !lead && !limit {
			f[pos][pre] = ans
		}
		return ans
	}
	a := dfs(0, -1, true, true)
	t := []byte(low)
	for i := len(t) - 1; i >= 0; i-- {
		if t[i] != '0' {
			t[i]--
			break
		}
		t[i] = '9'
	}
	num = string(t)
	f = [110][10]int{}
	for i := range f {
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	b := dfs(0, -1, true, true)
	return (a - b + mod) % mod
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
function countSteppingNumbers(low: string, high: string): number {
    const mod = 1e9 + 7;
    const m = high.length;
    let f: number[][] = Array(m + 1)
        .fill(0)
        .map(() => Array(10).fill(-1));
    let num = high;
    const dfs = (pos: number, pre: number, lead: boolean, limit: boolean): number => {
        if (pos >= num.length) {
            return lead ? 0 : 1;
        }
        if (!lead && !limit && f[pos][pre] !== -1) {
            return f[pos][pre];
        }
        let ans = 0;
        const up = limit ? +num[pos] : 9;
        for (let i = 0; i <= up; i++) {
            if (i == 0 && lead) {
                ans += dfs(pos + 1, pre, true, limit && i == up);
            } else if (pre == -1 || Math.abs(pre - i) == 1) {
                ans += dfs(pos + 1, i, false, limit && i == up);
            }
            ans %= mod;
        }
        if (!lead && !limit) {
            f[pos][pre] = ans;
        }
        return ans;
    };
    const a = dfs(0, -1, true, true);
    num = (BigInt(low) - 1n).toString();
    f = Array(m + 1)
        .fill(0)
        .map(() => Array(10).fill(-1));
    const b = dfs(0, -1, true, true);
    return (a - b + mod) % mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
