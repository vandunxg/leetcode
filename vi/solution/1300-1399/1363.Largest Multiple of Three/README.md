---
comments: true
difficulty: Hard
rating: 1822
source: Weekly Contest 177 Q4
tags:
    - Greedy
    - Array
    - Math
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [1363. Largest Multiple of Three](https://leetcode.com/problems/largest-multiple-of-three)

[中文文档](/solution/1300-1399/1363.Largest%20Multiple%20of%20Three/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chữ số <code>digits</code>, hãy trả về <em>bội số lớn nhất của <strong>ba</strong> có thể tạo bằng cách ghép một số chữ số đã cho theo <strong>bất kỳ thứ tự nào</strong></em>. Nếu không có đáp án, trả về chuỗi rỗng.</p>

<p>Vì đáp án có thể không vừa với kiểu số nguyên, hãy trả về đáp án dưới dạng chuỗi. Lưu ý đáp án không được có các số 0 ở đầu không cần thiết.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> digits = [8,1,9]
<strong>Đầu ra:</strong> &quot;981&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> digits = [8,6,7,1,0]
<strong>Đầu ra:</strong> &quot;8760&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> digits = [1]
<strong>Đầu ra:</strong> &quot;&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= digits.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= digits[i] &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Quy hoạch động + Backtracking

<!-- thinking:start -->

> **Tư duy**
>
> Tạo số nguyên lớn nhất chia hết cho $3$ từ các chữ số đã cho. Có thể xóa một vài chữ số dựa trên phần dư, nhưng rất dễ chọn sai để giữ số lớn nhất theo thứ tự từ điển trong một độ dài nhất định. Một tập con chia hết cho $3$ khi và chỉ khi tổng của nó chia hết cho $3$. Sau khi sắp xếp tăng dần, $f[i][j]$ là số chữ số lớn nhất có thể chọn trong $i$ chữ số đầu sao cho tổng của chúng đồng dư $j \bmod 3$.
>
> Truy vết ngược từ $f[n][0]$ để khôi phục các chữ số được chọn. Vì mảng đã được sắp xếp, các chữ số lớn hơn được xét sau và do đó được ghi trước. Xóa các số 0 ở đầu; nếu không chọn chữ số nào thì kết quả là chuỗi rỗng.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số lượng lớn nhất các số có thể chọn trong $i$ số đầu sao cho tổng các số được chọn modulo $3$ bằng $j$. Để tạo số lớn nhất có thể, ta cần chọn nhiều số nhất có thể, tức là tối đa hóa $f[i][j]$. Khởi tạo $f[0][0] = 0$ và các giá trị $f[0][j]$ còn lại bằng $-\infty$.

Xét cách chuyển trạng thái của $f[i][j]$. Ta có thể không chọn số thứ $i$, khi đó $f[i][j] = f[i - 1][j]$; hoặc chọn số thứ $i$, khi đó $f[i][j] = f[i - 1][(j - x_i \bmod 3 + 3) \bmod 3] + 1$, trong đó $x_i$ là giá trị của số thứ $i$. Do đó, ta có công thức chuyển trạng thái:

$$
f[i][j] = \max \{ f[i - 1][j], f[i - 1][(j - x_i \bmod 3 + 3) \bmod 3] + 1 \}
$$

Nếu $f[n][0] \le 0$, ta không thể chọn số nào nên đáp án là chuỗi rỗng. Ngược lại, ta có thể truy vết ngược mảng $f$ để tìm các số được chọn.

Đặt $i = n$, $j = 0$ và bắt đầu truy vết ngược từ $f[i][j]$. Gọi $k = (j - x_i \bmod 3 + 3) \bmod 3$. Nếu $f[i - 1][k] + 1 = f[i][j]$, ta đã chọn số thứ $i$; nếu không thì không chọn. Khi đã chọn số thứ $i$, cập nhật $j$ thành $k$; nếu không, giữ nguyên $j$. Để trong các số có cùng độ dài, chọn được số lớn nhất, ta sắp xếp mảng trước.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestMultipleOfThree(self, digits: List[int]) -> str:
        digits.sort()
        n = len(digits)
        f = [[-inf] * 3 for _ in range(n + 1)]
        f[0][0] = 0
        for i, x in enumerate(digits, 1):
            for j in range(3):
                f[i][j] = max(f[i - 1][j], f[i - 1][(j - x % 3 + 3) % 3] + 1)
        if f[n][0] <= 0:
            return ""
        arr = []
        j = 0
        for i in range(n, 0, -1):
            k = (j - digits[i - 1] % 3 + 3) % 3
            if f[i - 1][k] + 1 == f[i][j]:
                arr.append(digits[i - 1])
                j = k
        i = 0
        while i < len(arr) - 1 and arr[i] == 0:
            i += 1
        return "".join(map(str, arr[i:]))
```

#### Java

```java
class Solution {
    public String largestMultipleOfThree(int[] digits) {
        Arrays.sort(digits);
        int n = digits.length;
        int[][] f = new int[n + 1][3];
        final int inf = 1 << 30;
        for (var g : f) {
            Arrays.fill(g, -inf);
        }
        f[0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < 3; ++j) {
                f[i][j] = Math.max(f[i - 1][j], f[i - 1][(j - digits[i - 1] % 3 + 3) % 3] + 1);
            }
        }
        if (f[n][0] <= 0) {
            return "";
        }
        StringBuilder sb = new StringBuilder();
        for (int i = n, j = 0; i > 0; --i) {
            int k = (j - digits[i - 1] % 3 + 3) % 3;
            if (f[i - 1][k] + 1 == f[i][j]) {
                sb.append(digits[i - 1]);
                j = k;
            }
        }
        int i = 0;
        while (i < sb.length() - 1 && sb.charAt(i) == '0') {
            ++i;
        }
        return sb.substring(i);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string largestMultipleOfThree(vector<int>& digits) {
        sort(digits.begin(), digits.end());
        int n = digits.size();
        int f[n + 1][3];
        memset(f, -0x3f, sizeof(f));
        f[0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < 3; ++j) {
                f[i][j] = max(f[i - 1][j], f[i - 1][(j - digits[i - 1] % 3 + 3) % 3] + 1);
            }
        }
        if (f[n][0] <= 0) {
            return "";
        }
        string ans;
        for (int i = n, j = 0; i; --i) {
            int k = (j - digits[i - 1] % 3 + 3) % 3;
            if (f[i - 1][k] + 1 == f[i][j]) {
                ans += digits[i - 1] + '0';
                j = k;
            }
        }
        int i = 0;
        while (i < ans.size() - 1 && ans[i] == '0') {
            ++i;
        }
        return ans.substr(i);
    }
};
```

#### Go

```go
func largestMultipleOfThree(digits []int) string {
	sort.Ints(digits)
	n := len(digits)
	const inf = 1 << 30
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, 3)
		for j := range f[i] {
			f[i][j] = -inf
		}
	}
	f[0][0] = 0
	for i := 1; i <= n; i++ {
		for j := 0; j < 3; j++ {
			f[i][j] = max(f[i-1][j], f[i-1][(j-digits[i-1]%3+3)%3]+1)
		}
	}
	if f[n][0] <= 0 {
		return ""
	}
	ans := []byte{}
	for i, j := n, 0; i > 0; i-- {
		k := (j - digits[i-1]%3 + 3) % 3
		if f[i][j] == f[i-1][k]+1 {
			ans = append(ans, byte('0'+digits[i-1]))
			j = k
		}
	}
	i := 0
	for i < len(ans)-1 && ans[i] == '0' {
		i++
	}
	return string(ans[i:])
}
```

#### TypeScript

```ts
function largestMultipleOfThree(digits: number[]): string {
    digits.sort((a, b) => a - b);
    const n = digits.length;
    const f: number[][] = new Array(n + 1).fill(0).map(() => new Array(3).fill(-Infinity));
    f[0][0] = 0;
    for (let i = 1; i <= n; ++i) {
        for (let j = 0; j < 3; ++j) {
            f[i][j] = Math.max(f[i - 1][j], f[i - 1][(j - (digits[i - 1] % 3) + 3) % 3] + 1);
        }
    }
    if (f[n][0] <= 0) {
        return '';
    }
    const arr: number[] = [];
    for (let i = n, j = 0; i; --i) {
        const k = (j - (digits[i - 1] % 3) + 3) % 3;
        if (f[i - 1][k] + 1 === f[i][j]) {
            arr.push(digits[i - 1]);
            j = k;
        }
    }
    let i = 0;
    while (i < arr.length - 1 && arr[i] === 0) {
        ++i;
    }
    return arr.slice(i).join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
