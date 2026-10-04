---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - String
    - Matrix
---

<!-- problem:start -->

# [3078. Match Alphanumerical Pattern in Matrix I 🔒](https://leetcode.com/problems/match-alphanumerical-pattern-in-matrix-i)

[中文文档](/solution/3000-3099/3078.Match%20Alphanumerical%20Pattern%20in%20Matrix%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận số nguyên 2D <code>board</code> và một ma trận ký tự 2D <code>pattern</code>. Trong đó <code>0 &lt;= board[r][c] &lt;= 9</code> và mỗi phần tử của <code>pattern</code> hoặc là một chữ số hoặc một chữ cái tiếng Anh viết thường.</p>

<p>Nhiệm vụ của bạn là tìm một <span data-keyword="submatrix">ma trận con</span> của <code>board</code> <strong>khớp</strong> với <code>pattern</code>.</p>

<p>Một ma trận số nguyên <code>part</code> khớp với <code>pattern</code> nếu ta có thể thay thế các ô chứa chữ cái trong <code>pattern</code> bằng một số chữ số nào đó (mỗi chữ cái <strong>khác nhau</strong> tương ứng với một chữ số <strong>duy nhất</strong>) sao cho ma trận thu được giống hệt ma trận số nguyên <code>part</code>. Nói cách khác,</p>

<ul>
	<li>Hai ma trận có cùng kích thước.</li>
	<li>Nếu <code>pattern[r][c]</code> là một chữ số, thì <code>part[r][c]</code> phải là chữ số <strong>giống</strong> nó.</li>
	<li>Nếu <code>pattern[r][c]</code> là chữ cái <code>x</code>:
	<ul>
		<li>Với mọi <code>pattern[i][j] == x</code>, <code>part[i][j]</code> phải <strong>giống</strong> <code>part[r][c]</code>.</li>
		<li>Với mọi <code>pattern[i][j] != x</code>, <code>part[i][j]</code> phải <strong>khác</strong> <code>part[r][c]</code>.<span style="display: none;"> </span></li>
	</ul>
	</li>
</ul>

<p>Trả về <em>một mảng có độ dài </em><code>2</code><em> chứa số hàng và số cột của góc trên bên trái của một ma trận con của </em><code>board</code><em> khớp với </em><code>pattern</code><em>. Nếu có nhiều ma trận con như vậy, trả về tọa độ của ma trận con có chỉ số hàng nhỏ nhất; nếu vẫn hòa, trả về tọa độ của ma trận con có chỉ số cột nhỏ nhất. Nếu không có đáp án phù hợp, trả về</em> <code>[-1, -1]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div style="display:flex; flex-wrap: wrap; align-items: flex-start; gap: 12px;">
<table border="1" cellspacing="3" style="border-collapse: separate; text-align: center;">
	<tbody>
		<tr>
			<td style="padding: 5px 10px; border: 1px solid red; --darkreader-inline-border-top: #b30000; --darkreader-inline-border-right: #b30000; --darkreader-inline-border-bottom: #b30000; --darkreader-inline-border-left: #b30000;">1</td>
			<td style="padding: 5px 10px; border: 1px solid red; --darkreader-inline-border-top: #b30000; --darkreader-inline-border-right: #b30000; --darkreader-inline-border-bottom: #b30000; --darkreader-inline-border-left: #b30000;">2</td>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">2</td>
		</tr>
		<tr>
			<td style="padding: 5px 10px; border: 1px solid red; --darkreader-inline-border-top: #b30000; --darkreader-inline-border-right: #b30000; --darkreader-inline-border-bottom: #b30000; --darkreader-inline-border-left: #b30000;">2</td>
			<td style="padding: 5px 10px; border: 1px solid red; --darkreader-inline-border-top: #b30000; --darkreader-inline-border-right: #b30000; --darkreader-inline-border-bottom: #b30000; --darkreader-inline-border-left: #b30000;">2</td>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">3</td>
		</tr>
		<tr>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">2</td>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">3</td>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">3</td>
		</tr>
	</tbody>
</table>

<table border="1" cellspacing="3" style="border-collapse: separate; text-align: center;">
	<tbody>
		<tr>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">a</td>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">b</td>
		</tr>
		<tr>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">b</td>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">b</td>
		</tr>
	</tbody>
</table>
</div>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">board = [[1,2,2],[2,2,3],[2,3,3]], pattern = [&quot;ab&quot;,&quot;bb&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,0]</span></p>

<p><strong>Giải thích:</strong> Xét ánh xạ: <code>&quot;a&quot; -&gt; 1</code> và <code>&quot;b&quot; -&gt; 2</code>; ma trận con có góc trên bên trái là <code>(0,0)</code> khớp như minh họa trong ma trận phía trên.</p>

<p>Lưu ý rằng ma trận con có góc trên bên trái là (1,1) cũng khớp, nhưng vì nó xuất hiện sau ma trận kia nên ta trả về <code>[0,0]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div style="display:flex; flex-wrap: wrap; align-items: flex-start; gap: 12px;">
<table border="1" cellspacing="3" style="border-collapse: separate; text-align: center;">
	<tbody>
		<tr>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">1</td>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">1</td>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">2</td>
		</tr>
		<tr>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">3</td>
			<td style="padding: 5px 10px; border: 1px solid red; --darkreader-inline-border-top: #b30000; --darkreader-inline-border-right: #b30000; --darkreader-inline-border-bottom: #b30000; --darkreader-inline-border-left: #b30000;">3</td>
			<td style="padding: 5px 10px; border: 1px solid red; --darkreader-inline-border-top: #b30000; --darkreader-inline-border-right: #b30000; --darkreader-inline-border-bottom: #b30000; --darkreader-inline-border-left: #b30000;">4</td>
		</tr>
		<tr>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">6</td>
			<td style="padding: 5px 10px; border: 1px solid red; --darkreader-inline-border-top: #b30000; --darkreader-inline-border-right: #b30000; --darkreader-inline-border-bottom: #b30000; --darkreader-inline-border-left: #b30000;">6</td>
			<td style="padding: 5px 10px; border: 1px solid red; --darkreader-inline-border-top: #b30000; --darkreader-inline-border-right: #b30000; --darkreader-inline-border-bottom: #b30000; --darkreader-inline-border-left: #b30000;">6</td>
		</tr>
	</tbody>
</table>

<table border="1" cellspacing="3" style="border-collapse: separate; text-align: center;">
	<tbody>
		<tr>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">a</td>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">b</td>
		</tr>
		<tr>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">6</td>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">6</td>
		</tr>
	</tbody>
</table>
</div>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">board = [[1,1,2],[3,3,4],[6,6,6]], pattern = [&quot;ab&quot;,&quot;66&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,1]</span></p>

<p><strong>Giải thích:</strong> Xét ánh xạ: <code>&quot;a&quot; -&gt; 3</code> và <code>&quot;b&quot; -&gt; 4</code>; ma trận con có góc trên bên trái là <code>(1,1)</code> khớp như minh họa trong ma trận phía trên.</p>

<p>Lưu ý rằng vì các giá trị tương ứng với <code>&quot;a&quot;</code> và <code>&quot;b&quot;</code> phải khác nhau, ma trận con có góc trên bên trái là <code>(1,0)</code> không khớp. Do đó, ta trả về <code>[1,1]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div style="display:flex; flex-wrap: wrap; align-items: flex-start; gap: 12px;">
<table border="1" cellspacing="3" style="border-collapse: separate; text-align: center;">
	<tbody>
		<tr>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">1</td>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">2</td>
		</tr>
		<tr>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">2</td>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">1</td>
		</tr>
	</tbody>
</table>

<table border="1" cellspacing="3" style="border-collapse: separate; text-align: center;">
	<tbody>
		<tr>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">x</td>
			<td style="padding: 5px 10px; border: 1px solid black; --darkreader-inline-border-top: #8c8273; --darkreader-inline-border-right: #8c8273; --darkreader-inline-border-bottom: #8c8273; --darkreader-inline-border-left: #8c8273;">x</td>
		</tr>
	</tbody>
</table>
</div>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">board = [[1,2],[2,1]], pattern = [&quot;xx&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1,-1]</span></p>

<p><strong>Giải thích:</strong> Vì các giá trị của ma trận con khớp phải giống nhau nên không có ma trận con nào khớp. Do đó, ta trả về <code>[-1,-1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= board.length &lt;= 50</code></li>
	<li><code>1 &lt;= board[i].length &lt;= 50</code></li>
	<li><code>0 &lt;= board[i][j] &lt;= 9</code></li>
	<li><code>1 &lt;= pattern.length &lt;= 50</code></li>
	<li><code>1 &lt;= pattern[i].length &lt;= 50</code></li>
	<li><code>pattern[i][j]</code> là một chữ số được biểu diễn dưới dạng chuỗi hoặc một chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ cái trong pattern đại diện cho những chữ số chưa biết và phải khớp theo cả hai chiều; các chữ số phải bằng giá trị trên board. Vì hai ma trận có kích thước tối đa là $50$, ta có thể thử mọi vị trí căn chỉnh.
>
> Với mỗi vị trí góc trên bên trái, ta dùng hai map để đảm bảo song ánh giữa các chữ cái và chữ số, đồng thời so sánh trực tiếp các ô số.
>
> Ta liệt kê $(i,j)$ theo thứ tự từ trên xuống dưới, từ trái sang phải và trả về kết quả khớp đầu tiên.

<!-- thinking:end -->

Gọi $m$ và $n$ lần lượt là số hàng và số cột của ma trận `board`, còn $r$ và $c$ lần lượt là số hàng và số cột của ma trận `pattern`.

Ta có thể liệt kê vị trí góc trên bên trái $(i, j)$ của mỗi ma trận con có thể có trong `board` theo thứ tự từ nhỏ đến lớn, sau đó xác định xem ma trận con $r \times c$ có $(i, j)$ là góc trên bên trái có khớp với `pattern` hay không. Nếu tìm thấy một ma trận con khớp, ta trả về $(i, j)$. Nếu không, ta trả về $(-1, -1)$.

Độ phức tạp thời gian là $O(m \times n \times r \times c)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận `board`, còn $r$ và $c$ lần lượt là số hàng và số cột của ma trận `pattern`. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là tập ký tự. Trong bài toán này, $\Sigma$ gồm các chữ số và chữ cái viết thường, nên $|\Sigma| \leq 36$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findPattern(self, board: List[List[int]], pattern: List[str]) -> List[int]:
        def check(i: int, j: int) -> bool:
            d1 = {}
            d2 = {}
            for a in range(r):
                for b in range(c):
                    x, y = i + a, j + b
                    if pattern[a][b].isdigit():
                        if int(pattern[a][b]) != board[x][y]:
                            return False
                    else:
                        if pattern[a][b] in d1 and d1[pattern[a][b]] != board[x][y]:
                            return False
                        if board[x][y] in d2 and d2[board[x][y]] != pattern[a][b]:
                            return False
                        d1[pattern[a][b]] = board[x][y]
                        d2[board[x][y]] = pattern[a][b]
            return True

        m, n = len(board), len(board[0])
        r, c = len(pattern), len(pattern[0])
        for i in range(m - r + 1):
            for j in range(n - c + 1):
                if check(i, j):
                    return [i, j]
        return [-1, -1]
```

#### Java

```java
class Solution {
    public int[] findPattern(int[][] board, String[] pattern) {
        int m = board.length, n = board[0].length;
        int r = pattern.length, c = pattern[0].length();
        for (int i = 0; i < m - r + 1; ++i) {
            for (int j = 0; j < n - c + 1; ++j) {
                if (check(board, pattern, i, j)) {
                    return new int[] {i, j};
                }
            }
        }
        return new int[] {-1, -1};
    }

    private boolean check(int[][] board, String[] pattern, int i, int j) {
        int[] d1 = new int[26];
        int[] d2 = new int[10];
        Arrays.fill(d1, -1);
        Arrays.fill(d2, -1);
        for (int a = 0; a < pattern.length; ++a) {
            for (int b = 0; b < pattern[0].length(); ++b) {
                int x = i + a, y = j + b;
                if (Character.isDigit(pattern[a].charAt(b))) {
                    int v = pattern[a].charAt(b) - '0';
                    if (v != board[x][y]) {
                        return false;
                    }
                } else {
                    int v = pattern[a].charAt(b) - 'a';
                    if (d1[v] != -1 && d1[v] != board[x][y]) {
                        return false;
                    }
                    if (d2[board[x][y]] != -1 && d2[board[x][y]] != v) {
                        return false;
                    }
                    d1[v] = board[x][y];
                    d2[board[x][y]] = v;
                }
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findPattern(vector<vector<int>>& board, vector<string>& pattern) {
        int m = board.size(), n = board[0].size();
        int r = pattern.size(), c = pattern[0].size();
        auto check = [&](int i, int j) {
            vector<int> d1(26, -1);
            vector<int> d2(10, -1);
            for (int a = 0; a < r; ++a) {
                for (int b = 0; b < c; ++b) {
                    int x = i + a, y = j + b;
                    if (isdigit(pattern[a][b])) {
                        int v = pattern[a][b] - '0';
                        if (v != board[x][y]) {
                            return false;
                        }
                    } else {
                        int v = pattern[a][b] - 'a';
                        if (d1[v] != -1 && d1[v] != board[x][y]) {
                            return false;
                        }
                        if (d2[board[x][y]] != -1 && d2[board[x][y]] != v) {
                            return false;
                        }
                        d1[v] = board[x][y];
                        d2[board[x][y]] = v;
                    }
                }
            }
            return true;
        };
        for (int i = 0; i < m - r + 1; ++i) {
            for (int j = 0; j < n - c + 1; ++j) {
                if (check(i, j)) {
                    return {i, j};
                }
            }
        }
        return {-1, -1};
    }
};
```

#### Go

```go
func findPattern(board [][]int, pattern []string) []int {
	m, n := len(board), len(board[0])
	r, c := len(pattern), len(pattern[0])
	check := func(i, j int) bool {
		d1 := [26]int{}
		d2 := [10]int{}
		for a := 0; a < r; a++ {
			for b := 0; b < c; b++ {
				x, y := i+a, j+b
				if pattern[a][b] >= '0' && pattern[a][b] <= '9' {
					v := int(pattern[a][b] - '0')
					if v != board[x][y] {
						return false
					}
				} else {
					v := int(pattern[a][b] - 'a')
					if d1[v] > 0 && d1[v]-1 != board[x][y] {
						return false
					}
					if d2[board[x][y]] > 0 && d2[board[x][y]]-1 != v {
						return false
					}
					d1[v] = board[x][y] + 1
					d2[board[x][y]] = v + 1
				}
			}
		}
		return true
	}
	for i := 0; i < m-r+1; i++ {
		for j := 0; j < n-c+1; j++ {
			if check(i, j) {
				return []int{i, j}
			}
		}
	}
	return []int{-1, -1}
}
```

#### TypeScript

```ts
function findPattern(board: number[][], pattern: string[]): number[] {
    const m: number = board.length;
    const n: number = board[0].length;
    const r: number = pattern.length;
    const c: number = pattern[0].length;

    const check = (i: number, j: number): boolean => {
        const d1: number[] = Array(26).fill(-1);
        const d2: number[] = Array(10).fill(-1);

        for (let a = 0; a < r; ++a) {
            for (let b = 0; b < c; ++b) {
                const x: number = i + a;
                const y: number = j + b;

                if (!isNaN(Number(pattern[a][b]))) {
                    const v: number = Number(pattern[a][b]);
                    if (v !== board[x][y]) {
                        return false;
                    }
                } else {
                    const v: number = pattern[a].charCodeAt(b) - 'a'.charCodeAt(0);
                    if (d1[v] !== -1 && d1[v] !== board[x][y]) {
                        return false;
                    }
                    if (d2[board[x][y]] !== -1 && d2[board[x][y]] !== v) {
                        return false;
                    }
                    d1[v] = board[x][y];
                    d2[board[x][y]] = v;
                }
            }
        }
        return true;
    };

    for (let i = 0; i < m - r + 1; ++i) {
        for (let j = 0; j < n - c + 1; ++j) {
            if (check(i, j)) {
                return [i, j];
            }
        }
    }
    return [-1, -1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
