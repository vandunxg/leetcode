---
comments: true
difficulty: Hard
tags:
    - Array
    - Math
    - String
    - Binary Search
    - Dynamic Programming
---

<!-- problem:start -->

# [902. Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set)

[中文文档](/solution/0900-0999/0902.Numbers%20At%20Most%20N%20Given%20Digit%20Set/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>digits</code> được sắp xếp theo thứ tự <strong>không giảm</strong>. Bạn có thể dùng mỗi <code>digits[i]</code> bao nhiêu lần tùy ý để tạo số. Ví dụ, nếu <code>digits = [&#39;1&#39;,&#39;3&#39;,&#39;5&#39;]</code>, ta có thể tạo các số như <code>&#39;13&#39;</code>, <code>&#39;551&#39;</code> và <code>&#39;1351315&#39;</code>.</p>

<p>Trả về <em>số lượng số nguyên dương có thể tạo ra</em> mà không lớn hơn số nguyên <code>n</code> đã cho.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> digits = [&quot;1&quot;,&quot;3&quot;,&quot;5&quot;,&quot;7&quot;], n = 100
<strong>Output:</strong> 20
<strong>Giải thích: </strong>
20 số có thể tạo ra là:
1, 3, 5, 7, 11, 13, 15, 17, 31, 33, 35, 37, 51, 53, 55, 57, 71, 73, 75, 77.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> digits = [&quot;1&quot;,&quot;4&quot;,&quot;9&quot;], n = 1000000000
<strong>Output:</strong> 29523
<strong>Giải thích: </strong>
Ta có thể tạo 3 số có một chữ số, 9 số có hai chữ số, 27 số có ba chữ số,
81 số có bốn chữ số, 243 số có năm chữ số, 729 số có sáu chữ số,
2187 số có bảy chữ số, 6561 số có tám chữ số và 19683 số có chín chữ số.
Tổng cộng có 29523 số nguyên có thể tạo bằng các chữ số trong mảng digits.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> digits = [&quot;7&quot;], n = 8
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= digits.length &lt;= 9</code></li>
	<li><code>digits[i].length == 1</code></li>
	<li><code>digits[i]</code> là một chữ số từ <code>&#39;1&#39;</code> đến <code>&#39;9&#39;</code>.</li>
	<li>Tất cả giá trị trong <code>digits</code> đều <strong>khác nhau</strong>.</li>
	<li><code>digits</code> được sắp xếp theo thứ tự <strong>không giảm</strong>.</li>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Nếu tạo từng số từ $\textit{digits}$ rồi kiểm tra $x \le n$, ta sẽ lặp lại nhiều prefix khi độ dài số gần bằng độ dài thập phân của $n$. Số lượng cần đếm chỉ phụ thuộc vào vị trí hiện tại, việc số đang xét còn toàn chữ số 0 ở đầu hay không, và việc prefix có đang bị giới hạn bởi cận trên hay không.
>
> Viết $n$ thành chuỗi $s$ và memoize $\textit{dfs}(i, \textit{lead}, \textit{limit})$: khi vẫn đang ở trạng thái các chữ số đầu là 0, ta có thể chọn 0; nếu không, chỉ được chọn chữ số trong $\textit{digits}$ và không vượt quá giới hạn do $\textit{limit}$ đặt ra. Một số hoàn chỉnh chỉ được tính là $1$ nếu nó không phải toàn số 0 ở đầu.

<!-- thinking:end -->

Bài toán về cơ bản yêu cầu đếm số nguyên dương có thể tạo từ các chữ số trong digits thuộc đoạn $[l, .., r]$. Số lượng phụ thuộc vào số chữ số và giá trị của từng chữ số. Ta có thể giải bằng Digit DP; với phương pháp này, độ lớn của số ít ảnh hưởng đến độ phức tạp.

Với bài toán trên đoạn $[l, .., r]$, thông thường ta chuyển thành bài toán trên đoạn $[1, .., r]$ rồi trừ đi kết quả trên đoạn $[1, .., l - 1]$, tức là

$$
ans = \sum_{i=1}^{r} ans_i -  \sum_{i=1}^{l-1} ans_i
$$

Tuy nhiên, với bài này, ta chỉ cần tính kết quả cho đoạn $[1, .., r]$.

Ở đây, ta dùng memoization để triển khai Digit DP. Bắt đầu tìm kiếm từ trên xuống, tính số cách ở các trạng thái cuối rồi trả kết quả ngược lên từng lớp cho đến khi thu được đáp án tại trạng thái ban đầu.

Các bước chính như sau:

Chuyển số $n$ thành chuỗi $s$ và gọi độ dài của $s$ là $m$.

Tiếp theo, thiết kế hàm $\textit{dfs}(i, \textit{lead}, \textit{limit})$ biểu diễn số cách tạo các chữ số từ vị trí $i$ hiện tại đến chữ số cuối của chuỗi. Trong đó:

- Số nguyên $i$ biểu thị vị trí hiện tại trong chuỗi $s$.
- Giá trị boolean $\textit{lead}$ cho biết số hiện tại có đang chỉ gồm các số 0 ở đầu hay không.
- Giá trị boolean $\textit{limit}$ cho biết vị trí hiện tại có bị giới hạn bởi cận trên hay không.

Hàm hoạt động như sau:

Nếu $i$ lớn hơn hoặc bằng $m$, nghĩa là ta đã xử lý hết các chữ số. Nếu $\textit{lead}$ là true, số hiện tại vẫn chỉ gồm các số 0 ở đầu nên trả về $0$; nếu không thì trả về $1$.

Nếu chưa đến cuối, tính cận trên $\textit{up}$. Nếu $\textit{limit}$ là true thì $\textit{up}$ là chữ số tương ứng với $s[i]$; nếu không thì $\textit{up}$ bằng $9$.

Sau đó, lần lượt xét chữ số hiện tại $j$ trong đoạn $[0, \textit{up}]$. Nếu $j=0$ và $\textit{lead}$ là true, gọi đệ quy $\textit{dfs}(i + 1, \text{true}, \textit{limit} \wedge j = \textit{up})$. Nếu không, khi $j$ thuộc $\textit{digits}$, gọi đệ quy $\textit{dfs}(i + 1, \text{false}, \textit{limit} \wedge j = \textit{up})$. Cộng dồn tất cả kết quả để có đáp án.

Cuối cùng, trả về $\textit{dfs}(0, \text{true}, \text{true})$.

Độ phức tạp thời gian là $O(\log n \times D)$ và độ phức tạp không gian là $O(\log n)$, với $D = 10$.

Bài tương tự:

- [233. Number of Digit One](https://github.com/doocs/leetcode/blob/main/solution/0200-0299/0233.Number%20of%20Digit%20One/README_EN.md)
- [357. Count Numbers with Unique Digits](https://github.com/doocs/leetcode/blob/main/solution/0300-0399/0357.Count%20Numbers%20with%20Unique%20Digits/README_EN.md)
- [600. Non-negative Integers without Consecutive Ones](https://github.com/doocs/leetcode/blob/main/solution/0600-0699/0600.Non-negative%20Integers%20without%20Consecutive%20Ones/README_EN.md)
- [788. Rotated Digits](https://github.com/doocs/leetcode/blob/main/solution/0700-0799/0788.Rotated%20Digits/README_EN.md)
- [1012. Numbers With Repeated Digits](https://github.com/doocs/leetcode/blob/main/solution/1000-1099/1012.Numbers%20With%20Repeated%20Digits/README_EN.md)
- [2376. Count Special Integers](https://github.com/doocs/leetcode/blob/main/solution/2300-2399/2376.Count%20Special%20Integers/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def atMostNGivenDigitSet(self, digits: List[str], n: int) -> int:
        @cache
        def dfs(i: int, lead: int, limit: bool) -> int:
            if i >= len(s):
                return lead ^ 1

            up = int(s[i]) if limit else 9
            ans = 0
            for j in range(up + 1):
                if j == 0 and lead:
                    ans += dfs(i + 1, 1, limit and j == up)
                elif j in nums:
                    ans += dfs(i + 1, 0, limit and j == up)
            return ans

        s = str(n)
        nums = {int(x) for x in digits}
        return dfs(0, 1, True)
```

#### Java

```java
class Solution {
    private Set<Integer> nums = new HashSet<>();
    private char[] s;
    private Integer[] f;

    public int atMostNGivenDigitSet(String[] digits, int n) {
        s = String.valueOf(n).toCharArray();
        f = new Integer[s.length];
        for (var x : digits) {
            nums.add(Integer.parseInt(x));
        }
        return dfs(0, true, true);
    }

    private int dfs(int i, boolean lead, boolean limit) {
        if (i >= s.length) {
            return lead ? 0 : 1;
        }
        if (!lead && !limit && f[i] != null) {
            return f[i];
        }
        int up = limit ? s[i] - '0' : 9;
        int ans = 0;
        for (int j = 0; j <= up; ++j) {
            if (j == 0 && lead) {
                ans += dfs(i + 1, true, limit && j == up);
            } else if (nums.contains(j)) {
                ans += dfs(i + 1, false, limit && j == up);
            }
        }
        if (!lead && !limit) {
            f[i] = ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int atMostNGivenDigitSet(vector<string>& digits, int n) {
        string s = to_string(n);
        unordered_set<int> nums;
        for (auto& x : digits) {
            nums.insert(stoi(x));
        }
        int m = s.size();
        int f[m];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i, bool lead, bool limit) -> int {
            if (i >= m) {
                return lead ? 0 : 1;
            }
            if (!lead && !limit && f[i] != -1) {
                return f[i];
            }
            int up = limit ? s[i] - '0' : 9;
            int ans = 0;
            for (int j = 0; j <= up; ++j) {
                if (j == 0 && lead) {
                    ans += dfs(i + 1, true, limit && j == up);
                } else if (nums.count(j)) {
                    ans += dfs(i + 1, false, limit && j == up);
                }
            }
            if (!lead && !limit) {
                f[i] = ans;
            }
            return ans;
        };
        return dfs(0, true, true);
    }
};
```

#### Go

```go
func atMostNGivenDigitSet(digits []string, n int) int {
	s := strconv.Itoa(n)
	m := len(s)
	f := make([]int, m)
	for i := range f {
		f[i] = -1
	}
	nums := map[int]bool{}
	for _, d := range digits {
		x, _ := strconv.Atoi(d)
		nums[x] = true
	}
	var dfs func(i int, lead, limit bool) int
	dfs = func(i int, lead, limit bool) int {
		if i >= m {
			if lead {
				return 0
			}
			return 1
		}
		if !lead && !limit && f[i] != -1 {
			return f[i]
		}
		up := 9
		if limit {
			up = int(s[i] - '0')
		}
		ans := 0
		for j := 0; j <= up; j++ {
			if j == 0 && lead {
				ans += dfs(i+1, true, limit && j == up)
			} else if nums[j] {
				ans += dfs(i+1, false, limit && j == up)
			}
		}
		if !lead && !limit {
			f[i] = ans
		}
		return ans
	}
	return dfs(0, true, true)
}
```

#### TypeScript

```ts
function atMostNGivenDigitSet(digits: string[], n: number): number {
    const s = n.toString();
    const m = s.length;
    const f: number[] = Array(m).fill(-1);
    const nums = new Set<number>(digits.map(d => parseInt(d)));
    const dfs = (i: number, lead: boolean, limit: boolean): number => {
        if (i >= m) {
            return lead ? 0 : 1;
        }
        if (!lead && !limit && f[i] !== -1) {
            return f[i];
        }
        const up = limit ? +s[i] : 9;
        let ans = 0;
        for (let j = 0; j <= up; ++j) {
            if (!j && lead) {
                ans += dfs(i + 1, true, limit && j === up);
            } else if (nums.has(j)) {
                ans += dfs(i + 1, false, limit && j === up);
            }
        }
        if (!lead && !limit) {
            f[i] = ans;
        }
        return ans;
    };
    return dfs(0, true, true);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
