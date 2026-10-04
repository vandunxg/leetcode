---
comments: true
difficulty: Easy
tags:
    - Hash Table
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [3032. Count Numbers With Unique Digits II 🔒](https://leetcode.com/problems/count-numbers-with-unique-digits-ii)

[中文文档](/solution/3000-3099/3032.Count%20Numbers%20With%20Unique%20Digits%20II/README.md)

## Mô tả

<!-- description:start -->

Cho hai số nguyên <strong>dương</strong> <code>a</code> và <code>b</code>, hãy trả về <em>số lượng các số có các chữ số <strong>không trùng nhau</strong> trong khoảng</em> <code>[a, b]</code> <em>(<strong>bao gồm cả hai đầu mút</strong>).</em>
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 1, b = 20
<strong>Đầu ra:</strong> 19
<strong>Giải thích:</strong> Tất cả các số trong khoảng [1, 20] đều có các chữ số không trùng nhau, ngoại trừ 11. Vì vậy, đáp án là 19.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 9, b = 19
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Tất cả các số trong khoảng [9, 19] đều có các chữ số không trùng nhau, ngoại trừ 11. Vì vậy, đáp án là 10.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 80, b = 120
<strong>Đầu ra:</strong> 27
<strong>Giải thích:</strong> Có 41 số trong khoảng [80, 120], trong đó 27 số có các chữ số không trùng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= a &lt;= b &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Trạng thái nén + Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta đếm các số nguyên trong $[a,b]$ có các chữ số phân biệt, tức là tính $f(b)-f(a-1)$. Khi liệt kê các chữ số, cần xử lý chữ số 0 ở đầu và các chữ số lặp lại.
>
> Bit mask lưu các chữ số đã dùng cùng với cờ giới hạn là một cách chuẩn để thực hiện digit DP. Vì $b \le 1000$, số trạng thái rất nhỏ.
>
> Chúng ta memoize $\textit{dfs}(\textit{pos}, \textit{mask}, \textit{limit})$. Các chữ số 0 ở đầu không được đưa vào mask; mask bằng 0 nghĩa là chưa đặt chữ số nào.

<!-- thinking:end -->

Bài toán yêu cầu đếm có bao nhiêu số trong khoảng $[a, b]$ có các chữ số không trùng nhau. Chúng ta có thể giải bài này bằng trạng thái nén và digit DP.

Ta có thể dùng hàm $f(n)$ để đếm số lượng các số trong khoảng $[1, n]$ có các chữ số không trùng nhau. Khi đó, đáp án là $f(b) - f(a - 1)$.

Ngoài ra, ta có thể dùng một số nhị phân để ghi lại các chữ số đã xuất hiện trong số hiện tại. Ví dụ, nếu các chữ số $1, 3, 5$ đã xuất hiện, ta có thể dùng $10101$ để biểu diễn trạng thái này.

Tiếp theo, ta dùng tìm kiếm có memoization để thực hiện digit DP. Ta tìm kiếm từ vị trí bắt đầu xuống lớp cuối để lấy số lượng phương án, trả về đáp án theo từng lớp và cộng dồn chúng, cuối cùng nhận được đáp án từ lần tìm kiếm tại vị trí bắt đầu.

Các bước chính như sau:

1. Ta chuyển số $n$ thành chuỗi $num$, trong đó $num[0]$ là chữ số cao nhất và $num[len - 1]$ là chữ số thấp nhất.
2. Dựa trên thông tin của đề bài, ta thiết kế hàm $dfs(pos, mask, limit)$, trong đó $pos$ biểu thị vị trí đang xử lý, $mask$ biểu thị các chữ số đã xuất hiện trong số hiện tại, còn $limit$ biểu thị vị trí hiện tại có bị giới hạn hay không. Nếu $limit$ là true, chữ số tại vị trí hiện tại không được lớn hơn $num[pos]$.

Đáp án là $dfs(0, 0, true)$.

Độ phức tạp thời gian là $O(m \times 2^{10} \times 10)$, độ phức tạp không gian là $O(m \times 2^{10})$. Trong đó, $m$ là số chữ số của $b$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberCount(self, a: int, b: int) -> int:
        @cache
        def dfs(pos: int, mask: int, limit: bool) -> int:
            if pos >= len(num):
                return 1 if mask else 0
            up = int(num[pos]) if limit else 9
            ans = 0
            for i in range(up + 1):
                if mask >> i & 1:
                    continue
                nxt = 0 if mask == 0 and i == 0 else mask | 1 << i
                ans += dfs(pos + 1, nxt, limit and i == up)
            return ans

        num = str(a - 1)
        x = dfs(0, 0, True)
        dfs.cache_clear()
        num = str(b)
        y = dfs(0, 0, True)
        return y - x
```

#### Java

```java
class Solution {
    private String num;
    private Integer[][] f;

    public int numberCount(int a, int b) {
        num = String.valueOf(a - 1);
        f = new Integer[num.length()][1 << 10];
        int x = dfs(0, 0, true);
        num = String.valueOf(b);
        f = new Integer[num.length()][1 << 10];
        int y = dfs(0, 0, true);
        return y - x;
    }

    private int dfs(int pos, int mask, boolean limit) {
        if (pos >= num.length()) {
            return mask > 0 ? 1 : 0;
        }
        if (!limit && f[pos][mask] != null) {
            return f[pos][mask];
        }
        int up = limit ? num.charAt(pos) - '0' : 9;
        int ans = 0;
        for (int i = 0; i <= up; ++i) {
            if ((mask >> i & 1) == 1) {
                continue;
            }
            int nxt = mask == 0 && i == 0 ? 0 : mask | 1 << i;
            ans += dfs(pos + 1, nxt, limit && i == up);
        }
        if (!limit) {
            f[pos][mask] = ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberCount(int a, int b) {
        string num = to_string(b);
        int f[num.size()][1 << 10];
        memset(f, -1, sizeof(f));
        function<int(int, int, bool)> dfs = [&](int pos, int mask, bool limit) {
            if (pos >= num.size()) {
                return mask ? 1 : 0;
            }
            if (!limit && f[pos][mask] != -1) {
                return f[pos][mask];
            }
            int up = limit ? num[pos] - '0' : 9;
            int ans = 0;
            for (int i = 0; i <= up; ++i) {
                if (mask >> i & 1) {
                    continue;
                }
                int nxt = mask == 0 && i == 0 ? 0 : mask | 1 << i;
                ans += dfs(pos + 1, nxt, limit && i == up);
            }
            if (!limit) {
                f[pos][mask] = ans;
            }
            return ans;
        };

        int y = dfs(0, 0, true);
        num = to_string(a - 1);
        memset(f, -1, sizeof(f));
        int x = dfs(0, 0, true);
        return y - x;
    }
};
```

#### Go

```go
func numberCount(a int, b int) int {
	num := strconv.Itoa(b)
	f := make([][1 << 10]int, len(num))
	for i := range f {
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(pos, mask int, limit bool) int
	dfs = func(pos, mask int, limit bool) int {
		if pos >= len(num) {
			if mask != 0 {
				return 1
			}
			return 0
		}
		if !limit && f[pos][mask] != -1 {
			return f[pos][mask]
		}
		up := 9
		if limit {
			up = int(num[pos] - '0')
		}
		ans := 0
		for i := 0; i <= up; i++ {
			if mask>>i&1 == 1 {
				continue
			}
			nxt := mask | 1<<i
			if mask == 0 && i == 0 {
				nxt = 0
			}
			ans += dfs(pos+1, nxt, limit && i == up)
		}
		if !limit {
			f[pos][mask] = ans
		}
		return ans
	}
	y := dfs(0, 0, true)
	num = strconv.Itoa(a - 1)
	for i := range f {
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	x := dfs(0, 0, true)
	return y - x
}
```

#### TypeScript

```ts
function numberCount(a: number, b: number): number {
    let num: string = b.toString();
    const f: number[][] = Array(num.length)
        .fill(0)
        .map(() => Array(1 << 10).fill(-1));
    const dfs: (pos: number, mask: number, limit: boolean) => number = (pos, mask, limit) => {
        if (pos >= num.length) {
            return mask ? 1 : 0;
        }
        if (!limit && f[pos][mask] !== -1) {
            return f[pos][mask];
        }
        const up: number = limit ? +num[pos] : 9;
        let ans: number = 0;
        for (let i = 0; i <= up; i++) {
            if ((mask >> i) & 1) {
                continue;
            }
            let nxt: number = mask | (1 << i);
            if (mask === 0 && i === 0) {
                nxt = 0;
            }
            ans += dfs(pos + 1, nxt, limit && i === up);
        }
        if (!limit) {
            f[pos][mask] = ans;
        }
        return ans;
    };

    const y: number = dfs(0, 0, true);
    num = (a - 1).toString();
    f.forEach(v => v.fill(-1));
    const x: number = dfs(0, 0, true);
    return y - x;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> $a,b \le 1000$, vì vậy không cần các bảng digit DP.
>
> Ta có thể liệt kê mọi số nguyên trong khoảng và kiểm tra tính duy nhất của các chữ số bằng một set. Cách này ngắn gọn và đủ nhanh.

<!-- thinking:end -->

Vì $1 \le a \le b \le 1000$, ta có thể liệt kê mọi số nguyên trong $[a, b]$ và kiểm tra xem các chữ số của chúng có duy nhất hay không.

Độ phức tạp thời gian là $O((b - a + 1) \times \log b)$, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberCount(self, a: int, b: int) -> int:
        return sum(len(set(str(num))) == len(str(num)) for num in range(a, b + 1))
```

#### Java

```java
class Solution {
    public int numberCount(int a, int b) {
        int res = 0;
        for (int i = a; i <= b; ++i) {
            if (isValid(i)) {
                ++res;
            }
        }
        return res;
    }
    private boolean isValid(int n) {
        boolean[] present = new boolean[10];
        Arrays.fill(present, false);
        while (n > 0) {
            int dig = n % 10;
            if (present[dig]) {
                return false;
            }
            present[dig] = true;
            n /= 10;
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isvalid(int n) {
        vector<bool> present(10, false);
        while (n) {
            const int dig = n % 10;
            if (present[dig])
                return false;
            present[dig] = true;
            n /= 10;
        }
        return true;
    }
    int numberCount(int a, int b) {
        int res = 0;
        for (int i = a; i <= b; ++i) {
            if (isvalid(i)) {
                ++res;
            }
        }
        return res;
    }
};
```

#### Go

```go
func numberCount(a int, b int) int {
	count := 0
	for num := a; num <= b; num++ {
		if hasUniqueDigits(num) {
			count++
		}
	}
	return count
}
func hasUniqueDigits(num int) bool {
	digits := strconv.Itoa(num)
	seen := make(map[rune]bool)
	for _, digit := range digits {
		if seen[digit] {
			return false
		}
		seen[digit] = true
	}
	return true
}
```

#### TypeScript

```ts
function numberCount(a: number, b: number): number {
    let count: number = 0;
    for (let num = a; num <= b; num++) {
        if (hasUniqueDigits(num)) {
            count++;
        }
    }
    return count;
}
function hasUniqueDigits(num: number): boolean {
    const digits: Set<string> = new Set(num.toString().split(''));
    return digits.size === num.toString().length;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
