---
comments: true
difficulty: Hard
rating: 2120
source: Weekly Contest 306 Q4
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [2376. Count Special Integers](https://leetcode.com/problems/count-special-integers)

[中文文档](/solution/2300-2399/2376.Count%20Special%20Integers/README.md)

## Mô tả

<!-- description:start -->

<p>Một số nguyên dương được gọi là <strong>đặc biệt</strong> nếu tất cả các chữ số của nó đều <strong>khác nhau</strong>.</p>

<p>Cho một số nguyên <strong>dương</strong> <code>n</code>, hãy trả về <em>số lượng số nguyên đặc biệt thuộc đoạn </em><code>[1, n]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 20
<strong>Đầu ra:</strong> 19
<strong>Giải thích:</strong> Tất cả các số nguyên từ 1 đến 20, ngoại trừ 11, đều là số đặc biệt. Do đó, có 19 số nguyên đặc biệt.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Tất cả các số nguyên từ 1 đến 5 đều là số đặc biệt.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 135
<strong>Đầu ra:</strong> 110
<strong>Giải thích:</strong> Có 110 số nguyên từ 1 đến 135 là số đặc biệt.
Một số số nguyên không đặc biệt là: 22, 114 và 131.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2 * 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: State Compression + Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các số nguyên trong $[1,n]$ có các chữ số khác nhau. Vì $n \le 2 \times 10^9$ nên không thể liệt kê. Điều kiện chỉ phụ thuộc vào tập chữ số, vì vậy có thể dùng digit DP.
>
> Memoize $dfs(i,mask,lead,limit)$: $mask$ là các chữ số đã dùng, các số 0 ở đầu không chiếm chữ số nào, còn $limit$ đảm bảo số đang xét $\le n$. Khi kết thúc một số không chỉ gồm các số 0 ở đầu, ta cộng $1$.

<!-- thinking:end -->

Bài toán này về cơ bản yêu cầu đếm số lượng số trong đoạn $[l, ..r]$ thỏa mãn một số điều kiện nhất định. Các điều kiện liên quan đến cấu tạo của số thay vì độ lớn của nó, vì vậy ta có thể dùng Digit DP để giải quyết. Trong Digit DP, độ lớn của số ít ảnh hưởng đến độ phức tạp.

Với bài toán trên đoạn $[l, ..r]$, thông thường ta chuyển thành bài toán trên đoạn $[1, ..r]$ rồi trừ đi kết quả trên đoạn $[1, ..l - 1]$, tức là:

$$
ans = \sum_{i=1}^{r} ans_i -  \sum_{i=1}^{l-1} ans_i
$$

Tuy nhiên, trong bài toán này, ta chỉ cần tìm giá trị trên đoạn $[1, ..n]$.

Ở đây, ta dùng memoized search để cài đặt Digit DP. Ta duyệt từ vị trí bắt đầu xuống dưới, tại mức thấp nhất ta thu được số lượng nghiệm. Sau đó, ta lần lượt trả kết quả ngược lên qua từng lớp và cuối cùng nhận được đáp án tại điểm bắt đầu của phép tìm kiếm.

Dựa trên thông tin của đề bài, ta xây dựng hàm $\textit{dfs}(i, \textit{mask}, \textit{lead}, \textit{limit})$, trong đó:

- Chữ số $i$ biểu diễn vị trí hiện tại đang xét, bắt đầu từ chữ số cao nhất, tức là $i = 0$ biểu diễn chữ số cao nhất.
- Chữ số $\textit{mask}$ biểu diễn trạng thái hiện tại của số, tức là bit thứ $j$ của $\textit{mask}$ bằng $1$ cho biết chữ số $j$ đã được sử dụng.
- Boolean $\textit{lead}$ cho biết số hiện tại có chỉ gồm các số $0$ ở đầu hay không.
- Boolean $\textit{limit}$ cho biết số hiện tại có bị giới hạn bởi cận trên hay không.

Hàm được thực hiện như sau:

Nếu $i$ vượt quá độ dài của số $n$, nghĩa là phép tìm kiếm đã kết thúc. Nếu $\textit{lead}$ là true, nghĩa là số hiện tại chỉ gồm các số $0$ ở đầu, nên trả về $0$. Ngược lại, trả về $1$.

Nếu $\textit{limit}$ là false, $\textit{lead}$ là false và trạng thái của $\textit{mask}$ đã được memoize, ta trả về trực tiếp kết quả đã memoize.

Tiếp theo, ta tính cận trên hiện tại $up$. Nếu $\textit{limit}$ là true, $up$ là chữ số thứ $i$ của số hiện tại. Nếu không, $up = 9$.

Sau đó, ta duyệt trên đoạn $[0, up]$. Với mỗi chữ số $j$, nếu bit thứ $j$ của $\textit{mask}$ bằng $1$, nghĩa là chữ số $j$ đã được sử dụng, nên ta bỏ qua chữ số này. Ngược lại, nếu $\textit{lead}$ là true và $j = 0$, nghĩa là số hiện tại vẫn chỉ gồm các số $0$ ở đầu, nên ta đệ quy tìm kiếm chữ số tiếp theo. Nếu không, ta đệ quy tìm kiếm chữ số tiếp theo và cập nhật trạng thái của $\textit{mask}$.

Cuối cùng, nếu $\textit{limit}$ là false và $\textit{lead}$ là false, ta memoize trạng thái hiện tại.

Trả về đáp án cuối cùng.

Độ phức tạp thời gian là $O(m \times 2^D \times D)$, độ phức tạp không gian là $O(m \times 2^D)$. Trong đó, $m$ là độ dài của số $n$, và $D = 10$.

Các bài toán tương tự:

- [233. Number of Digit One](https://github.com/doocs/leetcode/blob/main/solution/0200-0299/0233.Number%20of%20Digit%20One/README_EN.md)
- [357. Count Numbers with Unique Digits](https://github.com/doocs/leetcode/blob/main/solution/0300-0399/0357.Count%20Numbers%20with%20Unique%20Digits/README_EN.md)
- [600. Non-negative Integers without Consecutive Ones](https://github.com/doocs/leetcode/blob/main/solution/0600-0699/0600.Non-negative%20Integers%20without%20Consecutive%20Ones/README_EN.md)
- [788. Rotated Digits](https://github.com/doocs/leetcode/blob/main/solution/0700-0799/0788.Rotated%20Digits/README_EN.md)
- [902. Numbers At Most N Given Digit Set](https://github.com/doocs/leetcode/blob/main/solution/0900-0999/0902.Numbers%20At%20Most%20N%20Given%20Digit%20Set/README_EN.md)
- [1012. Numbers with Repeated Digits](https://github.com/doocs/leetcode/blob/main/solution/1000-1099/1012.Numbers%20With%20Repeated%20Digits/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSpecialNumbers(self, n: int) -> int:
        @cache
        def dfs(i: int, mask: int, lead: bool, limit: bool) -> int:
            if i >= len(s):
                return int(lead ^ 1)
            up = int(s[i]) if limit else 9
            ans = 0
            for j in range(up + 1):
                if mask >> j & 1:
                    continue
                if lead and j == 0:
                    ans += dfs(i + 1, mask, True, limit and j == up)
                else:
                    ans += dfs(i + 1, mask | 1 << j, False, limit and j == up)
            return ans

        s = str(n)
        return dfs(0, 0, True, True)
```

#### Java

```java
class Solution {
    private char[] s;
    private Integer[][] f;

    public int countSpecialNumbers(int n) {
        s = String.valueOf(n).toCharArray();
        f = new Integer[s.length][1 << 10];
        return dfs(0, 0, true, true);
    }

    private int dfs(int i, int mask, boolean lead, boolean limit) {
        if (i >= s.length) {
            return lead ? 0 : 1;
        }
        if (!limit && !lead && f[i][mask] != null) {
            return f[i][mask];
        }
        int up = limit ? s[i] - '0' : 9;
        int ans = 0;
        for (int j = 0; j <= up; ++j) {
            if ((mask >> j & 1) == 1) {
                continue;
            }
            if (lead && j == 0) {
                ans += dfs(i + 1, mask, true, limit && j == up);
            } else {
                ans += dfs(i + 1, mask | (1 << j), false, limit && j == up);
            }
        }
        if (!limit && !lead) {
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
    int countSpecialNumbers(int n) {
        string s = to_string(n);
        int m = s.size();
        int f[m][1 << 10];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i, int mask, bool lead, bool limit) -> int {
            if (i >= m) {
                return lead ^ 1;
            }
            if (!limit && !lead && f[i][mask] != -1) {
                return f[i][mask];
            }
            int up = limit ? s[i] - '0' : 9;
            int ans = 0;
            for (int j = 0; j <= up; ++j) {
                if (mask >> j & 1) {
                    continue;
                }
                if (lead && j == 0) {
                    ans += dfs(i + 1, mask, true, limit && j == up);
                } else {
                    ans += dfs(i + 1, mask | (1 << j), false, limit && j == up);
                }
            }
            if (!limit && !lead) {
                f[i][mask] = ans;
            }
            return ans;
        };
        return dfs(0, 0, true, true);
    }
};
```

#### Go

```go
func countSpecialNumbers(n int) int {
	s := strconv.Itoa(n)
	m := len(s)
	f := make([][1 << 10]int, m+1)
	for i := range f {
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(int, int, bool, bool) int
	dfs = func(i, mask int, lead, limit bool) int {
		if i >= m {
			if lead {
				return 0
			}
			return 1
		}
		if !limit && !lead && f[i][mask] != -1 {
			return f[i][mask]
		}
		up := 9
		if limit {
			up = int(s[i] - '0')
		}
		ans := 0
		for j := 0; j <= up; j++ {
			if mask>>j&1 == 1 {
				continue
			}
			if lead && j == 0 {
				ans += dfs(i+1, mask, true, limit && j == up)
			} else {
				ans += dfs(i+1, mask|1<<j, false, limit && j == up)
			}
		}
		if !limit && !lead {
			f[i][mask] = ans
		}
		return ans
	}
	return dfs(0, 0, true, true)
}
```

#### TypeScript

```ts
function countSpecialNumbers(n: number): number {
    const s = n.toString();
    const m = s.length;
    const f: number[][] = Array.from({ length: m }, () => Array(1 << 10).fill(-1));
    const dfs = (i: number, mask: number, lead: boolean, limit: boolean): number => {
        if (i >= m) {
            return lead ? 0 : 1;
        }
        if (!limit && !lead && f[i][mask] !== -1) {
            return f[i][mask];
        }
        const up = limit ? +s[i] : 9;
        let ans = 0;
        for (let j = 0; j <= up; ++j) {
            if ((mask >> j) & 1) {
                continue;
            }
            if (lead && j === 0) {
                ans += dfs(i + 1, mask, true, limit && j === up);
            } else {
                ans += dfs(i + 1, mask | (1 << j), false, limit && j === up);
            }
        }
        if (!limit && !lead) {
            f[i][mask] = ans;
        }
        return ans;
    };
    return dfs(0, 0, true, true);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
