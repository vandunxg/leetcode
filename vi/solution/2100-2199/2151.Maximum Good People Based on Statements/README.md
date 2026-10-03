---
comments: true
difficulty: Hard
rating: 1979
source: Weekly Contest 277 Q4
tags:
    - Bit Manipulation
    - Array
    - Backtracking
    - Enumeration
---

<!-- problem:start -->

# [2151. Maximum Good People Based on Statements](https://leetcode.com/problems/maximum-good-people-based-on-statements)

[中文文档](/solution/2100-2199/2151.Maximum%20Good%20People%20Based%20on%20Statements/README.md)

## Mô tả

<!-- description:start -->

<p>Có hai loại người:</p>

<ul>
	<li><strong>Người tốt</strong>: Người luôn nói thật.</li>
	<li><strong>Người xấu</strong>: Người có thể nói thật hoặc nói dối.</li>
</ul>

<p>Bạn được cho một mảng số nguyên 2 chiều <strong>đánh chỉ số từ 0</strong> <code>statements</code> có kích thước <code>n x n</code>, biểu diễn các phát biểu của <code>n</code> người về nhau. Cụ thể hơn, <code>statements[i][j]</code> có thể là một trong các giá trị sau:</p>

<ul>
	<li><code>0</code>, biểu thị phát biểu của người <code>i</code> rằng người <code>j</code> là <strong>người xấu</strong>.</li>
	<li><code>1</code>, biểu thị phát biểu của người <code>i</code> rằng người <code>j</code> là <strong>người tốt</strong>.</li>
	<li><code>2</code>, biểu thị rằng người <code>i</code> <strong>không đưa ra phát biểu nào</strong> về người <code>j</code>.</li>
</ul>

<p>Ngoài ra, không người nào đưa ra phát biểu về chính mình. Cụ thể, <code>statements[i][i] = 2</code> với mọi <code>0 &lt;= i &lt; n</code>.</p>

