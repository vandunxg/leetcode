---
comments: true
difficulty: Hard
rating: 2230
source: Weekly Contest 128 Q4
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [1012. Numbers With Repeated Digits](https://leetcode.com/problems/numbers-with-repeated-digits)

[中文文档](/solution/1000-1099/1012.Numbers%20With%20Repeated%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy trả về <em>số lượng số nguyên dương trong đoạn </em><code>[1, n]</code><em> có <strong>ít nhất một</strong> chữ số bị lặp</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 20
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Số dương duy nhất (&lt;= 20) có ít nhất 1 chữ số lặp là 11.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 100
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Các số dương (&lt;= 100) có ít nhất 1 chữ số lặp là 11, 22, 33, 44, 55, 66, 77, 88, 99 và 100.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1000
<strong>Đầu ra:</strong> 262
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nén trạng thái + digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt đoạn $[1,n]$ để tìm số có chữ số lặp là không khả thi khi $n\le 10^9$. Thay vào đó, đếm các số có mọi chữ số khác nhau, gọi là $f(n)$, rồi lấy $n-f(n)$ để có đáp án.
>
> Khi điền chữ số từ trái sang phải, tập chữ số đã dùng giới hạn lựa chọn tiếp theo; các số 0 ở đầu không được tính vào tập này, còn tiền tố phải bị chặn bởi $n$. Mười chữ số có thể được biểu diễn bằng một bitmask.
>
> Hàm $\textit{dfs}(i,\textit{mask},\textit{lead},\textit{limit})$ có memoization và liệt kê chữ số ở vị trí $i$: số 0 ở đầu không làm thay đổi mask; nếu không, ta thêm một chữ số chưa dùng. Khi điền xong, chỉ tính số đó nếu nó không phải toàn số 0 ở đầu.

<!-- thinking:end -->

Bài toán yêu cầu đếm số nguyên trong đoạn $[1, .., n]$ có ít nhất một chữ số lặp. Ta có thể định nghĩa hàm $f(n)$ là số lượng số nguyên trong đoạn $[1, .., n]$ không có chữ số lặp. Khi đó, đáp án là $n - f(n)$.

Ngoài ra, ta có thể dùng một số nhị phân để ghi nhận các chữ số đã xuất hiện. Ví dụ, nếu các chữ số $1$, $2$ và $4$ đã xuất hiện, số nhị phân tương ứng là $\underline{1}0\underline{1}\underline{1}0$.

Tiếp theo, ta dùng memoization để triển khai digit DP. Bắt đầu từ vị trí cao nhất, ta tính số cách ở các trạng thái cuối rồi trả kết quả ngược lên từng lớp cho đến khi nhận được đáp án tại trạng thái ban đầu.

Các bước chính như sau:

Ta chuyển số $n$ thành chuỗi $s$. Tiếp theo, xây dựng hàm $\textit{dfs}(i, \textit{mask}, \textit{lead}, \textit{limit})$, trong đó:

- Số nguyên $i$ là chỉ số chữ số hiện tại, bắt đầu từ $0$.
- Số nguyên $\textit{mask}$ biểu diễn các chữ số đã xuất hiện, dưới dạng số nhị phân. Bit thứ $j$ của $\textit{mask}$ bằng $1$ nghĩa là chữ số $j$ đã xuất hiện; bằng $0$ nghĩa là chưa xuất hiện.
- Boolean $\textit{lead}$ cho biết số hiện tại có chỉ gồm các số 0 ở đầu hay không.
- Boolean $\textit{limit}$ cho biết vị trí hiện tại có bị giới hạn bởi cận trên hay không.

Hàm hoạt động như sau:

Nếu $i$ lớn hơn hoặc bằng $m$, nghĩa là ta đã xử lý hết chữ số. Nếu $\textit{lead}$ là true, số hiện tại chỉ là các số 0 ở đầu nên ta trả về $0$; ngược lại, trả về $1$.

Nếu chưa, ta tính cận trên $\textit{up}$. Khi $\textit{limit}$ là true, $\textit{up}$ là chữ số tương ứng với $s[i]$; nếu không, $\textit{up}$ bằng $9$.

Sau đó, ta lần lượt xét chữ số hiện tại $j$ trong đoạn $[0, \textit{up}]$. Nếu $j$ bằng $0$ và $\textit{lead}$ là true, ta đệ quy tính $\textit{dfs}(i + 1, \textit{mask}, \text{true}, \textit{limit} \wedge j = \textit{up})$. Nếu không, khi bit thứ $j$ của $\textit{mask}$ bằng $0$, ta đệ quy tính $\textit{dfs}(i + 1, \textit{mask} \,|\, 2^j, \text{false}, \textit{limit} \wedge j = \textit{up})$. Ta cộng dồn các kết quả để có đáp án.

Đáp án là $n - \textit{dfs}(0, 0, \text{true}, \text{true})$.

Độ phức tạp thời gian là $O(\log n \times 2^D \times D)$, còn độ phức tạp không gian là $O(\log n \times 2^D)$. Ở đây, $D = 10$.

Bài toán tương tự:

- [233. Number of Digit One](https://github.com/doocs/leetcode/blob/main/solution/0200-0299/0233.Number%20of%20Digit%20One/README_EN.md)
- [357. Count Numbers with Unique Digits](https://github.com/doocs/leetcode/blob/main/solution/0300-0399/0357.Count%20Numbers%20with%20Unique%20Digits/README_EN.md)
- [600. Non-negative Integers without Consecutive Ones](https://github.com/doocs/leetcode/blob/main/solution/0600-0699/0600.Non-negative%20Integers%20without%20Consecutive%20Ones/README_EN.md)
- [788. Rotated Digits](https://github.com/doocs/leetcode/blob/main/solution/0700-0799/0788.Rotated%20Digits/README_EN.md)
- [902. Numbers At Most N Given Digit Set](https://github.com/doocs/leetcode/blob/main/solution/0900-0999/0902.Numbers%20At%20Most%20N%20Given%20Digit%20Set/README_EN.md)
- [2376. Count Special Integers](https://github.com/doocs/leetcode/blob/main/solution/2300-2399/2376.Count%20Special%20Integers/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numDupDigitsAtMostN(self, n: int) -> int:
        @cache
        def dfs(i: int, mask: int, lead: bool, limit: bool) -> int:
            if i >= len(s):
                return lead ^ 1
            up = int(s[i]) if limit else 9
            ans = 0
            for j in range(up + 1):
                if lead and j == 0:
                    ans += dfs(i + 1, mask, True, False)
                elif mask >> j & 1 ^ 1:
                    ans += dfs(i + 1, mask | 1 << j, False, limit and j == up)
            return ans

        s = str(n)
        return n - dfs(0, 0, True, True)
```

#### Java

```java
class Solution {
    private char[] s;
    private Integer[][] f;

    public int numDupDigitsAtMostN(int n) {
        s = String.valueOf(n).toCharArray();
        f = new Integer[s.length][1 << 10];
        return n - dfs(0, 0, true, true);
    }

    private int dfs(int i, int mask, boolean lead, boolean limit) {
        if (i >= s.length) {
            return lead ? 0 : 1;
        }
        if (!lead && !limit && f[i][mask] != null) {
            return f[i][mask];
        }
        int up = limit ? s[i] - '0' : 9;
        int ans = 0;
        for (int j = 0; j <= up; ++j) {
            if (lead && j == 0) {
                ans += dfs(i + 1, mask, true, false);
            } else if ((mask >> j & 1) == 0) {
                ans += dfs(i + 1, mask | 1 << j, false, limit && j == up);
            }
        }
        if (!lead && !limit) {
            f[i][mask] = ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numDupDigitsAtMostN(int n) {
        string s = to_string(n);
        int m = s.size();
        int f[m][1 << 10];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i, int mask, bool lead, bool limit) -> int {
            if (i >= m) {
                return lead ^ 1;
            }
            if (!lead && !limit && f[i][mask] != -1) {
                return f[i][mask];
            }
            int up = limit ? s[i] - '0' : 9;
            int ans = 0;
            for (int j = 0; j <= up; ++j) {
                if (lead && j == 0) {
                    ans += dfs(i + 1, mask, true, limit && j == up);
                } else if (mask >> j & 1 ^ 1) {
                    ans += dfs(i + 1, mask | (1 << j), false, limit && j == up);
                }
            }
            if (!lead && !limit) {
                f[i][mask] = ans;
            }
            return ans;
        };
        return n - dfs(0, 0, true, true);
    }
};
```

#### Go

```go
func numDupDigitsAtMostN(n int) int {
	s := []byte(strconv.Itoa(n))
	m := len(s)
	f := make([][]int, m)
	for i := range f {
		f[i] = make([]int, 1<<10)
		for j := range f[i] {
			f[i][j] = -1
		}
	}

	var dfs func(i, mask int, lead, limit bool) int
	dfs = func(i, mask int, lead, limit bool) int {
		if i >= m {
			if lead {
				return 0
			}
			return 1
		}
		if !lead && !limit && f[i][mask] != -1 {
			return f[i][mask]
		}
		up := 9
		if limit {
			up = int(s[i] - '0')
		}
		ans := 0
		for j := 0; j <= up; j++ {
			if lead && j == 0 {
				ans += dfs(i+1, mask, true, limit && j == up)
			} else if mask>>j&1 == 0 {
				ans += dfs(i+1, mask|(1<<j), false, limit && j == up)
			}
		}
		if !lead && !limit {
			f[i][mask] = ans
		}
		return ans
	}
	return n - dfs(0, 0, true, true)
}
```

#### TypeScript

```ts
function numDupDigitsAtMostN(n: number): number {
    const s = n.toString();
    const m = s.length;
    const f = Array.from({ length: m }, () => Array(1 << 10).fill(-1));

    const dfs = (i: number, mask: number, lead: boolean, limit: boolean): number => {
        if (i >= m) {
            return lead ? 0 : 1;
        }
        if (!lead && !limit && f[i][mask] !== -1) {
            return f[i][mask];
        }
        const up = limit ? parseInt(s[i]) : 9;
        let ans = 0;
        for (let j = 0; j <= up; j++) {
            if (lead && j === 0) {
                ans += dfs(i + 1, mask, true, limit && j === up);
            } else if (((mask >> j) & 1) === 0) {
                ans += dfs(i + 1, mask | (1 << j), false, limit && j === up);
            }
        }
        if (!lead && !limit) {
            f[i][mask] = ans;
        }
        return ans;
    };

    return n - dfs(0, 0, true, true);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
