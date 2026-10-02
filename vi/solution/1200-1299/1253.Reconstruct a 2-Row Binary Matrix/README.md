---
comments: true
difficulty: Medium
rating: 1505
source: Weekly Contest 162 Q2
tags:
    - Greedy
    - Array
    - Matrix
---

<!-- problem:start -->

# [1253. Reconstruct a 2-Row Binary Matrix](https://leetcode.com/problems/reconstruct-a-2-row-binary-matrix)

[中文文档](/solution/1200-1299/1253.Reconstruct%20a%202-Row%20Binary%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho các thông tin sau về một ma trận có <code>n</code> cột và <code>2</code> hàng:</p>

<ul>
	<li>Đây là ma trận nhị phân, nghĩa là mỗi phần tử có thể là <code>0</code> hoặc <code>1</code>.</li>
	<li>Tổng các phần tử của hàng thứ 0 (hàng trên) được cho bởi <code>upper</code>.</li>
	<li>Tổng các phần tử của hàng thứ 1 (hàng dưới) được cho bởi <code>lower</code>.</li>
	<li>Tổng các phần tử trong cột thứ <code>i</code> (đánh số từ 0) là <code>colsum[i]</code>, trong đó <code>colsum</code> là mảng số nguyên có độ dài <code>n</code>.</li>
</ul>

<p>Nhiệm vụ của bạn là khôi phục ma trận dựa trên <code>upper</code>, <code>lower</code> và <code>colsum</code>.</p>

<p>Trả về ma trận dưới dạng mảng số nguyên 2 chiều.</p>

<p>Nếu có nhiều đáp án hợp lệ, bạn có thể trả về bất kỳ đáp án nào.</p>

<p>Nếu không tồn tại đáp án hợp lệ, hãy trả về mảng 2 chiều rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> upper = 2, lower = 1, colsum = [1,1,1]
<strong>Đầu ra:</strong> [[1,1,0],[0,0,1]]
<strong>Giải thích: </strong>[[1,0,1],[0,1,0]], và [[0,1,1],[1,0,0]] cũng là đáp án hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> upper = 2, lower = 3, colsum = [2,2,1,1]
<strong>Đầu ra:</strong> []
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> upper = 5, lower = 5, colsum = [2,1,2,0,1,0,1,2,0,1]
<strong>Đầu ra:</strong> [[1,1,1,0,1,0,0,1,0,0],[1,0,1,0,0,0,1,1,0,1]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= colsum.length &lt;= 10^5</code></li>
	<li><code>0 &lt;= upper, lower &lt;= colsum.length</code></li>
	<li><code>0 &lt;= colsum[i] &lt;= 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Tổng từng cột đã biết, còn hai hàng phải có lần lượt $upper$ và $lower$ số $1$. Vì $n \le 10^5$, không thể dùng backtracking. Nếu tổng cột là $2$, cả hai ô đều phải là $1$; nếu là $0$, cả hai ô phải là $0$; nếu là $1$, ta gán số $1$ cho hàng còn quota lớn hơn, nếu không hàng kia có thể không đủ quota về sau.
>
> Ta điền ma trận từ trái sang phải; nếu phần quota còn lại âm hoặc vẫn còn quota sau khi duyệt hết thì không có đáp án. Gán số $1$ cho hàng có quota ít hơn không làm phát sinh xung đột nào khác.

<!-- thinking:end -->

Trước tiên, ta tạo mảng kết quả $ans$, trong đó $ans[0]$ và $ans[1]$ lần lượt biểu diễn hàng thứ nhất và hàng thứ hai của ma trận.

Tiếp theo, ta duyệt mảng $colsum$ từ trái sang phải. Với phần tử hiện tại $colsum[j]$, có các trường hợp sau:

- Nếu $colsum[j] = 2$, ta đặt cả $ans[0][j]$ và $ans[1][j]$ bằng $1$. Khi đó, giảm $upper$ và $lower$ đi $1$.
- Nếu $colsum[j] = 1$, ta đặt một trong hai ô $ans[0][j]$ hoặc $ans[1][j]$ bằng $1$. Nếu $upper \gt lower$, ta ưu tiên đặt $ans[0][j]$ bằng $1$; ngược lại, ưu tiên đặt $ans[1][j]$ bằng $1$. Khi đó, giảm một trong hai giá trị $upper$ hoặc $lower$ đi $1$.
- Nếu $colsum[j] = 0$, ta đặt cả $ans[0][j]$ và $ans[1][j]$ bằng $0$.
- Nếu $upper \lt 0$ hoặc $lower \lt 0$, không thể tạo ma trận thỏa mãn yêu cầu, nên ta trả về mảng rỗng.

Sau khi duyệt xong, nếu cả $upper$ và $lower$ đều bằng $0$, ta trả về $ans$; nếu không thì trả về mảng rỗng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $colsum$. Nếu không tính không gian dùng cho mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reconstructMatrix(
        self, upper: int, lower: int, colsum: List[int]
    ) -> List[List[int]]:
        n = len(colsum)
        ans = [[0] * n for _ in range(2)]
        for j, v in enumerate(colsum):
            if v == 2:
                ans[0][j] = ans[1][j] = 1
                upper, lower = upper - 1, lower - 1
            if v == 1:
                if upper > lower:
                    upper -= 1
                    ans[0][j] = 1
                else:
                    lower -= 1
                    ans[1][j] = 1
            if upper < 0 or lower < 0:
                return []
        return ans if lower == upper == 0 else []
```

#### Java

```java
class Solution {
    public List<List<Integer>> reconstructMatrix(int upper, int lower, int[] colsum) {
        int n = colsum.length;
        List<Integer> first = new ArrayList<>();
        List<Integer> second = new ArrayList<>();
        for (int j = 0; j < n; ++j) {
            int a = 0, b = 0;
            if (colsum[j] == 2) {
                a = b = 1;
                upper--;
                lower--;
            } else if (colsum[j] == 1) {
                if (upper > lower) {
                    upper--;
                    a = 1;
                } else {
                    lower--;
                    b = 1;
                }
            }
            if (upper < 0 || lower < 0) {
                break;
            }
            first.add(a);
            second.add(b);
        }
        return upper == 0 && lower == 0 ? List.of(first, second) : List.of();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> reconstructMatrix(int upper, int lower, vector<int>& colsum) {
        int n = colsum.size();
        vector<vector<int>> ans(2, vector<int>(n));
        for (int j = 0; j < n; ++j) {
            if (colsum[j] == 2) {
                ans[0][j] = ans[1][j] = 1;
                upper--;
                lower--;
            }
            if (colsum[j] == 1) {
                if (upper > lower) {
                    upper--;
                    ans[0][j] = 1;
                } else {
                    lower--;
                    ans[1][j] = 1;
                }
            }
            if (upper < 0 || lower < 0) {
                break;
            }
        }
        return upper || lower ? vector<vector<int>>() : ans;
    }
};
```

#### Go

```go
func reconstructMatrix(upper int, lower int, colsum []int) [][]int {
	n := len(colsum)
	ans := make([][]int, 2)
	for i := range ans {
		ans[i] = make([]int, n)
	}
	for j, v := range colsum {
		if v == 2 {
			ans[0][j], ans[1][j] = 1, 1
			upper--
			lower--
		}
		if v == 1 {
			if upper > lower {
				upper--
				ans[0][j] = 1
			} else {
				lower--
				ans[1][j] = 1
			}
		}
		if upper < 0 || lower < 0 {
			break
		}
	}
	if upper != 0 || lower != 0 {
		return [][]int{}
	}
	return ans
}
```

#### TypeScript

```ts
function reconstructMatrix(upper: number, lower: number, colsum: number[]): number[][] {
    const n = colsum.length;
    const ans: number[][] = Array(2)
        .fill(0)
        .map(() => Array(n).fill(0));
    for (let j = 0; j < n; ++j) {
        if (colsum[j] === 2) {
            ans[0][j] = ans[1][j] = 1;
            upper--;
            lower--;
        } else if (colsum[j] === 1) {
            if (upper > lower) {
                ans[0][j] = 1;
                upper--;
            } else {
                ans[1][j] = 1;
                lower--;
            }
        }
        if (upper < 0 || lower < 0) {
            break;
        }
    }
    return upper || lower ? [] : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
