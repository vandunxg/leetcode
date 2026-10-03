---
comments: true
difficulty: Hard
rating: 2583
source: Weekly Contest 260 Q4
tags:
    - Stack
    - Memoization
    - Array
    - Hash Table
    - Math
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2019. The Score of Students Solving Math Expression](https://leetcode.com/problems/the-score-of-students-solving-math-expression)

[中文文档](/solution/2000-2099/2019.The%20Score%20of%20Students%20Solving%20Math%20Expression/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> <strong>chỉ</strong> chứa các chữ số <code>0-9</code>, ký hiệu cộng <code>&#39;+&#39;</code> và ký hiệu nhân <code>&#39;*&#39;</code>, biểu diễn một biểu thức toán học <strong>hợp lệ</strong> gồm các <strong>số có một chữ số</strong> (ví dụ: <code>3+5*2</code>). Biểu thức này được đưa cho <code>n</code> học sinh tiểu học. Học sinh được yêu cầu tính kết quả của biểu thức theo <strong>thứ tự thực hiện phép tính</strong> sau:</p>

<ol>
	<li>Thực hiện <strong>phép nhân</strong> từ <strong>trái sang phải</strong>; sau đó</li>
	<li>Thực hiện <strong>phép cộng</strong> từ <strong>trái sang phải</strong>.</li>
</ol>

<p>Bạn được cho một mảng số nguyên <code>answers</code> có độ dài <code>n</code>, chứa các câu trả lời do học sinh nộp theo thứ tự bất kỳ. Hãy chấm điểm <code>answers</code> theo các <strong>quy tắc</strong> sau:</p>

<ul>
	<li>Nếu một câu trả lời <strong>bằng</strong> kết quả chính xác của biểu thức, học sinh đó được <code>5</code> điểm;</li>
	<li>Nếu không, nhưng câu trả lời <strong>có thể được diễn giải</strong> là kết quả khi học sinh thực hiện các toán tử <strong>không đúng thứ tự</strong> nhưng tính toán <strong>chính xác</strong>, học sinh đó được <code>2</code> điểm;</li>
	<li>Trong các trường hợp còn lại, học sinh đó được <code>0</code> điểm.</li>
</ul>

<p>Trả về <em>tổng điểm của các học sinh</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2019.The%20Score%20of%20Students%20Solving%20Math%20Expression/images/student_solving_math.png" style="width: 678px; height: 109px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;7+3*1*2&quot;, answers = [20,13,42]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Như minh họa ở trên, kết quả chính xác của biểu thức là 13, do đó một học sinh được 5 điểm: [20,<u><strong>13</strong></u>,42]
Một học sinh có thể đã thực hiện các toán tử theo thứ tự sai như sau: ((7+3)*1)*2 = 20. Do đó một học sinh được 2 điểm: [<u><strong>20</strong></u>,13,42]
Điểm của các học sinh là: [2,5,0]. Tổng điểm là 2+5+0=7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;3+5*2&quot;, answers = [13,0,10,13,13,16,16]
<strong>Đầu ra:</strong> 19
<strong>Giải thích:</strong> Kết quả chính xác của biểu thức là 13, do đó ba học sinh được 5 điểm mỗi người: [<strong><u>13</u></strong>,0,10,<strong><u>13</u></strong>,<strong><u>13</u></strong>,16,16]
Một học sinh có thể đã thực hiện các toán tử theo thứ tự sai như sau: ((3+5)*2 = 16. Do đó hai học sinh được 2 điểm: [13,0,10,13,13,<strong><u>16</u></strong>,<strong><u>16</u></strong>]
Điểm của các học sinh là: [5,0,0,5,5,2,2]. Tổng điểm là 5+0+0+5+5+2+2=19.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;6+0*1&quot;, answers = [12,9,6,4,8,6]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Kết quả chính xác của biểu thức là 6.
Nếu một học sinh thực hiện sai (6+0)*1, kết quả cũng là 6.
Theo quy tắc chấm điểm, học sinh đó vẫn được 5 điểm (vì đã nhận được kết quả chính xác), không phải 2 điểm.
Điểm của các học sinh là: [0,0,5,0,0,5]. Tổng điểm là 10.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 31</code></li>
	<li><code>s</code> biểu diễn một biểu thức hợp lệ chỉ chứa các chữ số <code>0-9</code>, <code>&#39;+&#39;</code> và <code>&#39;*&#39;</code>.</li>
<li>Tất cả toán hạng nguyên trong biểu thức nằm trong đoạn <strong>bao gồm cả hai đầu</strong> <code>[0, 9]</code>.</li>
	<li><code>1 &lt;=</code> số lượng tất cả toán tử (<code>&#39;+&#39;</code> và <code>&#39;*&#39;</code>) trong biểu thức toán học <code>&lt;= 15</code></li>
	<li>Dữ liệu kiểm thử được tạo sao cho kết quả chính xác của biểu thức nằm trong đoạn <code>[0, 1000]</code>.</li>
	<li>Dữ liệu kiểm thử được tạo sao cho giá trị không bao giờ vượt quá 10<sup>9</sup> trong các bước trung gian của phép nhân.</li>
	<li><code>n == answers.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= answers[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động (DP trên đoạn)

<!-- thinking:start -->

> **Tư duy**
>
> Có nhiều nhất $15$ toán tử; các đáp án sai xuất phát từ những cách kết hợp khác nhau. Việc liệt kê tất cả cách đặt dấu ngoặc có số lượng theo cấp Catalan, còn DP trên đoạn sẽ hợp nhất tất cả giá trị. Kết quả chính xác được tính riêng theo thứ tự ưu tiên toán tử chuẩn.
>
> $f[i][j]$ là tập các giá trị có thể có của các chữ số $i..j$; ta tách tại $k$ và áp dụng toán tử tương ứng, đồng thời loại các kết quả $>1000$.
>
> Đếm $answers$: được $5$ điểm nếu bằng kết quả chính xác, nếu không thì được $2$ điểm nếu giá trị nằm trong $f[0][m-1]$.

<!-- thinking:end -->

Trước tiên, chúng ta xây dựng hàm $cal(s)$ để tính kết quả của một biểu thức toán học hợp lệ chỉ chứa các số có một chữ số. Kết quả chính xác là $x = cal(s)$.

Gọi độ dài của chuỗi $s$ là $n$, khi đó số chữ số trong $s$ là $m = \frac{n+1}{2}$.

Ta định nghĩa $f[i][j]$ là các giá trị có thể nhận được khi chọn các chữ số từ chữ số thứ $i$ đến chữ số thứ $j$ trong $s$ (chỉ số bắt đầu từ $0$). Ban đầu, $f[i][i]$ biểu diễn việc chọn chữ số thứ $i$, nên kết quả chỉ có thể là chính chữ số đó, tức là $f[i][i] = \{s[i \times 2]\}$ (chữ số thứ $i$ tương ứng với ký tự tại chỉ số $i \times 2$ trong chuỗi $s$).

Tiếp theo, ta duyệt $i$ từ lớn đến nhỏ, rồi duyệt $j$ từ nhỏ đến lớn. Ta cần tìm các giá trị có thể nhận được khi thực hiện phép toán trên tất cả chữ số trong đoạn $[i, j]$. Ta duyệt điểm phân chia $k$ trong đoạn $[i, j]$, khi đó có thể tính $f[i][j]$ từ $f[i][k]$ và $f[k+1][j]$ thông qua toán tử $s[k \times 2 + 1]$. Vì vậy, ta có công thức chuyển trạng thái sau:

$$
f[i][j] = \begin{cases}
\{s[i \times 2]\}, & i = j \\
\bigcup\limits_{k=i}^{j-1} \{f[i][k] \otimes f[k+1][j]\}, & i < j
\end{cases}
$$

Trong đó, $\otimes$ biểu diễn toán tử, tức là $s[k \times 2 + 1]$.

Các giá trị có thể nhận được khi thực hiện mọi phép toán trên tất cả chữ số trong chuỗi $s$ là $f[0][m-1]$.

Cuối cùng, ta đếm đáp án. Ta dùng một mảng $cnt$ để đếm số lần xuất hiện của mỗi câu trả lời trong mảng $answers$. Nếu câu trả lời bằng $x$, học sinh đó được $5$ điểm; nếu không, nếu câu trả lời thuộc $f[0][m-1]$, học sinh đó được $2$ điểm. Duyệt $cnt$ để tính tổng điểm.

Độ phức tạp thời gian là $O(n^3 \times M^2)$, độ phức tạp không gian là $O(n^2 \times M^2)$. Ở đây, $M$ là giá trị lớn nhất có thể có của đáp án, còn $n$ là số chữ số trong chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def scoreOfStudents(self, s: str, answers: List[int]) -> int:
        def cal(s: str) -> int:
            res, pre = 0, int(s[0])
            for i in range(1, n, 2):
                if s[i] == "*":
                    pre *= int(s[i + 1])
                else:
                    res += pre
                    pre = int(s[i + 1])
            res += pre
            return res

        n = len(s)
        x = cal(s)
        m = (n + 1) >> 1
        f = [[set() for _ in range(m)] for _ in range(m)]
        for i in range(m):
            f[i][i] = {int(s[i << 1])}
        for i in range(m - 1, -1, -1):
            for j in range(i, m):
                for k in range(i, j):
                    for l in f[i][k]:
                        for r in f[k + 1][j]:
                            if s[k << 1 | 1] == "+" and l + r <= 1000:
                                f[i][j].add(l + r)
                            elif s[k << 1 | 1] == "*" and l * r <= 1000:
                                f[i][j].add(l * r)
        cnt = Counter(answers)
        ans = cnt[x] * 5
        for k, v in cnt.items():
            if k != x and k in f[0][m - 1]:
                ans += v << 1
        return ans
```

#### Java

```java
class Solution {
    public int scoreOfStudents(String s, int[] answers) {
        int n = s.length();
        int x = cal(s);
        int m = (n + 1) >> 1;
        Set<Integer>[][] f = new Set[m][m];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < m; ++j) {
                f[i][j] = new HashSet<>();
            }
            f[i][i].add(s.charAt(i << 1) - '0');
        }
        for (int i = m - 1; i >= 0; --i) {
            for (int j = i; j < m; ++j) {
                for (int k = i; k < j; ++k) {
                    for (int l : f[i][k]) {
                        for (int r : f[k + 1][j]) {
                            char op = s.charAt(k << 1 | 1);
                            if (op == '+' && l + r <= 1000) {
                                f[i][j].add(l + r);
                            } else if (op == '*' && l * r <= 1000) {
                                f[i][j].add(l * r);
                            }
                        }
                    }
                }
            }
        }
        int[] cnt = new int[1001];
        for (int ans : answers) {
            ++cnt[ans];
        }
        int ans = 5 * cnt[x];
        for (int i = 0; i <= 1000; ++i) {
            if (i != x && f[0][m - 1].contains(i)) {
                ans += 2 * cnt[i];
            }
        }
        return ans;
    }

    private int cal(String s) {
        int res = 0, pre = s.charAt(0) - '0';
        for (int i = 1; i < s.length(); i += 2) {
            char op = s.charAt(i);
            int cur = s.charAt(i + 1) - '0';
            if (op == '*') {
                pre *= cur;
            } else {
                res += pre;
                pre = cur;
            }
        }
        res += pre;
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int scoreOfStudents(string s, vector<int>& answers) {
        int n = s.size();
        int x = cal(s);
        int m = (n + 1) >> 1;
        unordered_set<int> f[m][m];
        for (int i = 0; i < m; ++i) {
            f[i][i] = {s[i * 2] - '0'};
        }
        for (int i = m - 1; ~i; --i) {
            for (int j = i; j < m; ++j) {
                for (int k = i; k < j; ++k) {
                    for (int l : f[i][k]) {
                        for (int r : f[k + 1][j]) {
                            char op = s[k << 1 | 1];
                            if (op == '+' && l + r <= 1000) {
                                f[i][j].insert(l + r);
                            } else if (op == '*' && l * r <= 1000) {
                                f[i][j].insert(l * r);
                            }
                        }
                    }
                }
            }
        }
        int cnt[1001]{};
        for (int t : answers) {
            ++cnt[t];
        }
        int ans = 5 * cnt[x];
        for (int i = 0; i <= 1000; ++i) {
            if (i != x && f[0][m - 1].count(i)) {
                ans += cnt[i] << 1;
            }
        }
        return ans;
    }

    int cal(string& s) {
        int res = 0;
        int pre = s[0] - '0';
        for (int i = 1; i < s.size(); i += 2) {
            int cur = s[i + 1] - '0';
            if (s[i] == '*') {
                pre *= cur;
            } else {
                res += pre;
                pre = cur;
            }
        }
        res += pre;
        return res;
    }
};
```

#### Go

```go
func scoreOfStudents(s string, answers []int) int {
	n := len(s)
	x := cal(s)
	m := (n + 1) >> 1
	f := make([][]map[int]bool, m)
	for i := range f {
		f[i] = make([]map[int]bool, m)
		for j := range f[i] {
			f[i][j] = make(map[int]bool)
		}
		f[i][i][int(s[i<<1]-'0')] = true
	}
	for i := m - 1; i >= 0; i-- {
		for j := i; j < m; j++ {
			for k := i; k < j; k++ {
				for l := range f[i][k] {
					for r := range f[k+1][j] {
						op := s[k<<1|1]
						if op == '+' && l+r <= 1000 {
							f[i][j][l+r] = true
						} else if op == '*' && l*r <= 1000 {
							f[i][j][l*r] = true
						}
					}
				}
			}
		}
	}
	cnt := [1001]int{}
	for _, v := range answers {
		cnt[v]++
	}
	ans := cnt[x] * 5
	for k, v := range cnt {
		if k != x && f[0][m-1][k] {
			ans += v << 1
		}
	}
	return ans
}

func cal(s string) int {
	res, pre := 0, int(s[0]-'0')
	for i := 1; i < len(s); i += 2 {
		cur := int(s[i+1] - '0')
		if s[i] == '+' {
			res += pre
			pre = cur
		} else {
			pre *= cur
		}
	}
	res += pre
	return res
}
```

#### TypeScript

```ts
function scoreOfStudents(s: string, answers: number[]): number {
    const n = s.length;
    const cal = (s: string): number => {
        let res = 0;
        let pre = s.charCodeAt(0) - '0'.charCodeAt(0);
        for (let i = 1; i < s.length; i += 2) {
            const cur = s.charCodeAt(i + 1) - '0'.charCodeAt(0);
            if (s[i] === '+') {
                res += pre;
                pre = cur;
            } else {
                pre *= cur;
            }
        }
        res += pre;
        return res;
    };
    const x = cal(s);
    const m = (n + 1) >> 1;
    const f: Set<number>[][] = Array(m)
        .fill(0)
        .map(() =>
            Array(m)
                .fill(0)
                .map(() => new Set()),
        );
    for (let i = 0; i < m; ++i) {
        f[i][i].add(s[i << 1].charCodeAt(0) - '0'.charCodeAt(0));
    }
    for (let i = m - 1; i >= 0; --i) {
        for (let j = i; j < m; ++j) {
            for (let k = i; k < j; ++k) {
                for (const l of f[i][k]) {
                    for (const r of f[k + 1][j]) {
                        const op = s[(k << 1) + 1];
                        if (op === '+' && l + r <= 1000) {
                            f[i][j].add(l + r);
                        } else if (op === '*' && l * r <= 1000) {
                            f[i][j].add(l * r);
                        }
                    }
                }
            }
        }
    }
    const cnt: number[] = Array(1001).fill(0);
    for (const v of answers) {
        ++cnt[v];
    }
    let ans = cnt[x] * 5;
    for (let i = 0; i <= 1000; ++i) {
        if (i !== x && f[0][m - 1].has(i)) {
            ans += cnt[i] << 1;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
