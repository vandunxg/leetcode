---
comments: true
difficulty: Medium
tags:
    - Recursion
    - Memoization
    - Math
    - String
    - Dynamic Programming
    - Parentheses
---

<!-- problem:start -->

# [241. Different Ways to Add Parentheses](https://leetcode.com/problems/different-ways-to-add-parentheses)

[中文文档](/solution/0200-0299/0241.Different%20Ways%20to%20Add%20Parentheses/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>expression</code> gồm các số và toán tử, hãy trả về <em>tất cả các kết quả có thể nhận được khi tính theo mọi cách nhóm các số và toán tử khác nhau</em>. Bạn có thể trả lời theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Các test case được tạo sao cho các giá trị đầu ra nằm trong phạm vi số nguyên 32-bit và số lượng kết quả khác nhau không vượt quá <code>10<sup>4</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;2-1-1&quot;
<strong>Đầu ra:</strong> [0,2]
<strong>Giải thích:</strong>
((2-1)-1) = 0
(2-(1-1)) = 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;2*3-4*5&quot;
<strong>Đầu ra:</strong> [-34,-14,-10,-10,10]
<strong>Giải thích:</strong>
(2*(3-(4*5))) = -34
((2*3)-(4*5)) = -14
((2*(3-4))*5) = -10
(2*((3-4)*5)) = -10
(((2*3)-4)*5) = 10
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= expression.length &lt;= 20</code></li>
	<li><code>expression</code> chỉ gồm các chữ số và các toán tử <code>&#39;+&#39;</code>, <code>&#39;-&#39;</code>, và <code>&#39;*&#39;</code>.</li>
	<li>Tất cả giá trị số nguyên trong biểu thức đầu vào nằm trong phạm vi <code>[0, 99]</code>.</li>
	<li>Các giá trị số nguyên trong biểu thức đầu vào không có <code>&#39;-&#39;</code> hoặc <code>&#39;+&#39;</code> ở đầu để biểu thị dấu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi cách đặt dấu ngoặc sẽ tách biểu thức tại một toán tử nào đó. Nếu tính trực tiếp mọi cách tách, ta sẽ tính lại cùng một biểu thức con nhiều lần.
>
> Ta tách tại từng toán tử, đệ quy để lấy mọi giá trị ở bên trái và bên phải, kết hợp chúng, rồi memoize kết quả của mỗi chuỗi con.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def diffWaysToCompute(self, expression: str) -> List[int]:
        @cache
        def dfs(exp):
            if exp.isdigit():
                return [int(exp)]
            ans = []
            for i, c in enumerate(exp):
                if c in '-+*':
                    left, right = dfs(exp[:i]), dfs(exp[i + 1 :])
                    for a in left:
                        for b in right:
                            if c == '-':
                                ans.append(a - b)
                            elif c == '+':
                                ans.append(a + b)
                            else:
                                ans.append(a * b)
            return ans

        return dfs(expression)
```

#### Java

```java
class Solution {
    private static Map<String, List<Integer>> memo = new HashMap<>();

    public List<Integer> diffWaysToCompute(String expression) {
        return dfs(expression);
    }

    private List<Integer> dfs(String exp) {
        if (memo.containsKey(exp)) {
            return memo.get(exp);
        }
        List<Integer> ans = new ArrayList<>();
        if (exp.length() < 3) {
            ans.add(Integer.parseInt(exp));
            return ans;
        }
        for (int i = 0; i < exp.length(); ++i) {
            char c = exp.charAt(i);
            if (c == '-' || c == '+' || c == '*') {
                List<Integer> left = dfs(exp.substring(0, i));
                List<Integer> right = dfs(exp.substring(i + 1));
                for (int a : left) {
                    for (int b : right) {
                        if (c == '-') {
                            ans.add(a - b);
                        } else if (c == '+') {
                            ans.add(a + b);
                        } else {
                            ans.add(a * b);
                        }
                    }
                }
            }
        }
        memo.put(exp, ans);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> diffWaysToCompute(string expression) {
        return dfs(expression);
    }

    vector<int> dfs(string exp) {
        if (memo.count(exp)) return memo[exp];
        if (exp.size() < 3) return {stoi(exp)};
        vector<int> ans;
        int n = exp.size();
        for (int i = 0; i < n; ++i) {
            char c = exp[i];
            if (c == '-' || c == '+' || c == '*') {
                vector<int> left = dfs(exp.substr(0, i));
                vector<int> right = dfs(exp.substr(i + 1, n - i - 1));
                for (int& a : left) {
                    for (int& b : right) {
                        if (c == '-')
                            ans.push_back(a - b);
                        else if (c == '+')
                            ans.push_back(a + b);
                        else
                            ans.push_back(a * b);
                    }
                }
            }
        }
        memo[exp] = ans;
        return ans;
    }

private:
    unordered_map<string, vector<int>> memo;
};
```

#### Go

```go
var memo = map[string][]int{}

func diffWaysToCompute(expression string) []int {
	return dfs(expression)
}

func dfs(exp string) []int {
	if v, ok := memo[exp]; ok {
		return v
	}
	if len(exp) < 3 {
		v, _ := strconv.Atoi(exp)
		return []int{v}
	}
	ans := []int{}
	for i, c := range exp {
		if c == '-' || c == '+' || c == '*' {
			left, right := dfs(exp[:i]), dfs(exp[i+1:])
			for _, a := range left {
				for _, b := range right {
					if c == '-' {
						ans = append(ans, a-b)
					} else if c == '+' {
						ans = append(ans, a+b)
					} else {
						ans = append(ans, a*b)
					}
				}
			}
		}
	}
	memo[exp] = ans
	return ans
}
```

#### C#

```cs
public class Solution {
    public IList<int> DiffWaysToCompute(string input) {
        var values = new List<int>();
        var operators = new List<char>();
        var sum = 0;
        foreach (var ch in input) {
            if (ch == '+' || ch == '-' || ch == '*') {
                values.Add(sum);
                operators.Add(ch);
                sum = 0;
            }
            else {
                sum = sum * 10 + ch - '0';
            }
        }
        values.Add(sum);

        var f = new List<int>[values.Count, values.Count];
        for (var i = 0; i < values.Count; ++i) {
            f[i, i] = new List<int> { values[i] };
        }

        for (var diff = 1; diff < values.Count; ++diff) {
            for (var left = 0; left + diff < values.Count; ++left) {
                var right = left + diff;
                f[left, right] = new List<int>();
                for (var i = left; i < right; ++i) {
                    foreach (var leftValue in f[left, i]) {
                        foreach (var rightValue in f[i + 1, right]) {
                            switch (operators[i]) {
                                case '+':
                                    f[left, right].Add(leftValue + rightValue);
                                    break;
                                case '-':
                                    f[left, right].Add(leftValue - rightValue);
                                    break;
                                case '*':
                                    f[left, right].Add(leftValue * rightValue);
                                    break;
                            }
                        }
                    }
                }
            }
        }

        return f[0, values.Count - 1];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
