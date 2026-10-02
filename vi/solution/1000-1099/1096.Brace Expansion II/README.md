---
comments: true
difficulty: Hard
rating: 2348
source: Weekly Contest 142 Q4
tags:
    - Stack
    - Breadth-First Search
    - Hash Table
    - String
    - Backtracking
    - Sorting
---

<!-- problem:start -->

# [1096. Brace Expansion II](https://leetcode.com/problems/brace-expansion-ii)

[中文文档](/solution/1000-1099/1096.Brace%20Expansion%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Theo ngữ pháp dưới đây, một chuỗi có thể biểu diễn một tập hợp các từ viết thường. Ký hiệu <code>R(expr)</code> là tập hợp các từ được biểu thức biểu diễn.</p>

<p>Có thể hiểu ngữ pháp này qua một số ví dụ đơn giản:</p>

<ul>
	<li>Một chữ cái biểu diễn tập đơn chỉ chứa từ đó.
	<ul>
		<li><code>R(&quot;a&quot;) = {&quot;a&quot;}</code></li>
		<li><code>R(&quot;w&quot;) = {&quot;w&quot;}</code></li>
	</ul>
	</li>
	<li>Khi có danh sách gồm từ hai biểu thức trở lên, phân tách bằng dấu phẩy, ta lấy hợp các khả năng.
	<ul>
		<li><code>R(&quot;{a,b,c}&quot;) = {&quot;a&quot;,&quot;b&quot;,&quot;c&quot;}</code></li>
		<li><code>R(&quot;{{a,b},{b,c}}&quot;) = {&quot;a&quot;,&quot;b&quot;,&quot;c&quot;}</code> (lưu ý rằng mỗi từ chỉ xuất hiện tối đa một lần trong tập kết quả)</li>
	</ul>
	</li>
	<li>Khi nối hai biểu thức, ta lấy tập hợp mọi cách nối hai từ, trong đó từ thứ nhất thuộc biểu thức đầu và từ thứ hai thuộc biểu thức sau.
	<ul>
		<li><code>R(&quot;{a,b}{c,d}&quot;) = {&quot;ac&quot;,&quot;ad&quot;,&quot;bc&quot;,&quot;bd&quot;}</code></li>
		<li><code>R(&quot;a{b,c}{d,e}f{g,h}&quot;) = {&quot;abdfg&quot;, &quot;abdfh&quot;, &quot;abefg&quot;, &quot;abefh&quot;, &quot;acdfg&quot;, &quot;acdfh&quot;, &quot;acefg&quot;, &quot;acefh&quot;}</code></li>
	</ul>
	</li>
</ul>

<p>Định nghĩa hình thức, ngữ pháp có ba quy tắc:</p>

<ul>
	<li>Với mọi chữ cái viết thường <code>x</code>, ta có <code>R(x) = {x}</code>.</li>
	<li>Với các biểu thức <code>e<sub>1</sub>, e<sub>2</sub>, ... , e<sub>k</sub></code> có <code>k &gt;= 2</code>, ta có <code>R({e<sub>1</sub>, e<sub>2</sub>, ...}) = R(e<sub>1</sub>) &cup; R(e<sub>2</sub>) &cup; ...</code>.</li>
	<li>Với các biểu thức <code>e<sub>1</sub></code> và <code>e<sub>2</sub></code>, ta có <code>R(e<sub>1</sub> + e<sub>2</sub>) = {a + b for (a, b) in R(e<sub>1</sub>) &times; R(e<sub>2</sub>)}</code>, trong đó <code>+</code> biểu thị phép nối chuỗi và <code>&times;</code> biểu thị tích Descartes.</li>
</ul>

<p>Cho một biểu thức biểu diễn tập hợp các từ theo ngữ pháp trên, hãy trả về <em>danh sách các từ mà biểu thức biểu diễn, theo thứ tự đã sắp xếp</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;{a,b}{c,{d,e}}&quot;
<strong>Đầu ra:</strong> [&quot;ac&quot;,&quot;ad&quot;,&quot;ae&quot;,&quot;bc&quot;,&quot;bd&quot;,&quot;be&quot;]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;{{a,z},a{b,c},{ab,z}}&quot;
<strong>Đầu ra:</strong> [&quot;a&quot;,&quot;ab&quot;,&quot;ac&quot;,&quot;z&quot;]
<strong>Giải thích:</strong> Mỗi từ khác nhau chỉ xuất hiện một lần trong đáp án cuối cùng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= expression.length &lt;= 60</code></li>
	<li><code>expression[i]</code> chỉ gồm <code>&#39;{&#39;</code>, <code>&#39;}&#39;</code>, <code>&#39;,&#39;</code> hoặc chữ cái tiếng Anh viết thường.</li>
	<li><code>expression</code> biểu diễn một tập hợp từ theo ngữ pháp đã nêu trong mô tả.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Biểu thức kết hợp phép hợp, phép nối chuỗi và các dấu ngoặc lồng nhau. Với độ dài $\le 60$, ta có thể khai triển cặp ngoặc trong cùng, ghép lại biểu thức rồi đệ quy.
>
> Tìm dấu `}` đầu tiên và dấu `{` tương ứng. Tiền tố $a$, từng lựa chọn $b_i$ và hậu tố $c$ được ghép thành $a+b_i+c$. Khi chuỗi không còn dấu ngoặc, thêm chuỗi đó vào set.
>
> Sắp xếp set để thu được đáp án.

<!-- thinking:end -->

Định nghĩa hàm đệ quy $\textit{dfs}(\textit{exp})$ để khai triển $\textit{exp}$ và lưu mọi từ vào set $s$.

Tìm chỉ số $j$ của dấu `}` đầu tiên. Nếu không có dấu này, $\textit{exp}$ đã là một từ hoàn chỉnh, nên thêm nó vào $s$.

Nếu không, tìm ngược từ $j$ để xác định dấu `{` tương ứng tại chỉ số $i$. Tiền tố $\textit{exp}[:i]$ và hậu tố $\textit{exp}[j + 1:]$ lần lượt là $a$ và $c$. Tách phần thân trong ngoặc $\textit{exp}[i + 1: j]$ theo dấu phẩy để được $b_1, b_2, \cdots, b_k$, rồi đệ quy với $\textit{dfs}(a + b_i + c)$ cho từng $b_i$. Dấu `}` đầu tiên luôn đóng một nhóm ngoặc không chứa ngoặc lồng bên trong, nên có thể tách chính xác phần thân đó theo dấu phẩy.

Sắp xếp $s$ theo thứ tự từ điển để thu được đáp án.

Độ phức tạp thời gian là $O(3^{n/6})$ và độ phức tạp không gian là $O(n \times 3^{n/7})$, trong đó $n$ là độ dài của $\textit{expression}$. Trường hợp xấu nhất về thời gian là một phép hợp ba nhánh lồng nhau như $\{\ldots\{a,b,c\},a,b\}$: mỗi 6 ký tự bổ sung làm cây đệ quy lớn thêm khoảng 3 lần, khiến tổng độ dài các chuỗi được xử lý là $\Theta(3^{n/6})$. Nối các nhóm `{a,b,c}` tạo ra $\Theta(3^{n/7})$ từ có độ dài $O(n)$. Tập đã loại trùng chiếm $O(n \times 3^{n/7})$ bộ nhớ; sắp xếp tập tốn $O(n^2 \times 3^{n/7})$, vẫn nằm trong giới hạn thời gian trên. Độ sâu đệ quy là $O(n)$, nên các chuỗi trên stack dùng thêm $O(n^2)$ bộ nhớ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def braceExpansionII(self, expression: str) -> List[str]:
        def dfs(exp):
            j = exp.find('}')
            if j == -1:
                s.add(exp)
                return
            i = exp.rfind('{', 0, j)
            a, c = exp[:i], exp[j + 1 :]
            for b in exp[i + 1 : j].split(','):
                dfs(a + b + c)

        s = set()
        dfs(expression)
        return sorted(s)
```

#### Java

```java
class Solution {
    private TreeSet<String> s = new TreeSet<>();

    public List<String> braceExpansionII(String expression) {
        dfs(expression);
        return new ArrayList<>(s);
    }

    private void dfs(String exp) {
        int j = exp.indexOf('}');
        if (j == -1) {
            s.add(exp);
            return;
        }
        int i = exp.lastIndexOf('{', j);
        String a = exp.substring(0, i);
        String c = exp.substring(j + 1);
        for (String b : exp.substring(i + 1, j).split(",")) {
            dfs(a + b + c);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> braceExpansionII(string expression) {
        dfs(expression);
        return vector<string>(s.begin(), s.end());
    }

private:
    set<string> s;

    void dfs(string exp) {
        int j = exp.find_first_of('}');
        if (j == string::npos) {
            s.insert(exp);
            return;
        }
        int i = exp.rfind('{', j);
        string a = exp.substr(0, i);
        string c = exp.substr(j + 1);
        stringstream ss(exp.substr(i + 1, j - i - 1));
        string b;
        while (getline(ss, b, ',')) {
            dfs(a + b + c);
        }
    }
};
```

#### Go

```go
func braceExpansionII(expression string) []string {
	s := map[string]struct{}{}
	var dfs func(string)
	dfs = func(exp string) {
		j := strings.Index(exp, "}")
		if j == -1 {
			s[exp] = struct{}{}
			return
		}
		i := strings.LastIndex(exp[:j], "{")
		a, c := exp[:i], exp[j+1:]
		for _, b := range strings.Split(exp[i+1:j], ",") {
			dfs(a + b + c)
		}
	}
	dfs(expression)
	ans := make([]string, 0, len(s))
	for k := range s {
		ans = append(ans, k)
	}
	sort.Strings(ans)
	return ans
}
```

#### TypeScript

```ts
function braceExpansionII(expression: string): string[] {
    const dfs = (exp: string) => {
        const j = exp.indexOf('}');
        if (j === -1) {
            s.add(exp);
            return;
        }
        const i = exp.lastIndexOf('{', j);
        const a = exp.substring(0, i);
        const c = exp.substring(j + 1);
        for (const b of exp.substring(i + 1, j).split(',')) {
            dfs(a + b + c);
        }
    };
    const s: Set<string> = new Set();
    dfs(expression);
    return Array.from(s).sort();
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Phân tích ngữ pháp

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sao chép hậu tố chưa khai triển vào từng lựa chọn. Vì vậy, một phép hợp ba nhánh lồng nhau như $\{\ldots\{a,b,c\},a,b\}$ sẽ lặp lại việc xử lý cùng một hậu tố và tốn $O(3^{n/6})$ thời gian, ngay cả khi số từ phân biệt chỉ là hằng số.
>
> Dấu phẩy chỉ xuất hiện bên trong ngoặc và biểu thị phép hợp. Các thành phần liền kề bên ngoài dấu phẩy biểu thị phép nối chuỗi. Phân tích từ trái sang phải giúp tính mỗi biểu thức con một lần.
>
> Một thành phần là một dãy chữ cái viết thường hoặc một biểu thức trong ngoặc. Phép nối chuỗi được thực hiện bằng tích Descartes giữa tập hiện tại và thành phần đó; dấu phẩy lấy hợp các tập kết quả. Hash set loại bỏ phần tử trùng lặp, rồi sắp xếp tập cuối cùng.

<!-- thinking:end -->

Gọi $s$ là biểu thức. Bắt đầu từ chỉ số $i$, định nghĩa hai hàm.

$\textit{expr}(i)$ phân tích phép hợp các term. Hàm phân tích một $\textit{term}$; khi ký tự tiếp theo là dấu phẩy, nó bỏ qua dấu phẩy, phân tích term tiếp theo rồi lấy hợp hai tập. Hàm dừng tại `}` hoặc cuối $s$.

$\textit{term}(i)$ phân tích phép nối các factor, bắt đầu với $\{\varepsilon\}$. Khi gặp `{`, hàm đệ quy phân tích biểu thức bên trong rồi bỏ qua dấu `}` tương ứng. Nếu không, factor là dãy chữ cái viết thường liên tiếp tiếp theo. Thay tập hiện tại bằng tích Descartes của nó với factor đó. Hàm trả về khi gặp dấu phẩy, `}` hoặc đến cuối $s$.

Sắp xếp tập do $\textit{expr}(0)$ trả về. Mỗi biểu thức con chỉ được phân tích một lần, nên không còn phải sao chép và quét lại hậu tố ở mỗi nhánh.

Độ phức tạp thời gian là $O(n^2 \times 3^{n/7})$ và độ phức tạp không gian là $O(n \times 3^{n/7})$, trong đó $n$ là độ dài biểu thức. Trường hợp xấu nhất là nối nhiều nhóm `{a,b,c}`, tạo ra $\Theta(3^{n/7})$ từ có độ dài $O(n)$. Việc tạo các từ này tốn $O(n \times 3^{n/7})$, còn sắp xếp tốn $O(n^2 \times 3^{n/7})$. Phép hợp ba nhánh lồng nhau giữ cho kích thước mọi tập trung gian là $O(1)$, nên trường hợp đó chỉ tốn $O(n)$ thời gian.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def braceExpansionII(self, expression: str) -> List[str]:
        def expr(i: int):
            res, i = term(i)
            while i < len(expression) and expression[i] == ',':
                other, i = term(i + 1)
                res |= other
            return res, i

        def term(i: int):
            res = {''}
            while i < len(expression) and expression[i] not in ',}':
                if expression[i] == '{':
                    cur, i = expr(i + 1)
                    i += 1
                else:
                    j = i + 1
                    while j < len(expression) and expression[j].islower():
                        j += 1
                    cur = {expression[i:j]}
                    i = j
                res = {a + b for a in res for b in cur}
            return res, i

        ans, _ = expr(0)
        return sorted(ans)
```

#### Java

```java
class Solution {
    private String exp;
    private int i;

    public List<String> braceExpansionII(String expression) {
        exp = expression;
        i = 0;
        List<String> ans = new ArrayList<>(expr());
        Collections.sort(ans);
        return ans;
    }

    private Set<String> expr() {
        Set<String> res = term();
        while (i < exp.length() && exp.charAt(i) == ',') {
            ++i;
            res.addAll(term());
        }
        return res;
    }

    private Set<String> term() {
        Set<String> res = new HashSet<>();
        res.add("");
        while (i < exp.length() && exp.charAt(i) != ',' && exp.charAt(i) != '}') {
            Set<String> cur = new HashSet<>();
            if (exp.charAt(i) == '{') {
                ++i;
                cur = expr();
                ++i;
            } else {
                int j = i + 1;
                while (j < exp.length() && exp.charAt(j) >= 'a' && exp.charAt(j) <= 'z') {
                    ++j;
                }
                cur.add(exp.substring(i, j));
                i = j;
            }
            Set<String> nxt = new HashSet<>();
            for (String a : res) {
                for (String b : cur) {
                    nxt.add(a + b);
                }
            }
            res = nxt;
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> braceExpansionII(string expression) {
        exp = std::move(expression);
        i = 0;
        set<string> ans = parseExpr();
        return vector<string>(ans.begin(), ans.end());
    }

private:
    string exp;
    int i = 0;

    set<string> parseExpr() {
        set<string> res = parseTerm();
        while (i < exp.size() && exp[i] == ',') {
            ++i;
            set<string> other = parseTerm();
            res.insert(other.begin(), other.end());
        }
        return res;
    }

    set<string> parseTerm() {
        set<string> res{""};
        while (i < (int) exp.size() && exp[i] != ',' && exp[i] != '}') {
            set<string> cur;
            if (exp[i] == '{') {
                ++i;
                cur = parseExpr();
                ++i;
            } else {
                int j = i + 1;
                while (j < (int) exp.size() && exp[j] >= 'a' && exp[j] <= 'z') {
                    ++j;
                }
                cur.insert(exp.substr(i, j - i));
                i = j;
            }
            set<string> nxt;
            for (const string& a : res) {
                for (const string& b : cur) {
                    nxt.insert(a + b);
                }
            }
            res.swap(nxt);
        }
        return res;
    }
};
```

#### Go

```go
func braceExpansionII(expression string) []string {
	exp := expression
	i := 0
	var parseExpr func() map[string]struct{}
	var parseTerm func() map[string]struct{}
	parseExpr = func() map[string]struct{} {
		res := parseTerm()
		for i < len(exp) && exp[i] == ',' {
			i++
			for w := range parseTerm() {
				res[w] = struct{}{}
			}
		}
		return res
	}
	parseTerm = func() map[string]struct{} {
		res := map[string]struct{}{"": {}}
		for i < len(exp) && exp[i] != ',' && exp[i] != '}' {
			cur := map[string]struct{}{}
			if exp[i] == '{' {
				i++
				cur = parseExpr()
				i++
			} else {
				j := i + 1
				for j < len(exp) && exp[j] >= 'a' && exp[j] <= 'z' {
					j++
				}
				cur[exp[i:j]] = struct{}{}
				i = j
			}
			nxt := map[string]struct{}{}
			for a := range res {
				for b := range cur {
					nxt[a+b] = struct{}{}
				}
			}
			res = nxt
		}
		return res
	}
	all := parseExpr()
	ans := make([]string, 0, len(all))
	for w := range all {
		ans = append(ans, w)
	}
	sort.Strings(ans)
	return ans
}
```

#### TypeScript

```ts
function braceExpansionII(expression: string): string[] {
    let i = 0;
    const expr = (): Set<string> => {
        const res = term();
        while (i < expression.length && expression[i] === ',') {
            ++i;
            for (const w of term()) {
                res.add(w);
            }
        }
        return res;
    };
    const term = (): Set<string> => {
        let res = new Set<string>(['']);
        while (i < expression.length && expression[i] !== ',' && expression[i] !== '}') {
            let cur: Set<string>;
            if (expression[i] === '{') {
                ++i;
                cur = expr();
                ++i;
            } else {
                let j = i + 1;
                while (j < expression.length && expression[j] >= 'a' && expression[j] <= 'z') {
                    ++j;
                }
                cur = new Set([expression.slice(i, j)]);
                i = j;
            }
            const nxt = new Set<string>();
            for (const a of res) {
                for (const b of cur) {
                    nxt.add(a + b);
                }
            }
            res = nxt;
        }
        return res;
    };
    return Array.from(expr()).sort();
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