<p>Hãy trả về <em><strong>số người tối đa</strong> có thể là <strong>người tốt</strong> dựa trên các phát biểu của </em><code>n</code><em> người</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2151.Maximum%20Good%20People%20Based%20on%20Statements/images/logic1.jpg" style="width: 600px; height: 262px;" />
<pre>
<strong>Đầu vào:</strong> statements = [[2,1,2],[1,2,2],[2,0,2]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Mỗi người đưa ra một phát biểu.
- Người 0 nói rằng người 1 là người tốt.
- Người 1 nói rằng người 0 là người tốt.
- Người 2 nói rằng người 1 là người xấu.
Ta chọn người 2 làm mấu chốt.
- Giả sử người 2 là người tốt:
    - Dựa trên phát biểu của người 2, người 1 là người xấu.
    - Bây giờ ta biết chắc rằng người 1 là người xấu và người 2 là người tốt.
    - Dựa trên phát biểu của người 1, vì người 1 là người xấu nên người này có thể:
        - nói thật. Trong trường hợp này sẽ xảy ra mâu thuẫn và giả định trên không đúng.
        - nói dối. Trong trường hợp này, người 0 cũng là người xấu và đã nói dối trong phát biểu của mình.
    - <strong>Nếu người 2 là người tốt thì nhóm chỉ có một người tốt</strong>.
- Giả sử người 2 là người xấu:
    - Dựa trên phát biểu của người 2, vì người 2 là người xấu nên người này có thể:
        - nói thật. Theo kịch bản này, cả người 0 và người 1 đều là người xấu như đã giải thích ở trên.
            - <strong>Nếu người 2 là người xấu nhưng nói thật thì nhóm không có người tốt nào</strong>.
        - nói dối. Trong trường hợp này, người 1 là người tốt.
            - Vì người 1 là người tốt nên người 0 cũng là người tốt.
            - <strong>Nếu người 2 là người xấu và nói dối thì nhóm có hai người tốt</strong>.
Ta thấy trong trường hợp tốt nhất có nhiều nhất 2 người tốt, nên trả về 2.
Lưu ý rằng có nhiều cách để đi đến kết luận này.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2151.Maximum%20Good%20People%20Based%20on%20Statements/images/logic2.jpg" style="width: 600px; height: 262px;" />
<pre>
<strong>Đầu vào:</strong> statements = [[2,0],[0,2]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Mỗi người đưa ra một phát biểu.
- Người 0 nói rằng người 1 là người xấu.
- Người 1 nói rằng người 0 là người xấu.
Ta chọn người 0 làm mấu chốt.
- Giả sử người 0 là người tốt:
    - Dựa trên phát biểu của người 0, người 1 là người xấu và đã nói dối.
    - <strong>Nếu người 0 là người tốt thì nhóm chỉ có một người tốt</strong>.
- Giả sử người 0 là người xấu:
    - Dựa trên phát biểu của người 0, vì người 0 là người xấu nên người này có thể:
        - nói thật. Theo kịch bản này, cả người 0 và người 1 đều là người xấu.
            - <strong>Nếu người 0 là người xấu nhưng nói thật thì nhóm không có người tốt nào</strong>.
        - nói dối. Trong trường hợp này, người 1 là người tốt.
            - <strong>Nếu người 0 là người xấu và nói dối thì nhóm chỉ có một người tốt</strong>.
Ta thấy trong trường hợp tốt nhất có nhiều nhất một người tốt, nên trả về 1.
Lưu ý rằng có nhiều cách để đi đến kết luận này.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == statements.length == statements[i].length</code></li>
	<li><code>2 &lt;= n &lt;= 15</code></li>
	<li><code>statements[i][j]</code> chỉ có thể là <code>0</code>, <code>1</code> hoặc <code>2</code>.</li>
	<li><code>statements[i][i] == 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi người đều là người tốt hoặc người xấu; các phát biểu của người tốt phải khớp với giả thuyết, còn người xấu có thể nói đúng hoặc không. Với $n\le 15$, ta có thể liệt kê $2^n$ tập con gồm những người tốt.
>
> Với mỗi bit được bật trong một mask, kiểm tra xem mọi phát biểu $0/1$ về những người khác có khớp với mask hay không; nếu có mâu thuẫn thì loại mask đó, nếu không thì số bit 1 của nó là một ứng viên.
>
> Lấy giá trị popcount lớn nhất trong tất cả các mask.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumGood(self, statements: List[List[int]]) -> int:
        def check(mask: int) -> int:
            cnt = 0
            for i, row in enumerate(statements):
                if mask >> i & 1:
                    for j, x in enumerate(row):
                        if x < 2 and (mask >> j & 1) != x:
                            return 0
                    cnt += 1
            return cnt

        return max(check(i) for i in range(1, 1 << len(statements)))
```

#### Java

```java
class Solution {
    public int maximumGood(int[][] statements) {
        int ans = 0;
        for (int mask = 1; mask < 1 << statements.length; ++mask) {
            ans = Math.max(ans, check(mask, statements));
        }
        return ans;
    }

    private int check(int mask, int[][] statements) {
        int cnt = 0;
        int n = statements.length;
        for (int i = 0; i < n; ++i) {
            if (((mask >> i) & 1) == 1) {
                for (int j = 0; j < n; ++j) {
                    int v = statements[i][j];
                    if (v < 2 && ((mask >> j) & 1) != v) {
                        return 0;
                    }
                }
                ++cnt;
            }
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumGood(vector<vector<int>>& statements) {
        int ans = 0;
        for (int mask = 1; mask < 1 << statements.size(); ++mask) ans = max(ans, check(mask, statements));
        return ans;
    }

    int check(int mask, vector<vector<int>>& statements) {
        int cnt = 0;
        int n = statements.size();
        for (int i = 0; i < n; ++i) {
            if ((mask >> i) & 1) {
                for (int j = 0; j < n; ++j) {
                    int v = statements[i][j];
                    if (v < 2 && ((mask >> j) & 1) != v) return 0;
                }
                ++cnt;
            }
        }
        return cnt;
    }
};
```

#### Go

```go
func maximumGood(statements [][]int) int {
	n := len(statements)
	check := func(mask int) int {
		cnt := 0
		for i, s := range statements {
			if ((mask >> i) & 1) == 1 {
				for j, v := range s {
					if v < 2 && ((mask>>j)&1) != v {
						return 0
					}
				}
				cnt++
			}
		}
		return cnt
	}
	ans := 0
	for mask := 1; mask < 1<<n; mask++ {
		ans = max(ans, check(mask))
	}
	return ans
}
```

#### TypeScript

```ts
function maximumGood(statements: number[][]): number {
    const n = statements.length;
    function check(mask) {
        let cnt = 0;
        for (let i = 0; i < n; ++i) {
            if ((mask >> i) & 1) {
                for (let j = 0; j < n; ++j) {
                    const v = statements[i][j];
                    if (v < 2 && ((mask >> j) & 1) != v) {
                        return 0;
                    }
                }
                ++cnt;
            }
        }
        return cnt;
    }
    let ans = 0;
    for (let mask = 1; mask < 1 << n; ++mask) {
        ans = Math.max(ans, check(mask));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
