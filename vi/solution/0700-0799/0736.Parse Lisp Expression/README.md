---
comments: true
difficulty: Hard
tags:
    - Stack
    - Recursion
    - Hash Table
    - String
---

<!-- problem:start -->

# [736. Parse Lisp Expression](https://leetcode.com/problems/parse-lisp-expression)

[中文文档](/solution/0700-0799/0736.Parse%20Lisp%20Expression/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi biểu thức <code>expression</code> biểu diễn một biểu thức kiểu Lisp. Hãy trả về giá trị số nguyên của biểu thức đó.</p>

<p>Cú pháp của các biểu thức này được định nghĩa như sau.</p>

<ul>
	<li>Một biểu thức có thể là số nguyên, biểu thức let, biểu thức add, biểu thức mult hoặc biến đã được gán giá trị. Mỗi biểu thức luôn cho kết quả là một số nguyên duy nhất.</li>
	<li>(Số nguyên có thể dương hoặc âm.)</li>
	<li>Biểu thức let có dạng <code>&quot;(let v<sub>1</sub> e<sub>1</sub> v<sub>2</sub> e<sub>2</sub> ... v<sub>n</sub> e<sub>n</sub> expr)&quot;</code>, trong đó let luôn là chuỗi <code>&quot;let&quot;</code>, theo sau là một hoặc nhiều cặp biến và biểu thức xen kẽ. Biến thứ nhất <code>v<sub>1</sub></code> được gán giá trị của biểu thức <code>e<sub>1</sub></code>, biến thứ hai <code>v<sub>2</sub></code> được gán giá trị của biểu thức <code>e<sub>2</sub></code>, và cứ tiếp tục theo thứ tự; giá trị của biểu thức let là giá trị của biểu thức <code>expr</code>.</li>
	<li>Biểu thức add có dạng <code>&quot;(add e<sub>1</sub> e<sub>2</sub>)&quot;</code>, trong đó add luôn là chuỗi <code>&quot;add&quot;</code>, có đúng hai biểu thức <code>e<sub>1</sub></code>, <code>e<sub>2</sub></code>, và kết quả là tổng giá trị của chúng.</li>
	<li>Biểu thức mult có dạng <code>&quot;(mult e<sub>1</sub> e<sub>2</sub>)&quot;</code>, trong đó mult luôn là chuỗi <code>&quot;mult&quot;</code>, có đúng hai biểu thức <code>e<sub>1</sub></code>, <code>e<sub>2</sub></code>, và kết quả là tích giá trị của chúng.</li>
	<li>Bài toán dùng một tập con giới hạn các tên biến. Tên biến bắt đầu bằng chữ cái viết thường, sau đó có thể gồm không hoặc nhiều chữ cái viết thường hoặc chữ số. Ngoài ra, các tên <code>&quot;add&quot;</code>, <code>&quot;let&quot;</code> và <code>&quot;mult&quot;</code> được dành riêng, không bao giờ được dùng làm tên biến.</li>
	<li>Cuối cùng là khái niệm scope. Khi đánh giá biểu thức chứa tên biến, trước tiên ta tìm giá trị của biến trong scope gần nhất (xét theo cặp ngoặc), sau đó lần lượt kiểm tra các scope bên ngoài. Đảm bảo mọi biểu thức đều hợp lệ. Xem ví dụ để biết thêm chi tiết về scope.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;(let x 2 (mult x (let x 3 y 4 (add x y))))&quot;
<strong>Đầu ra:</strong> 14
<strong>Giải thích:</strong> Trong biểu thức (add x y), khi tìm giá trị của biến x,
ta kiểm tra từ scope trong cùng ra ngoài theo ngữ cảnh đang đánh giá biến.
Vì tìm thấy x = 3 trước nên giá trị của x là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;(let x 3 x 2 x)&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các phép gán trong câu lệnh let được xử lý tuần tự.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;(let x 1 y 2 x (add x y) (add x y))&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Biểu thức (add x y) thứ nhất cho kết quả 3 và được gán cho x.
Biểu thức (add x y) thứ hai cho kết quả 3+2 = 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= expression.length &lt;= 2000</code></li>
	<li><code>expression</code> không có dấu cách ở đầu hoặc cuối.</li>
	<li>Các token trong <code>expression</code> được phân tách bằng đúng một dấu cách.</li>
	<li>Đáp án và mọi phép tính trung gian được đảm bảo nằm trong phạm vi số nguyên <strong>32-bit</strong>.</li>
	<li>Biểu thức được đảm bảo hợp lệ và cho kết quả là số nguyên.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Ngôn ngữ gồm số nguyên, $\textit{let}$, $\textit{add}$, $\textit{mult}$ và các scope lồng nhau. Độ dài tối đa là $2000$. Có thể tách token trước hoặc duyệt trực tiếp chuỗi bằng một chỉ số.
>
> $\textit{let}$ gán giá trị cho tên biến và scope bên trong che khuất scope bên ngoài, nên mỗi biến cần lưu một stack giá trị và pop sau khi xử lý xong biểu thức. $\textit{add}$/$\textit{mult}$ chỉ cần đánh giá hai biểu thức con.
>
> $\textit{eval}$ rẽ nhánh theo token hiện tại: tên biến hoặc số nguyên đứng riêng, hay biểu thức `let`/`add`/`mult` trong ngoặc. Việc gán biến được quản lý bằng thao tác push/pop trên $\textit{scope}$. Chỉ cần duyệt chuỗi một lần để đánh giá biểu thức.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def evaluate(self, expression: str) -> int:
        def parseVar():
            nonlocal i
            j = i
            while i < n and expression[i] not in " )":
                i += 1
            return expression[j:i]

        def parseInt():
            nonlocal i
            sign, v = 1, 0
            if expression[i] == "-":
                sign = -1
                i += 1
            while i < n and expression[i].isdigit():
                v = v * 10 + int(expression[i])
                i += 1
            return sign * v

        def eval():
            nonlocal i
            if expression[i] != "(":
                return scope[parseVar()][-1] if expression[i].islower() else parseInt()
            i += 1
            if expression[i] == "l":
                i += 4
                vars = []
                while 1:
                    var = parseVar()
                    if expression[i] == ")":
                        ans = scope[var][-1]
                        break
                    vars.append(var)
                    i += 1
                    scope[var].append(eval())
                    i += 1
                    if not expression[i].islower():
                        ans = eval()
                        break
                for v in vars:
                    scope[v].pop()
            else:
                add = expression[i] == "a"
                i += 4 if add else 5
                a = eval()
                i += 1
                b = eval()
                ans = a + b if add else a * b
            i += 1
            return ans

        i, n = 0, len(expression)
        scope = defaultdict(list)
        return eval()
```

#### Java

```java
class Solution {
    private int i;
    private String expr;
    private Map<String, Deque<Integer>> scope = new HashMap<>();

    public int evaluate(String expression) {
        expr = expression;
        return eval();
    }

    private int eval() {
        char c = expr.charAt(i);
        if (c != '(') {
            return Character.isLowerCase(c) ? scope.get(parseVar()).peekLast() : parseInt();
        }
        ++i;
        c = expr.charAt(i);
        int ans = 0;
        if (c == 'l') {
            i += 4;
            List<String> vars = new ArrayList<>();
            while (true) {
                String var = parseVar();
                if (expr.charAt(i) == ')') {
                    ans = scope.get(var).peekLast();
                    break;
                }
                vars.add(var);
                ++i;
                scope.computeIfAbsent(var, k -> new ArrayDeque<>()).offer(eval());
                ++i;
                if (!Character.isLowerCase(expr.charAt(i))) {
                    ans = eval();
                    break;
                }
            }
            for (String v : vars) {
                scope.get(v).pollLast();
            }
        } else {
            boolean add = c == 'a';
            i += add ? 4 : 5;
            int a = eval();
            ++i;
            int b = eval();
            ans = add ? a + b : a * b;
        }
        ++i;
        return ans;
    }

    private String parseVar() {
        int j = i;
        while (i < expr.length() && expr.charAt(i) != ' ' && expr.charAt(i) != ')') {
            ++i;
        }
        return expr.substring(j, i);
    }

    private int parseInt() {
        int sign = 1;
        if (expr.charAt(i) == '-') {
            sign = -1;
            ++i;
        }
        int v = 0;
        while (i < expr.length() && Character.isDigit(expr.charAt(i))) {
            v = v * 10 + (expr.charAt(i) - '0');
            ++i;
        }
        return sign * v;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int i = 0;
    string expr;
    unordered_map<string, vector<int>> scope;

    int evaluate(string expression) {
        expr = expression;
        return eval();
    }

    int eval() {
        if (expr[i] != '(') return islower(expr[i]) ? scope[parseVar()].back() : parseInt();
        int ans = 0;
        ++i;
        if (expr[i] == 'l') {
            i += 4;
            vector<string> vars;
            while (1) {
                string var = parseVar();
                if (expr[i] == ')') {
                    ans = scope[var].back();
                    break;
                }
                ++i;
                vars.push_back(var);
                scope[var].push_back(eval());
                ++i;
                if (!islower(expr[i])) {
                    ans = eval();
                    break;
                }
            }
            for (string v : vars) scope[v].pop_back();
        } else {
            bool add = expr[i] == 'a';
            i += add ? 4 : 5;
            int a = eval();
            ++i;
            int b = eval();
            ans = add ? a + b : a * b;
        }
        ++i;
        return ans;
    }

    string parseVar() {
        int j = i;
        while (i < expr.size() && expr[i] != ' ' && expr[i] != ')') ++i;
        return expr.substr(j, i - j);
    }

    int parseInt() {
        int sign = 1, v = 0;
        if (expr[i] == '-') {
            sign = -1;
            ++i;
        }
        while (i < expr.size() && expr[i] >= '0' && expr[i] <= '9') {
            v = v * 10 + (expr[i] - '0');
            ++i;
        }
        return sign * v;
    }
};
```

#### Go

```go
func evaluate(expression string) int {
	i, n := 0, len(expression)
	scope := map[string][]int{}

	parseVar := func() string {
		j := i
		for ; i < n && expression[i] != ' ' && expression[i] != ')'; i++ {
		}
		return expression[j:i]
	}

	parseInt := func() int {
		sign, v := 1, 0
		if expression[i] == '-' {
			sign = -1
			i++
		}
		for ; i < n && expression[i] >= '0' && expression[i] <= '9'; i++ {
			v = (v * 10) + int(expression[i]-'0')
		}
		return sign * v
	}

	var eval func() int
	eval = func() int {
		if expression[i] != '(' {
			if unicode.IsLower(rune(expression[i])) {
				t := scope[parseVar()]
				return t[len(t)-1]
			}
			return parseInt()
		}
		i++
		ans := 0
		if expression[i] == 'l' {
			i += 4
			vars := []string{}
			for {
				v := parseVar()
				if expression[i] == ')' {
					t := scope[v]
					ans = t[len(t)-1]
					break
				}
				i++
				vars = append(vars, v)
				scope[v] = append(scope[v], eval())
				i++
				if !unicode.IsLower(rune(expression[i])) {
					ans = eval()
					break
				}
			}
			for _, v := range vars {
				scope[v] = scope[v][:len(scope[v])-1]
			}
		} else {
			add := expression[i] == 'a'
			if add {
				i += 4
			} else {
				i += 5
			}
			a := eval()
			i++
			b := eval()
			if add {
				ans = a + b
			} else {
				ans = a * b
			}
		}
		i++
		return ans
	}
	return eval()
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
