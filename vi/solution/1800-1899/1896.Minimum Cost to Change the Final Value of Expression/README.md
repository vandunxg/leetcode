---
comments: true
difficulty: Hard
rating: 2531
source: Biweekly Contest 54 Q4
tags:
    - Stack
    - Math
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [1896. Minimum Cost to Change the Final Value of Expression](https://leetcode.com/problems/minimum-cost-to-change-the-final-value-of-expression)

[中文文档](/solution/1800-1899/1896.Minimum%20Cost%20to%20Change%20the%20Final%20Value%20of%20Expression/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một biểu thức Boolean <strong>hợp lệ</strong> dưới dạng chuỗi <code>expression</code>, chỉ gồm các ký tự <code>&#39;1&#39;</code>,<code>&#39;0&#39;</code>,<code>&#39;&amp;&#39;</code> (phép <strong>AND</strong> theo bit),<code>&#39;|&#39;</code> (phép <strong>OR</strong> theo bit),<code>&#39;(&#39;</code> và <code>&#39;)&#39;</code>.</p>

<ul>
	<li>Ví dụ, <code>&quot;()1|1&quot;</code> và <code>&quot;(1)&amp;()&quot;</code> là các biểu thức <strong>không hợp lệ</strong>, còn <code>&quot;1&quot;</code>, <code>&quot;(((1))|(0))&quot;</code> và <code>&quot;1|(0&amp;(1))&quot;</code> là các biểu thức <strong>hợp lệ</strong>.</li>
</ul>

<p>Hãy trả về <em><strong>chi phí nhỏ nhất</strong> để thay đổi giá trị cuối cùng của biểu thức</em>.</p>

<ul>
	<li>Ví dụ, nếu <code>expression = &quot;1|1|(0&amp;0)&amp;1&quot;</code>, <strong>giá trị</strong> của nó là <code>1|1|(0&amp;0)&amp;1 = 1|1|0&amp;1 = 1|0&amp;1 = 1&amp;1 = 1</code>. Ta muốn thực hiện các thao tác để biểu thức <strong>mới</strong> có giá trị bằng <code>0</code>.</li>
</ul>

<p><strong>Chi phí</strong> thay đổi giá trị cuối cùng của biểu thức là <strong>số thao tác</strong> được thực hiện trên biểu thức. Các loại <strong>thao tác</strong> gồm:</p>

<ul>
	<li>Đổi <code>&#39;1&#39;</code> thành <code>&#39;0&#39;</code>.</li>
	<li>Đổi <code>&#39;0&#39;</code> thành <code>&#39;1&#39;</code>.</li>
	<li>Đổi <code>&#39;&amp;&#39;</code> thành <code>&#39;|&#39;</code>.</li>
	<li>Đổi <code>&#39;|&#39;</code> thành <code>&#39;&amp;&#39;</code>.</li>
</ul>

<p><strong>Lưu ý:</strong> <code>&#39;&amp;&#39;</code> <strong>không</strong> được ưu tiên hơn <code>&#39;|&#39;</code> trong <strong>thứ tự tính toán</strong>. Hãy tính biểu thức trong ngoặc <strong>trước</strong>, sau đó tính theo thứ tự <strong>từ trái sang phải</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;1&amp;(0|1)&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể đổi &quot;1&amp;(0<u><strong>|</strong></u>1)&quot; thành &quot;1&amp;(0<u><strong>&amp;</strong></u>1)&quot; bằng cách đổi &#39;|&#39; thành &#39;&amp;&#39; với 1 thao tác.
Biểu thức mới có giá trị bằng 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;(0&amp;0)&amp;(0&amp;0&amp;0)&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể đổi &quot;(0<u><strong>&amp;0</strong></u>)<strong><u>&amp;</u></strong>(0&amp;0&amp;0)&quot; thành &quot;(0<u><strong>|1</strong></u>)<u><strong>|</strong></u>(0&amp;0&amp;0)&quot; bằng 3 thao tác.
Biểu thức mới có giá trị bằng 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;(0|(1|0&amp;1))&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể đổi &quot;(0|(<u><strong>1</strong></u>|0&amp;1))&quot; thành &quot;(0|(<u><strong>0</strong></u>|0&amp;1))&quot; bằng 1 thao tác.
Biểu thức mới có giá trị bằng 0.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= expression.length &lt;= 10<sup>5</sup></code></li>
	<li><code>expression</code>&nbsp;chỉ chứa&nbsp;<code>&#39;1&#39;</code>,<code>&#39;0&#39;</code>,<code>&#39;&amp;&#39;</code>,<code>&#39;|&#39;</code>,<code>&#39;(&#39;</code> và&nbsp;<code>&#39;)&#39;</code></li>
	<li>Tất cả các dấu ngoặc đều được ghép đúng.</li>
	<li>Không có cặp ngoặc rỗng (tức là&nbsp;<code>&quot;()&quot;</code>&nbsp;không phải là một chuỗi con của&nbsp;<code>expression</code>).</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Biểu thức Boolean hợp lệ sử dụng $0/1$, $\&$, $|$ và dấu ngoặc. Một lần chỉnh sửa có thể đảo một chữ số hoặc một toán tử. Biểu thức có thể dài tới $10^5$, nên không thể tính lại sau từng lần chỉnh sửa.
>
> Mỗi biểu thức con chỉ cần lưu giá trị hiện tại và chi phí để đảo giá trị đó. Một lá có chi phí đảo là $1$. Khi ghép hai vế bằng $\&$ hoặc $|$, chi phí đảo được tính từ việc đổi toán tử, đảo một vế hoặc đảo cả hai vế. Hai stack dùng để phân tích dấu ngoặc và toán tử, rồi lần lượt gộp các cặp từ dưới lên.

<!-- thinking:end -->

Biểu diễn mỗi biểu thức con bằng $(\textit{val},\textit{cost})$: giá trị Boolean hiện tại và số lần chỉnh sửa nhỏ nhất để đảo giá trị đó. Một chữ số có chi phí đảo là $1$.

$\&$ và $|$ có cùng độ ưu tiên và được kết hợp từ trái sang phải; dấu ngoặc có độ ưu tiên cao hơn. Hai stack lưu các biểu thức con và toán tử. Một toán tử sẽ rút gọn mọi toán tử đang chờ có cùng độ ưu tiên, còn dấu ngoặc đóng sẽ rút gọn cho đến dấu ngoặc mở tương ứng.

Gọi hai vế là $(v_1,c_1)$ và $(v_2,c_2)$.

- Với $\&$ và cả hai vế đều bằng $1$, giá trị là $1$ và chi phí đảo là $\min(c_1,c_2)$.
- Với cả hai vế đều bằng $0$, giá trị là $0$. Đảo cả hai toán hạng có chi phí $c_1+c_2$; đổi $\&$ thành $|$ rồi đảo một toán hạng có chi phí $1+\min(c_1,c_2)$.
- Với đúng một vế bằng $0$, giá trị là $0$. Ta có thể đảo số $0$ đó hoặc đổi $\&$ thành $|$, rồi chọn chi phí nhỏ hơn.
- Ba trường hợp của $|$ đối ngẫu với ba trường hợp trên.

Sau khi rút gọn biểu thức, $\textit{cost}$ ở đỉnh stack là đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài biểu thức.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperationsToFlip(self, expression: str) -> int:
        def merge(a, b, op):
            v1, c1 = a
            v2, c2 = b
            if op == '&':
                val = v1 & v2
                if v1 == 1 and v2 == 1:
                    cost = min(c1, c2)
                elif v1 == 0 and v2 == 0:
                    cost = min(c1 + c2, 1 + min(c1, c2))
                else:
                    cost = min(c1 if v1 == 0 else c2, 1)
            else:
                val = v1 | v2
                if v1 == 0 and v2 == 0:
                    cost = min(c1, c2)
                elif v1 == 1 and v2 == 1:
                    cost = min(c1 + c2, 1 + min(c1, c2))
                else:
                    cost = min(c1 if v1 == 1 else c2, 1)
            return val, cost

        nums = []
        ops = []

        def apply():
            b = nums.pop()
            a = nums.pop()
            nums.append(merge(a, b, ops.pop()))

        for c in expression:
            if c == '(':
                ops.append(c)
            elif c in '01':
                nums.append((int(c), 1))
            elif c in '&|':
                while ops and ops[-1] in '&|':
                    apply()
                ops.append(c)
            else:
                while ops[-1] != '(':
                    apply()
                ops.pop()
        while ops:
            apply()
        return nums[-1][1]
```

#### Java

```java
class Solution {
    public int minOperationsToFlip(String expression) {
        int n = expression.length();
        int[] val = new int[n];
        int[] cost = new int[n];
        int top = 0;
        char[] ops = new char[n];
        int otop = 0;
        for (int i = 0; i < n; ++i) {
            char c = expression.charAt(i);
            if (c == '(') {
                ops[otop++] = c;
            } else if (c == '0' || c == '1') {
                val[top] = c - '0';
                cost[top++] = 1;
            } else if (c == '&' || c == '|') {
                while (otop > 0 && (ops[otop - 1] == '&' || ops[otop - 1] == '|')) {
                    merge(val, cost, top, ops[--otop]);
                    --top;
                }
                ops[otop++] = c;
            } else {
                while (ops[otop - 1] != '(') {
                    merge(val, cost, top, ops[--otop]);
                    --top;
                }
                --otop;
            }
        }
        while (otop > 0) {
            merge(val, cost, top, ops[--otop]);
            --top;
        }
        return cost[0];
    }

    private void merge(int[] val, int[] cost, int top, char op) {
        int v1 = val[top - 2], c1 = cost[top - 2];
        int v2 = val[top - 1], c2 = cost[top - 1];
        if (op == '&') {
            val[top - 2] = v1 & v2;
            if (v1 == 1 && v2 == 1) {
                cost[top - 2] = Math.min(c1, c2);
            } else if (v1 == 0 && v2 == 0) {
                cost[top - 2] = Math.min(c1 + c2, 1 + Math.min(c1, c2));
            } else {
                cost[top - 2] = Math.min(v1 == 0 ? c1 : c2, 1);
            }
        } else {
            val[top - 2] = v1 | v2;
            if (v1 == 0 && v2 == 0) {
                cost[top - 2] = Math.min(c1, c2);
            } else if (v1 == 1 && v2 == 1) {
                cost[top - 2] = Math.min(c1 + c2, 1 + Math.min(c1, c2));
            } else {
                cost[top - 2] = Math.min(v1 == 1 ? c1 : c2, 1);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperationsToFlip(string expression) {
        vector<pair<int, int>> nums;
        vector<char> ops;
        auto merge = [&](char op) {
            auto b = nums.back();
            nums.pop_back();
            auto a = nums.back();
            nums.pop_back();
            int v1 = a.first, c1 = a.second, v2 = b.first, c2 = b.second;
            int val, cost;
            if (op == '&') {
                val = v1 & v2;
                if (v1 == 1 && v2 == 1) {
                    cost = min(c1, c2);
                } else if (v1 == 0 && v2 == 0) {
                    cost = min(c1 + c2, 1 + min(c1, c2));
                } else {
                    cost = min(v1 == 0 ? c1 : c2, 1);
                }
            } else {
                val = v1 | v2;
                if (v1 == 0 && v2 == 0) {
                    cost = min(c1, c2);
                } else if (v1 == 1 && v2 == 1) {
                    cost = min(c1 + c2, 1 + min(c1, c2));
                } else {
                    cost = min(v1 == 1 ? c1 : c2, 1);
                }
            }
            nums.emplace_back(val, cost);
        };
        for (char c : expression) {
            if (c == '(') {
                ops.push_back(c);
            } else if (c == '0' || c == '1') {
                nums.emplace_back(c - '0', 1);
            } else if (c == '&' || c == '|') {
                while (!ops.empty() && (ops.back() == '&' || ops.back() == '|')) {
                    merge(ops.back());
                    ops.pop_back();
                }
                ops.push_back(c);
            } else {
                while (ops.back() != '(') {
                    merge(ops.back());
                    ops.pop_back();
                }
                ops.pop_back();
            }
        }
        while (!ops.empty()) {
            merge(ops.back());
            ops.pop_back();
        }
        return nums[0].second;
    }
};
```

#### Go

```go
func minOperationsToFlip(expression string) int {
	type pair struct{ val, cost int }
	nums := make([]pair, 0)
	ops := make([]byte, 0)
	merge := func(op byte) {
		b := nums[len(nums)-1]
		a := nums[len(nums)-2]
		nums = nums[:len(nums)-2]
		v1, c1, v2, c2 := a.val, a.cost, b.val, b.cost
		val, cost := 0, 0
		if op == '&' {
			val = v1 & v2
			if v1 == 1 && v2 == 1 {
				cost = min(c1, c2)
			} else if v1 == 0 && v2 == 0 {
				cost = min(c1+c2, 1+min(c1, c2))
			} else if v1 == 0 {
				cost = min(c1, 1)
			} else {
				cost = min(c2, 1)
			}
		} else {
			val = v1 | v2
			if v1 == 0 && v2 == 0 {
				cost = min(c1, c2)
			} else if v1 == 1 && v2 == 1 {
				cost = min(c1+c2, 1+min(c1, c2))
			} else if v1 == 1 {
				cost = min(c1, 1)
			} else {
				cost = min(c2, 1)
			}
		}
		nums = append(nums, pair{val, cost})
	}
	for i := 0; i < len(expression); i++ {
		c := expression[i]
		if c == '(' {
			ops = append(ops, c)
		} else if c == '0' || c == '1' {
			nums = append(nums, pair{int(c - '0'), 1})
		} else if c == '&' || c == '|' {
			for len(ops) > 0 && (ops[len(ops)-1] == '&' || ops[len(ops)-1] == '|') {
				merge(ops[len(ops)-1])
				ops = ops[:len(ops)-1]
			}
			ops = append(ops, c)
		} else {
			for ops[len(ops)-1] != '(' {
				merge(ops[len(ops)-1])
				ops = ops[:len(ops)-1]
			}
			ops = ops[:len(ops)-1]
		}
	}
	for len(ops) > 0 {
		merge(ops[len(ops)-1])
		ops = ops[:len(ops)-1]
	}
	return nums[0].cost
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
