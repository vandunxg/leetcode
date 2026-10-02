---
comments: true
difficulty: Medium
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [788. Rotated Digits](https://leetcode.com/problems/rotated-digits)

[中文文档](/solution/0700-0799/0788.Rotated%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Một số nguyên <code>x</code> được gọi là <strong>tốt</strong> nếu sau khi xoay riêng từng chữ số 180 độ, ta thu được một số hợp lệ khác <code>x</code>. Phải xoay mọi chữ số, không được chọn giữ nguyên chữ số nào.</p>

<p>Một số hợp lệ nếu sau khi xoay, mỗi chữ số vẫn là một chữ số. Ví dụ:</p>

<ul>
	<li><code>0</code>, <code>1</code> và <code>8</code> xoay thành chính chúng,</li>
	<li><code>2</code> và <code>5</code> xoay thành nhau (chúng xoay theo hướng ngược nhau, hay nói cách khác, <code>2</code> hoặc <code>5</code> bị phản chiếu),</li>
	<li><code>6</code> và <code>9</code> xoay thành nhau, còn</li>
	<li>các chữ số còn lại khi xoay không tạo thành chữ số nào khác nên không hợp lệ.</li>
</ul>

<p>Cho số nguyên <code>n</code>, hãy trả về <em>số lượng số nguyên <strong>tốt</strong> trong đoạn </em><code>[1, n]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có bốn số tốt trong đoạn [1, 10]: 2, 5, 6, 9.
Lưu ý rằng 1 và 10 không phải số tốt vì chúng không thay đổi sau khi xoay.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 0
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Số tốt phải hợp lệ sau khi xoay và phải thay đổi. Vì $n\le 10^4$, ta có thể kiểm tra từng số nguyên.
>
> Nếu có chữ số không hợp lệ thì kết luận số không tốt; nếu không, tạo giá trị sau khi xoay bằng bảng ánh xạ rồi so sánh với $x$.

<!-- thinking:end -->

Cách làm trực quan và hiệu quả là duyệt trực tiếp từng số trong $[1,2,..n]$ để xác định số đó có tốt hay không. Nếu số đó tốt, tăng đáp án thêm một.

Mấu chốt của bài toán là xác định một số $x$ có phải số tốt hay không. Cách thực hiện như sau:

Trước tiên, dùng mảng $d$ có độ dài 10 để lưu chữ số sau khi xoay tương ứng với mỗi chữ số hợp lệ. Trong bài này, các chữ số hợp lệ là $[0, 1, 8, 2, 5, 6, 9]$, tương ứng với các chữ số sau khi xoay $[0, 1, 8, 5, 2, 9, 6]$. Nếu chữ số không hợp lệ, ta đặt giá trị tương ứng sau khi xoay bằng $-1$.

Tiếp theo, duyệt từng chữ số $v$ của số $x$. Nếu $v$ không hợp lệ thì $x$ không phải số tốt, ta trả về ngay $\textit{false}$. Nếu hợp lệ, cộng chữ số sau khi xoay $d[v]$ tương ứng vào $y$. Cuối cùng, kiểm tra $x$ và $y$ có bằng nhau không. Nếu khác nhau, $x$ là số tốt và ta trả về $\textit{true}$.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là số được cho. Độ phức tạp không gian là $O(1)$.

Bài toán tương tự:

- [1056. Confusing Number](https://github.com/doocs/leetcode/blob/main/solution/1000-1099/1056.Confusing%20Number/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rotatedDigits(self, n: int) -> int:
        def check(x):
            y, t = 0, x
            k = 1
            while t:
                v = t % 10
                if d[v] == -1:
                    return False
                y = d[v] * k + y
                k *= 10
                t //= 10
            return x != y

        d = [0, 1, 5, -1, -1, 2, 9, -1, 8, 6]
        return sum(check(i) for i in range(1, n + 1))
```

#### Java

```java
class Solution {
    private int[] d = new int[] {0, 1, 5, -1, -1, 2, 9, -1, 8, 6};

    public int rotatedDigits(int n) {
        int ans = 0;
        for (int i = 1; i <= n; ++i) {
            if (check(i)) {
                ++ans;
            }
        }
        return ans;
    }

    private boolean check(int x) {
        int y = 0, t = x;
        int k = 1;
        while (t > 0) {
            int v = t % 10;
            if (d[v] == -1) {
                return false;
            }
            y = d[v] * k + y;
            k *= 10;
            t /= 10;
        }
        return x != y;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int rotatedDigits(int n) {
        int d[10] = {0, 1, 5, -1, -1, 2, 9, -1, 8, 6};
        auto check = [&](int x) -> bool {
            int y = 0, t = x;
            int k = 1;
            while (t) {
                int v = t % 10;
                if (d[v] == -1) {
                    return false;
                }
                y = d[v] * k + y;
                k *= 10;
                t /= 10;
            }
            return x != y;
        };
        int ans = 0;
        for (int i = 1; i <= n; ++i) {
            ans += check(i);
        }
        return ans;
    }
};
```

#### Go

```go
func rotatedDigits(n int) int {
	d := []int{0, 1, 5, -1, -1, 2, 9, -1, 8, 6}
	check := func(x int) bool {
		y, t := 0, x
		k := 1
		for ; t > 0; t /= 10 {
			v := t % 10
			if d[v] == -1 {
				return false
			}
			y = d[v]*k + y
			k *= 10
		}
		return x != y
	}
	ans := 0
	for i := 1; i <= n; i++ {
		if check(i) {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function rotatedDigits(n: number): number {
    const d: number[] = [0, 1, 5, -1, -1, 2, 9, -1, 8, 6];
    const check = (x: number): boolean => {
        let y = 0;
        let t = x;
        let k = 1;

        while (t > 0) {
            const v = t % 10;
            if (d[v] === -1) {
                return false;
            }
            y = d[v] * k + y;
            k *= 10;
            t = Math.floor(t / 10);
        }
        return x !== y;
    };
    return Array.from({ length: n }, (_, i) => i + 1).filter(check).length;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 không hiệu quả khi giới hạn vượt quá $10^4$. Tính hợp lệ chỉ phụ thuộc vào các chữ số, vì vậy có thể dùng digit DP để đếm trong $[1,n]$.
>
> $dfs(i,ok,limit)$: $ok$ cho biết đã xuất hiện chữ số $2/5/6/9$ hay chưa. Bỏ qua chữ số không hợp lệ; khi kết thúc, trả về $ok$.

<!-- thinking:end -->

Lời giải 1 đủ để giải bài toán này, nhưng độ phức tạp thời gian khá cao. Nếu phạm vi dữ liệu lên đến $10^9$, cách làm ở Lời giải 1 sẽ vượt quá giới hạn thời gian.

Bài toán thực chất yêu cầu đếm số lượng số trong đoạn $[l, ..r]$ thỏa mãn một số điều kiện. Điều kiện phụ thuộc vào cấu tạo các chữ số chứ không phụ thuộc vào độ lớn của số, nên ta có thể dùng Digit DP. Với Digit DP, độ lớn của số ít ảnh hưởng đến độ phức tạp.

Với bài toán trên đoạn $[l, ..r]$, ta thường chuyển thành bài toán trên $[1, ..r]$ rồi trừ đi kết quả trên $[1, ..l - 1]$, tức là:

$$
ans = \sum_{i=1}^{r} ans_i -  \sum_{i=1}^{l-1} ans_i
$$

Tuy nhiên, với bài toán này, ta chỉ cần tìm kết quả trên đoạn $[1, ..r]$.

Ở đây, ta dùng tìm kiếm có ghi nhớ để triển khai Digit DP. Ta tìm kiếm từ vị trí bắt đầu xuống các mức sâu hơn; ở mức cuối cùng, ta nhận được số nghiệm. Sau đó, các kết quả được trả ngược lên từng mức để cuối cùng thu được đáp án tại vị trí bắt đầu.

Các bước cơ bản như sau:

Chuyển số $n$ thành chuỗi $s$. Sau đó, định nghĩa hàm $\textit{dfs}(i, \textit{ok}, \textit{limit})$, trong đó $i$ là vị trí chữ số, $\textit{ok}$ cho biết số hiện tại có thỏa mãn điều kiện bài toán hay không, còn $\textit{limit}$ là giá trị boolean cho biết các chữ số có thể điền có bị giới hạn hay không.

Hàm hoạt động như sau:

Nếu $i$ lớn hơn hoặc bằng độ dài chuỗi $s$, trả về $\textit{ok}$;

Ngược lại, lấy chữ số hiện tại làm $up$. Nếu $\textit{limit}$ là $\textit{true}$ thì $up$ bằng chữ số hiện tại; nếu không thì $up$ bằng $9$;

Tiếp theo, duyệt $[0, ..up]$. Nếu $j$ là chữ số hợp lệ thuộc $[0, 1, 8]$, gọi đệ quy $\textit{dfs}(i + 1, \textit{ok}, \textit{limit} \land j = \textit{up})$; nếu $j$ là chữ số hợp lệ thuộc $[2, 5, 6, 9]$, gọi đệ quy $\textit{dfs}(i + 1, 1, \textit{limit} \land j = \textit{up})$. Cộng kết quả của mọi lời gọi đệ quy rồi trả về.

Độ phức tạp thời gian là $O(\log n \times D)$, độ phức tạp không gian là $O(\log n)$. Ở đây, $D = 10$.

Bài toán tương tự:

- [233. Number of Digit One](https://github.com/doocs/leetcode/blob/main/solution/0200-0299/0233.Number%20of%20Digit%20One/README_EN.md)
- [357. Count Numbers with Unique Digits](https://github.com/doocs/leetcode/blob/main/solution/0300-0399/0357.Count%20Numbers%20with%20Unique%20Digits/README_EN.md)
- [600. Non-negative Integers without Consecutive Ones](https://github.com/doocs/leetcode/blob/main/solution/0600-0699/0600.Non-negative%20Integers%20without%20Consecutive%20Ones/README_EN.md)
- [902. Numbers At Most N Given Digit Set](https://github.com/doocs/leetcode/blob/main/solution/0900-0999/0902.Numbers%20At%20Most%20N%20Given%20Digit%20Set/README_EN.md)
- [1012. Numbers with Repeated Digits](https://github.com/doocs/leetcode/blob/main/solution/1000-1099/1012.Numbers%20With%20Repeated%20Digits/README_EN.md)
- [2376. Count Special Integers](https://github.com/doocs/leetcode/blob/main/solution/2300-2399/2376.Count%20Special%20Integers/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rotatedDigits(self, n: int) -> int:
        @cache
        def dfs(i: int, ok: int, limit: bool) -> int:
            if i >= len(s):
                return ok
            up = int(s[i]) if limit else 9
            ans = 0
            for j in range(up + 1):
                if j in (0, 1, 8):
                    ans += dfs(i + 1, ok, limit and j == up)
                elif j in (2, 5, 6, 9):
                    ans += dfs(i + 1, 1, limit and j == up)
            return ans

        s = str(n)
        return dfs(0, 0, True)
```

#### Java

```java
class Solution {
    private char[] s;
    private Integer[][] f;

    public int rotatedDigits(int n) {
        s = String.valueOf(n).toCharArray();
        f = new Integer[s.length][2];
        return dfs(0, 0, true);
    }

    private int dfs(int i, int ok, boolean limit) {
        if (i >= s.length) {
            return ok;
        }
        if (!limit && f[i][ok] != null) {
            return f[i][ok];
        }
        int up = limit ? s[i] - '0' : 9;
        int ans = 0;
        for (int j = 0; j <= up; ++j) {
            if (j == 0 || j == 1 || j == 8) {
                ans += dfs(i + 1, ok, limit && j == up);
            } else if (j == 2 || j == 5 || j == 6 || j == 9) {
                ans += dfs(i + 1, 1, limit && j == up);
            }
        }
        if (!limit) {
            f[i][ok] = ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int rotatedDigits(int n) {
        string s = to_string(n);
        int m = s.size();
        int f[m][2];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i, int ok, bool limit) -> int {
            if (i >= m) {
                return ok;
            }
            if (!limit && f[i][ok] != -1) {
                return f[i][ok];
            }
            int up = limit ? s[i] - '0' : 9;
            int ans = 0;
            for (int j = 0; j <= up; ++j) {
                if (j == 0 || j == 1 || j == 8) {
                    ans += dfs(i + 1, ok, limit && j == up);
                } else if (j == 2 || j == 5 || j == 6 || j == 9) {
                    ans += dfs(i + 1, 1, limit && j == up);
                }
            }
            if (!limit) {
                f[i][ok] = ans;
            }
            return ans;
        };
        return dfs(0, 0, true);
    }
};
```

#### Go

```go
func rotatedDigits(n int) int {
	s := strconv.Itoa(n)
	m := len(s)
	f := make([][2]int, m)
	for i := range f {
		f[i] = [2]int{-1, -1}
	}
	var dfs func(i, ok int, limit bool) int
	dfs = func(i, ok int, limit bool) int {
		if i >= m {
			return ok
		}
		if !limit && f[i][ok] != -1 {
			return f[i][ok]
		}
		up := 9
		if limit {
			up = int(s[i] - '0')
		}
		ans := 0
		for j := 0; j <= up; j++ {
			if j == 0 || j == 1 || j == 8 {
				ans += dfs(i+1, ok, limit && j == up)
			} else if j == 2 || j == 5 || j == 6 || j == 9 {
				ans += dfs(i+1, 1, limit && j == up)
			}
		}
		if !limit {
			f[i][ok] = ans
		}
		return ans
	}
	return dfs(0, 0, true)
}
```

#### TypeScript

```ts
function rotatedDigits(n: number): number {
    const s = n.toString();
    const m = s.length;
    const f: number[][] = Array.from({ length: m }, () => Array(2).fill(-1));
    const dfs = (i: number, ok: number, limit: boolean): number => {
        if (i >= m) {
            return ok;
        }
        if (!limit && f[i][ok] !== -1) {
            return f[i][ok];
        }
        const up = limit ? +s[i] : 9;
        let ans = 0;
        for (let j = 0; j <= up; ++j) {
            if ([0, 1, 8].includes(j)) {
                ans += dfs(i + 1, ok, limit && j === up);
            } else if ([2, 5, 6, 9].includes(j)) {
                ans += dfs(i + 1, 1, limit && j === up);
            }
        }
        if (!limit) {
            f[i][ok] = ans;
        }
        return ans;
    };
    return dfs(0, 0, true);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
