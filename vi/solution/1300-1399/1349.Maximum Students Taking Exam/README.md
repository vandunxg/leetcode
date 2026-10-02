---
comments: true
difficulty: Hard
rating: 2385
source: Weekly Contest 175 Q4
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Bitmask
    - Matrix
    - Min Cut
    - Bipartite Graph
    - Max Flow
    - Graph Matching
    - Maximum Matching
    - Edmonds–Karp
    - Dinic
    - MPM
    - Push-Relabel
    - Network Flow
---

<!-- problem:start -->

# [1349. Maximum Students Taking Exam](https://leetcode.com/problems/maximum-students-taking-exam)

[中文文档](/solution/1300-1399/1349.Maximum%20Students%20Taking%20Exam/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận <code>m&nbsp;* n</code> <code>seats</code> biểu diễn cách bố trí chỗ ngồi trong phòng thi. Ghế bị hỏng được ký hiệu bằng ký tự <code>&#39;#&#39;</code>, còn ghế nguyên vẹn được ký hiệu bằng <code>&#39;.&#39;</code>.</p>

<p>Sinh viên có thể nhìn thấy bài làm của người ngồi bên trái, bên phải, chéo trên trái và chéo trên phải, nhưng không thể nhìn thấy bài của người ngồi ngay phía trước hoặc phía sau. Hãy trả về số sinh viên <strong>tối đa</strong> có thể cùng dự thi mà không thể gian lận.</p>

<p>Sinh viên chỉ được xếp vào những ghế còn nguyên vẹn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img height="200" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1349.Maximum%20Students%20Taking%20Exam/images/image.png" width="339" />
<pre>
<strong>Đầu vào:</strong> seats = [[&quot;#&quot;,&quot;.&quot;,&quot;#&quot;,&quot;#&quot;,&quot;.&quot;,&quot;#&quot;],
&nbsp;               [&quot;.&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;.&quot;],
&nbsp;               [&quot;#&quot;,&quot;.&quot;,&quot;#&quot;,&quot;#&quot;,&quot;.&quot;,&quot;#&quot;]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Giáo viên có thể xếp 4 sinh viên vào các ghế còn trống sao cho họ không thể gian lận trong phòng thi. 
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> seats = [[&quot;.&quot;,&quot;#&quot;],
&nbsp;               [&quot;#&quot;,&quot;#&quot;],
&nbsp;               [&quot;#&quot;,&quot;.&quot;],
&nbsp;               [&quot;#&quot;,&quot;#&quot;],
&nbsp;               [&quot;.&quot;,&quot;#&quot;]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Xếp tất cả sinh viên vào các ghế còn trống. 

</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> seats = [[&quot;#&quot;,&quot;.&quot;,&quot;<strong>.</strong>&quot;,&quot;.&quot;,&quot;#&quot;],
&nbsp;               [&quot;<strong>.</strong>&quot;,&quot;#&quot;,&quot;<strong>.</strong>&quot;,&quot;#&quot;,&quot;<strong>.</strong>&quot;],
&nbsp;               [&quot;<strong>.</strong>&quot;,&quot;.&quot;,&quot;#&quot;,&quot;.&quot;,&quot;<strong>.</strong>&quot;],
&nbsp;               [&quot;<strong>.</strong>&quot;,&quot;#&quot;,&quot;<strong>.</strong>&quot;,&quot;#&quot;,&quot;<strong>.</strong>&quot;],
&nbsp;               [&quot;#&quot;,&quot;.&quot;,&quot;<strong>.</strong>&quot;,&quot;.&quot;,&quot;#&quot;]]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Xếp sinh viên vào các ghế còn trống ở cột 1, 3 và 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>seats</code>&nbsp;contains only characters&nbsp;<code>&#39;.&#39;<font face="sans-serif, Arial, Verdana, Trebuchet MS">&nbsp;and</font></code><code>&#39;#&#39;.</code></li>
	<li><code>m ==&nbsp;seats.length</code></li>
	<li><code>n ==&nbsp;seats[i].length</code></li>
	<li><code>1 &lt;= m &lt;= 8</code></li>
	<li><code>1 &lt;= n &lt;= 8</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nén trạng thái + Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Lưới có kích thước tối đa $8 \times 8$; sinh viên không thể ngồi cạnh nhau theo chiều ngang hoặc chéo phía trước. Chọn tập con trên toàn bộ số ghế cần xét $2^{mn}$ trường hợp, nhưng mỗi hàng chỉ có $2^n$ mask. Mã hóa các ghế còn trống, thử những mask chỉ chọn ghế trống và không có hai bit kề nhau, sau đó loại các ghế ở hàng tiếp theo bị tấn công theo đường chéo. $dfs(\textit{seat},i)$ có ghi nhớ lưu số sinh viên tối đa có thể xếp.

<!-- thinking:end -->

Mỗi ghế có hai trạng thái: có thể chọn hoặc không thể chọn. Vì vậy, ta dùng số nhị phân để biểu diễn trạng thái ghế của từng hàng, trong đó $1$ nghĩa là có thể chọn và $0$ nghĩa là không thể chọn. Ví dụ, hàng đầu tiên trong Ví dụ 1 có thể được biểu diễn là $010010$. Ta chuyển trạng thái ghế ban đầu thành mảng một chiều $ss$, trong đó $ss[i]$ biểu diễn trạng thái ghế của hàng thứ $i$.

Tiếp theo, ta thiết kế hàm $dfs(seat, i)$ biểu diễn số sinh viên tối đa có thể xếp từ hàng thứ $i$ trở đi, với trạng thái ghế của hàng hiện tại là $seat$.

Ta có thể duyệt mọi trạng thái chọn ghế $mask$ của hàng thứ $i$ và kiểm tra xem $mask$ có thỏa mãn các điều kiện sau không:

- Trạng thái $mask$ không được chọn những ghế nằm ngoài $seat$;
- Trạng thái $mask$ không được chọn các ghế liền kề nhau.

Nếu thỏa mãn các điều kiện, ta tính số ghế được chọn ở hàng hiện tại là $cnt$. Nếu đây là hàng cuối, cập nhật giá trị trả về của hàm thành $ans = \max(ans, cnt)$. Nếu không, ta tiếp tục tìm đệ quy số sinh viên tối đa ở hàng tiếp theo. Trạng thái ghế của hàng kế tiếp là $nxt = ss[i + 1]$; ta cần loại các ghế bên trái và bên phải theo đường chéo của những ghế được chọn ở hàng hiện tại. Sau đó, ta đệ quy tìm số sinh viên tối đa ở các hàng còn lại, tức $ans = \max(ans, cnt + dfs(nxt, i + 1))$.

Cuối cùng, ta trả về $ans$.

Để tránh tính toán lặp lại, ta dùng tìm kiếm có ghi nhớ để lưu giá trị trả về của hàm $dfs(seat, i)$ trong mảng hai chiều $f$, trong đó $f[seat][i]$ là số sinh viên tối đa có thể xếp từ hàng thứ $i$ trở đi khi trạng thái ghế của hàng hiện tại là $seat$.

Độ phức tạp thời gian là $O(4^n \times n \times m)$ và độ phức tạp không gian là $O(2^n \times m)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận ghế.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxStudents(self, seats: List[List[str]]) -> int:
        def f(seat: List[str]) -> int:
            mask = 0
            for i, c in enumerate(seat):
                if c == '.':
                    mask |= 1 << i
            return mask

        @cache
        def dfs(seat: int, i: int) -> int:
            ans = 0
            for mask in range(1 << n):
                if (seat | mask) != seat or (mask & (mask << 1)):
                    continue
                cnt = mask.bit_count()
                if i == len(ss) - 1:
                    ans = max(ans, cnt)
                else:
                    nxt = ss[i + 1]
                    nxt &= ~(mask << 1)
                    nxt &= ~(mask >> 1)
                    ans = max(ans, cnt + dfs(nxt, i + 1))
            return ans

        n = len(seats[0])
        ss = [f(s) for s in seats]
        return dfs(ss[0], 0)
```

#### Java

```java
class Solution {
    private Integer[][] f;
    private int n;
    private int[] ss;

    public int maxStudents(char[][] seats) {
        int m = seats.length;
        n = seats[0].length;
        ss = new int[m];
        f = new Integer[1 << n][m];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (seats[i][j] == '.') {
                    ss[i] |= 1 << j;
                }
            }
        }
        return dfs(ss[0], 0);
    }

    private int dfs(int seat, int i) {
        if (f[seat][i] != null) {
            return f[seat][i];
        }
        int ans = 0;
        for (int mask = 0; mask < 1 << n; ++mask) {
            if ((seat | mask) != seat || (mask & (mask << 1)) != 0) {
                continue;
            }
            int cnt = Integer.bitCount(mask);
            if (i == ss.length - 1) {
                ans = Math.max(ans, cnt);
            } else {
                int nxt = ss[i + 1];
                nxt &= ~(mask << 1);
                nxt &= ~(mask >> 1);
                ans = Math.max(ans, cnt + dfs(nxt, i + 1));
            }
        }
        return f[seat][i] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxStudents(vector<vector<char>>& seats) {
        int m = seats.size();
        int n = seats[0].size();
        vector<int> ss(m);
        vector<vector<int>> f(1 << n, vector<int>(m, -1));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (seats[i][j] == '.') {
                    ss[i] |= 1 << j;
                }
            }
        }
        function<int(int, int)> dfs = [&](int seat, int i) -> int {
            if (f[seat][i] != -1) {
                return f[seat][i];
            }
            int ans = 0;
            for (int mask = 0; mask < 1 << n; ++mask) {
                if ((seat | mask) != seat || (mask & (mask << 1)) != 0) {
                    continue;
                }
                int cnt = __builtin_popcount(mask);
                if (i == m - 1) {
                    ans = max(ans, cnt);
                } else {
                    int nxt = ss[i + 1];
                    nxt &= ~(mask >> 1);
                    nxt &= ~(mask << 1);
                    ans = max(ans, cnt + dfs(nxt, i + 1));
                }
            }
            return f[seat][i] = ans;
        };
        return dfs(ss[0], 0);
    }
};
```

#### Go

```go
func maxStudents(seats [][]byte) int {
	m, n := len(seats), len(seats[0])
	ss := make([]int, m)
	f := make([][]int, 1<<n)
	for i, seat := range seats {
		for j, c := range seat {
			if c == '.' {
				ss[i] |= 1 << j
			}
		}
	}
	for i := range f {
		f[i] = make([]int, m)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(int, int) int
	dfs = func(seat, i int) int {
		if f[seat][i] != -1 {
			return f[seat][i]
		}
		ans := 0
		for mask := 0; mask < 1<<n; mask++ {
			if (seat|mask) != seat || (mask&(mask<<1)) != 0 {
				continue
			}
			cnt := bits.OnesCount(uint(mask))
			if i == m-1 {
				ans = max(ans, cnt)
			} else {
				nxt := ss[i+1] & ^(mask >> 1) & ^(mask << 1)
				ans = max(ans, cnt+dfs(nxt, i+1))
			}
		}
		f[seat][i] = ans
		return ans
	}
	return dfs(ss[0], 0)
}
```

#### TypeScript

```ts
function maxStudents(seats: string[][]): number {
    const m: number = seats.length;
    const n: number = seats[0].length;
    const ss: number[] = Array(m).fill(0);
    const f: number[][] = Array.from({ length: 1 << n }, () => Array(m).fill(-1));
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (seats[i][j] === '.') {
                ss[i] |= 1 << j;
            }
        }
    }

    const dfs = (seat: number, i: number): number => {
        if (f[seat][i] !== -1) {
            return f[seat][i];
        }
        let ans: number = 0;
        for (let mask = 0; mask < 1 << n; ++mask) {
            if ((seat | mask) !== seat || (mask & (mask << 1)) !== 0) {
                continue;
            }
            const cnt: number = mask.toString(2).split('1').length - 1;
            if (i === m - 1) {
                ans = Math.max(ans, cnt);
            } else {
                let nxt: number = ss[i + 1];
                nxt &= ~(mask >> 1);
                nxt &= ~(mask << 1);
                ans = Math.max(ans, cnt + dfs(nxt, i + 1));
            }
        }
        return (f[seat][i] = ans);
    };
    return dfs(ss[0], 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
