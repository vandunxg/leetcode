---
comments: true
difficulty: Medium
rating: 1797
source: Weekly Contest 139 Q2
tags:
    - Array
    - Hash Table
    - Matrix
---

<!-- problem:start -->

# [1072. Flip Columns For Maximum Number of Equal Rows](https://leetcode.com/problems/flip-columns-for-maximum-number-of-equal-rows)

[中文文档](/solution/1000-1099/1072.Flip%20Columns%20For%20Maximum%20Number%20of%20Equal%20Rows/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận nhị phân <code>m x n</code> <code>matrix</code>.</p>

<p>Bạn có thể chọn tùy ý số cột trong ma trận và lật mọi ô trong các cột đó (tức đổi giá trị mỗi ô từ <code>0</code> thành <code>1</code> hoặc ngược lại).</p>

<p>Trả về số hàng lớn nhất có mọi giá trị bằng nhau sau khi thực hiện một số lần lật.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix = [[0,1],[1,1]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Không cần lật giá trị nào, có 1 hàng gồm các giá trị bằng nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix = [[0,1],[1,0]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Sau khi lật các giá trị ở cột đầu tiên, cả hai hàng đều có các giá trị bằng nhau.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix = [[0,0,0],[0,0,1],[1,1,0]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Sau khi lật các giá trị ở hai cột đầu tiên, hai hàng cuối có các giá trị bằng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == matrix.length</code></li>
	<li><code>n == matrix[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 300</code></li>
	<li><code>matrix[i][j]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi lật một số cột, các hàng giống nhau sẽ cùng toàn số 0 hoặc cùng toàn số 1. Hai hàng có thể trở nên giống nhau khi và chỉ khi chúng bằng nhau hoặc là phần bù bit của nhau. Vì $m,n\le 300$, không thể thử tất cả $2^n$ mask lật.
>
> Chuẩn hóa từng hàng để bit đầu tiên bằng $0$ (nếu hàng bắt đầu bằng $1$ thì lật toàn bộ hàng). Khi đó, hai hàng bù nhau sẽ có cùng một key.
>
> Đếm tần suất của các tuple đã chuẩn hóa; tần suất lớn nhất là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxEqualRowsAfterFlips(self, matrix: List[List[int]]) -> int:
        cnt = Counter()
        for row in matrix:
            t = tuple(row) if row[0] == 0 else tuple(x ^ 1 for x in row)
            cnt[t] += 1
        return max(cnt.values())
```

#### Java

```java
class Solution {
    public int maxEqualRowsAfterFlips(int[][] matrix) {
        Map<String, Integer> cnt = new HashMap<>();
        int ans = 0, n = matrix[0].length;
        for (var row : matrix) {
            char[] cs = new char[n];
            for (int i = 0; i < n; ++i) {
                cs[i] = (char) (row[0] ^ row[i]);
            }
            ans = Math.max(ans, cnt.merge(String.valueOf(cs), 1, Integer::sum));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxEqualRowsAfterFlips(vector<vector<int>>& matrix) {
        unordered_map<string, int> cnt;
        int ans = 0;
        for (auto& row : matrix) {
            string s;
            for (int x : row) {
                s.push_back('0' + (row[0] == 0 ? x : x ^ 1));
            }
            ans = max(ans, ++cnt[s]);
        }
        return ans;
    }
};
```

#### Go

```go
func maxEqualRowsAfterFlips(matrix [][]int) (ans int) {
	cnt := map[string]int{}
	for _, row := range matrix {
		s := []byte{}
		for _, x := range row {
			if row[0] == 1 {
				x ^= 1
			}
			s = append(s, byte(x)+'0')
		}
		t := string(s)
		cnt[t]++
		ans = max(ans, cnt[t])
	}
	return
}
```

#### TypeScript

```ts
function maxEqualRowsAfterFlips(matrix: number[][]): number {
    const cnt = new Map<string, number>();
    let ans = 0;
    for (const row of matrix) {
        if (row[0] === 1) {
            for (let i = 0; i < row.length; i++) {
                row[i] ^= 1;
            }
        }
        const s = row.join('');
        cnt.set(s, (cnt.get(s) || 0) + 1);
        ans = Math.max(ans, cnt.get(s)!);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
