---
comments: true
difficulty: Hard
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [600. Non-negative Integers without Consecutive Ones](https://leetcode.com/problems/non-negative-integers-without-consecutive-ones)

[中文文档](/solution/0600-0699/0600.Non-negative%20Integers%20without%20Consecutive%20Ones/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên dương <code>n</code>, hãy trả về số lượng số nguyên trong khoảng <code>[0, n]</code> có biểu diễn nhị phân <strong>không chứa</strong> hai bit 1 liên tiếp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Các số nguyên không âm nhỏ hơn hoặc bằng 5 cùng biểu diễn nhị phân tương ứng là:
0 : 0
1 : 1
2 : 10
3 : 11
4 : 100
5 : 101
Trong đó, chỉ số 3 vi phạm quy tắc vì có hai bit 1 liên tiếp; 5 số còn lại đều thỏa mãn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Liệt kê mọi số nguyên trong $[0, n]$ rồi kiểm tra từng bit sẽ quá chậm khi $n$ lên đến $10^9$. Số lượng giá trị hợp lệ chỉ phụ thuộc vào các bit còn lại và bit trước đó có bằng $1$ hay không.
>
> Điền bit từ cao xuống thấp với trạng thái $(i, \textit{pre}, \textit{limit})$: nếu $\textit{pre}=1$ thì bit hiện tại không thể là $1$, còn $\textit{limit}$ đảm bảo prefix không vượt quá $n$. Memoization giúp thời gian chạy tuyến tính theo số bit.

<!-- thinking:end -->

Bài toán về cơ bản yêu cầu đếm các số trong khoảng $[l, ..r]$ có biểu diễn nhị phân không chứa hai bit $1$ liên tiếp. Số lượng phụ thuộc vào số chữ số và giá trị của từng chữ số nhị phân. Ta có thể giải bằng Digit DP; với cách này, độ lớn của số ít ảnh hưởng đến độ phức tạp.

Với bài toán đếm trên khoảng $[l, ..r]$, ta thường chuyển thành bài toán đếm trên $[0, ..r]$ rồi trừ đi kết quả trên $[0, ..l - 1]$, tức là:

$$
ans = \sum_{i=0}^{r} ans_i -  \sum_{i=0}^{l-1} ans_i
$$

Tuy nhiên, bài này chỉ cần tính kết quả cho khoảng $[0, ..r]$.

Ở đây, ta dùng tìm kiếm có memoization để triển khai Digit DP. Các bước chính như sau:

Đầu tiên, ta lấy độ dài biểu diễn nhị phân của $n$, ký hiệu là $m$. Sau đó, dựa vào đề bài, ta định nghĩa hàm $\textit{dfs}(i, \textit{pre}, \textit{limit})$, trong đó:

- $i$ là vị trí hiện tại đang xét, bắt đầu từ bit cao nhất (ký tự đầu tiên của chuỗi nhị phân).
- $\textit{pre}$ là bit ở vị trí nhị phân trước đó. Với bài này, giá trị ban đầu của $\textit{pre}$ là $0$.
- Giá trị boolean $\textit{limit}$ cho biết các bit có bị giới hạn khi điền hay không. Nếu không bị giới hạn, ta có thể chọn $[0,1]$. Nếu bị giới hạn, ta chỉ có thể chọn $[0, \textit{up}]$.

Hàm hoạt động như sau:

Nếu $i$ đã vượt khỏi độ dài của $n$, tức $i < 0$, quá trình tìm kiếm đã kết thúc và ta trả về $1$. Ngược lại, ta lần lượt thử các bit $j$ từ $0$ đến $\textit{up}$ ở vị trí $i$. Với mỗi $j$:

- Nếu cả $\textit{pre}$ và $j$ đều bằng $1$, tức có hai bit $1$ liên tiếp, ta bỏ qua lựa chọn này.
- Nếu không, ta gọi đệ quy cho vị trí tiếp theo, cập nhật $\textit{pre}$ thành $j$, đồng thời cập nhật $\textit{limit}$ bằng phép AND logic giữa $\textit{limit}$ và điều kiện $j$ bằng $\textit{up}$.

Cuối cùng, ta cộng kết quả của mọi lời gọi đệ quy ở bước tiếp theo để có đáp án.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là số nguyên dương đã cho.

Các bài toán tương tự:

- [233. Number of Digit One](https://github.com/doocs/leetcode/blob/main/solution/0200-0299/0233.Number%20of%20Digit%20One/README_EN.md)
- [357. Count Numbers with Unique Digits](https://github.com/doocs/leetcode/blob/main/solution/0300-0399/0357.Count%20Numbers%20with%20Unique%20Digits/README_EN.md)
- [788. Rotated Digits](https://github.com/doocs/leetcode/blob/main/solution/0700-0799/0788.Rotated%20Digits/README_EN.md)
- [902. Numbers At Most N Given Digit Set](https://github.com/doocs/leetcode/blob/main/solution/0900-0999/0902.Numbers%20At%20Most%20N%20Given%20Digit%20Set/README_EN.md)
- [1012. Numbers With Repeated Digits](https://github.com/doocs/leetcode/blob/main/solution/1000-1099/1012.Numbers%20With%20Repeated%20Digits/README_EN.md)
- [2376. Count Special Integers](https://github.com/doocs/leetcode/blob/main/solution/2300-2399/2376.Count%20Special%20Integers/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findIntegers(self, n: int) -> int:
        @cache
        def dfs(i: int, pre: int, limit: bool) -> int:
            if i < 0:
                return 1
            up = (n >> i & 1) if limit else 1
            ans = 0
            for j in range(up + 1):
                if pre and j:
                    continue
                ans += dfs(i - 1, j, limit and j == up)
            return ans

        return dfs(n.bit_length() - 1, 0, True)
```

#### Java

```java
class Solution {
    private int n;
    private Integer[][] f;

    public int findIntegers(int n) {
        this.n = n;
        int m = Integer.SIZE - Integer.numberOfLeadingZeros(n);
        f = new Integer[m][2];
        return dfs(m - 1, 0, true);
    }

    private int dfs(int i, int pre, boolean limit) {
        if (i < 0) {
            return 1;
        }
        if (!limit && f[i][pre] != null) {
            return f[i][pre];
        }
        int up = limit ? (n >> i & 1) : 1;
        int ans = 0;
        for (int j = 0; j <= up; ++j) {
            if (j == 1 && pre == 1) {
                continue;
            }
            ans += dfs(i - 1, j, limit && j == up);
        }
        if (!limit) {
            f[i][pre] = ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findIntegers(int n) {
        int m = 32 - __builtin_clz(n);
        int f[m][2];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i, int pre, bool limit) -> int {
            if (i < 0) {
                return 1;
            }
            if (!limit && f[i][pre] != -1) {
                return f[i][pre];
            }
            int up = limit ? (n >> i & 1) : 1;
            int ans = 0;
            for (int j = 0; j <= up; ++j) {
                if (j && pre) {
                    continue;
                }
                ans += dfs(i - 1, j, limit && j == up);
            }
            if (!limit) {
                f[i][pre] = ans;
            }
            return ans;
        };
        return dfs(m - 1, 0, true);
    }
};
```

#### Go

```go
func findIntegers(n int) int {
	m := bits.Len(uint(n))
	f := make([][2]int, m)
	for i := range f {
		f[i] = [2]int{-1, -1}
	}
	var dfs func(i, pre int, limit bool) int
	dfs = func(i, pre int, limit bool) int {
		if i < 0 {
			return 1
		}
		if !limit && f[i][pre] != -1 {
			return f[i][pre]
		}
		up := 1
		if limit {
			up = n >> i & 1
		}
		ans := 0
		for j := 0; j <= up; j++ {
			if j == 1 && pre == 1 {
				continue
			}
			ans += dfs(i-1, j, limit && j == up)
		}
		if !limit {
			f[i][pre] = ans
		}
		return ans
	}
	return dfs(m-1, 0, true)
}
```

#### TypeScript

```ts
function findIntegers(n: number): number {
    const m = n.toString(2).length;
    const f: number[][] = Array.from({ length: m }, () => Array(2).fill(-1));
    const dfs = (i: number, pre: number, limit: boolean): number => {
        if (i < 0) {
            return 1;
        }
        if (!limit && f[i][pre] !== -1) {
            return f[i][pre];
        }
        const up = limit ? (n >> i) & 1 : 1;
        let ans = 0;
        for (let j = 0; j <= up; ++j) {
            if (pre === 1 && j === 1) {
                continue;
            }
            ans += dfs(i - 1, j, limit && j === up);
        }
        if (!limit) {
            f[i][pre] = ans;
        }
        return ans;
    };
    return dfs(m - 1, 0, true);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
