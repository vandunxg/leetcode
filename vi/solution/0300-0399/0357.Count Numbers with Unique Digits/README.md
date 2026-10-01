---
comments: true
difficulty: Medium
tags:
    - Math
    - Dynamic Programming
    - Backtracking
---

<!-- problem:start -->

# [357. Count Numbers with Unique Digits](https://leetcode.com/problems/count-numbers-with-unique-digits)

[中文文档](/solution/0300-0399/0357.Count%20Numbers%20with%20Unique%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy trả về số lượng số có các chữ số không trùng nhau, <code>x</code>, thỏa mãn <code>0 &lt;= x &lt; 10<sup>n</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 91
<strong>Giải thích:</strong> Kết quả là tổng số các giá trị trong khoảng 0 &le; x &lt; 100, loại trừ 11,22,33,44,55,66,77,88,99
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 0
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 8</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: State Compression + Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các số có nhiều nhất $n$ chữ số và mọi chữ số đều khác nhau (kể cả $0$). Brute force vẫn chạy được với $n=8$, nhưng digit DP xử lý số 0 ở đầu và các chữ số đã dùng một cách thống nhất.
>
> $dfs(i,mask,lead)$ biểu diễn $i+1$ vị trí còn lại, mask các chữ số đã dùng và trạng thái hiện tại còn là các số 0 ở đầu hay không. Số 0 ở đầu không chiếm chữ số; nếu không, đưa $j$ vào mask. Bắt đầu từ $i=n-1$; không gian trạng thái cần memoize khá nhỏ.

<!-- thinking:end -->

Bài toán này về cơ bản yêu cầu đếm các số trong khoảng $[l, ..r]$ thỏa mãn điều kiện cho trước. Điều kiện phụ thuộc vào các chữ số tạo nên số đó chứ không phải độ lớn của số, nên ta có thể dùng Digit DP. Với Digit DP, độ lớn của số ít ảnh hưởng đến độ phức tạp.

Với bài toán trên khoảng $[l, ..r]$, ta thường chuyển thành bài toán trên $[1, ..r]$ rồi trừ đi kết quả trên $[1, ..l - 1]$, tức là:

$$
ans = \sum_{i=1}^{r} ans_i -  \sum_{i=1}^{l-1} ans_i
$$

Tuy nhiên, với bài toán này, ta chỉ cần tìm kết quả cho khoảng $[1, ..10^n-1]$.

Ở đây, ta dùng đệ quy có memoization để triển khai Digit DP. Ta tìm kiếm từ vị trí bắt đầu đi xuống; tại mức thấp nhất, ta nhận được số lượng lời giải. Sau đó, kết quả được trả ngược lên từng mức, cho đến khi thu được đáp án cuối cùng ở điểm bắt đầu.

Dựa trên đề bài, ta thiết kế hàm $\textit{dfs}(i, \textit{mask}, \textit{lead})$, trong đó:

- Chữ số $i$ biểu thị vị trí hiện tại đang xét, bắt đầu từ chữ số cao nhất; tức là $i = 0$ biểu thị chữ số cao nhất.
- $\textit{mask}$ biểu thị trạng thái hiện tại của số: bit thứ $j$ của $\textit{mask}$ bằng $1$ nghĩa là chữ số $j$ đã được dùng.
- Giá trị boolean $\textit{lead}$ cho biết số hiện tại có chỉ gồm các số $0$ ở đầu hay không.

Hàm hoạt động như sau:

Nếu $i$ đã vượt quá độ dài $n$ của số, tức là $i < 0$, quá trình tìm kiếm đã kết thúc; trả về $1$.

Nếu không, ta xét lần lượt các chữ số $j$ từ $0$ đến $9$ ở vị trí $i$. Với mỗi $j$:

- Nếu bit thứ $j$ của $\textit{mask}$ bằng $1$, chữ số $j$ đã được dùng nên ta bỏ qua chữ số đó.
- Nếu $\textit{lead}$ là true và $j = 0$, số hiện tại vẫn chỉ có các số $0$ ở đầu. Khi đệ quy xuống mức tiếp theo, $\textit{lead}$ tiếp tục là true.
- Nếu không, ta đệ quy xuống mức tiếp theo, đặt bit thứ $j$ của $\textit{mask}$ thành $1$ và đặt $\textit{lead}$ thành false.

Cuối cùng, ta cộng kết quả của tất cả lời gọi đệ quy ở mức tiếp theo để thu được đáp án.

Đáp án là $\textit{dfs}(n - 1, 0, \textit{True})$.

Độ phức tạp thời gian là $O(n \times 2^D \times D)$ và độ phức tạp không gian là $O(n \times 2^D)$. Ở đây, $n$ là số chữ số của số $n$, còn $D = 10$.

Các bài toán tương tự:

- [233. Number of Digit One](https://github.com/doocs/leetcode/blob/main/solution/0200-0299/0233.Number%20of%20Digit%20One/README_EN.md)
- [600. Non-negative Integers without Consecutive Ones](https://github.com/doocs/leetcode/blob/main/solution/0600-0699/0600.Non-negative%20Integers%20without%20Consecutive%20Ones/README_EN.md)
- [788. Rotated Digits](https://github.com/doocs/leetcode/blob/main/solution/0700-0799/0788.Rotated%20Digits/README_EN.md)
- [902. Numbers At Most N Given Digit Set](https://github.com/doocs/leetcode/blob/main/solution/0900-0999/0902.Numbers%20At%20Most%20N%20Given%20Digit%20Set/README_EN.md)
- [1012. Numbers with Repeated Digits](https://github.com/doocs/leetcode/blob/main/solution/1000-1099/1012.Numbers%20With%20Repeated%20Digits/README_EN.md)
- [2376. Count Special Integers](https://github.com/doocs/leetcode/blob/main/solution/2300-2399/2376.Count%20Special%20Integers/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countNumbersWithUniqueDigits(self, n: int) -> int:
        @cache
        def dfs(i: int, mask: int, lead: bool) -> int:
            if i < 0:
                return 1
            ans = 0
            for j in range(10):
                if mask >> j & 1:
                    continue
                if lead and j == 0:
                    ans += dfs(i - 1, mask, True)
                else:
                    ans += dfs(i - 1, mask | 1 << j, False)
            return ans

        return dfs(n - 1, 0, True)
```

#### Java

```java
class Solution {
    private Integer[][] f;

    public int countNumbersWithUniqueDigits(int n) {
        f = new Integer[n][1 << 10];
        return dfs(n - 1, 0, true);
    }

    private int dfs(int i, int mask, boolean lead) {
        if (i < 0) {
            return 1;
        }
        if (!lead && f[i][mask] != null) {
            return f[i][mask];
        }
        int ans = 0;
        for (int j = 0; j <= 9; ++j) {
            if ((mask >> j & 1) == 1) {
                continue;
            }
            if (lead && j == 0) {
                ans += dfs(i - 1, mask, true);
            } else {
                ans += dfs(i - 1, mask | 1 << j, false);
            }
        }
        if (!lead) {
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
    int countNumbersWithUniqueDigits(int n) {
        int f[n + 1][1 << 10];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i, int mask, bool lead) -> int {
            if (i < 0) {
                return 1;
            }
            if (!lead && f[i][mask] != -1) {
                return f[i][mask];
            }
            int ans = 0;
            for (int j = 0; j <= 9; ++j) {
                if (mask >> j & 1) {
                    continue;
                }
                if (lead && j == 0) {
                    ans += dfs(i - 1, mask, true);
                } else {
                    ans += dfs(i - 1, mask | 1 << i, false);
                }
            }
            if (!lead) {
                f[i][mask] = ans;
            }
            return ans;
        };
        return dfs(n - 1, 0, true);
    }
};
```

#### Go

```go
func countNumbersWithUniqueDigits(n int) int {
	f := make([][1 << 10]int, n)
	for i := range f {
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(i, mask int, lead bool) int
	dfs = func(i, mask int, lead bool) int {
		if i < 0 {
			return 1
		}
		if !lead && f[i][mask] != -1 {
			return f[i][mask]
		}
		ans := 0
		for j := 0; j < 10; j++ {
			if mask>>j&1 == 1 {
				continue
			}
			if lead && j == 0 {
				ans += dfs(i-1, mask, true)
			} else {
				ans += dfs(i-1, mask|1<<j, false)
			}
		}
		if !lead {
			f[i][mask] = ans
		}
		return ans
	}
	return dfs(n-1, 0, true)
}
```

#### TypeScript

```ts
function countNumbersWithUniqueDigits(n: number): number {
    const f: number[][] = Array.from({ length: n }, () => Array(1 << 10).fill(-1));
    const dfs = (i: number, mask: number, lead: boolean): number => {
        if (i < 0) {
            return 1;
        }
        if (!lead && f[i][mask] !== -1) {
            return f[i][mask];
        }
        let ans = 0;
        for (let j = 0; j < 10; ++j) {
            if ((mask >> j) & 1) {
                continue;
            }
            if (lead && j === 0) {
                ans += dfs(i - 1, mask, true);
            } else {
                ans += dfs(i - 1, mask | (1 << j), false);
            }
        }
        if (!lead) {
            f[i][mask] = ans;
        }
        return ans;
    };
    return dfs(n - 1, 0, true);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
