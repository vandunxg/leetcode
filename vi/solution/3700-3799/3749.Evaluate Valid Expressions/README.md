---
comments: true
difficulty: Hard
tags:
    - Stack
    - Hash Table
    - Math
    - String
    - Divide and Conquer
---

<!-- problem:start -->

# [3749. Evaluate Valid Expressions 🔒](https://leetcode.com/problems/evaluate-valid-expressions)

[中文文档](/solution/3700-3799/3749.Evaluate%20Valid%20Expressions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>expression</code> biểu diễn một biểu thức toán học lồng nhau ở dạng đơn giản hóa.</p>

<p>Một biểu thức <strong>hợp lệ</strong> là một <strong>literal</strong> số nguyên hoặc tuân theo định dạng <code>op(a,b)</code>, trong đó:</p>

<ul>
	<li><code>op</code> là một trong các giá trị <code>&quot;add&quot;</code>, <code>&quot;sub&quot;</code>, <code>&quot;mul&quot;</code> hoặc <code>&quot;div&quot;</code>.</li>
	<li><code>a</code> và <code>b</code> đều là các biểu thức hợp lệ.</li>
</ul>

<p>Các <strong>phép toán</strong> được định nghĩa như sau:</p>

<ul>
	<li><code>add(a,b) = a + b</code></li>
	<li><code>sub(a,b) = a - b</code></li>
	<li><code>mul(a,b) = a * b</code></li>
	<li><code>div(a,b) = a / b</code></li>
</ul>

<p>Trả về một số nguyên biểu diễn <strong>kết quả</strong> sau khi tính giá trị đầy đủ của biểu thức.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">expression = &quot;add(2,3)&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Phép toán <code>add(2,3)</code> có nghĩa là <code>2 + 3 = 5</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">expression = &quot;-42&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-42</span></p>

<p><strong>Giải thích:</strong></p>

<p>Biểu thức chỉ là một literal số nguyên, nên kết quả là -42.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">expression = &quot;div(mul(4,sub(9,5)),add(1,1))&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Trước hết, tính biểu thức bên trong: <code>sub(9,5) = 9 - 5 = 4</code></li>
	<li>Tiếp theo, nhân các kết quả: <code>mul(4,4) = 4 * 4 = 16</code></li>
	<li>Sau đó, thực hiện phép cộng ở bên phải: <code>add(1,1) = 1 + 1 = 2</code></li>
	<li>Cuối cùng, chia hai kết quả chính: <code>div(16,2) = 16 / 2 = 8</code></li>
</ul>

<p>Do đó, giá trị của toàn bộ biểu thức là 8.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= expression.length &lt;= 10<sup>5</sup></code></li>
	<li><code>expression</code> hợp lệ và chỉ gồm các chữ số, dấu phẩy, dấu ngoặc đơn, dấu trừ <code>&#39;-&#39;</code> và các chuỗi chữ thường <code>&quot;add&quot;</code>, <code>&quot;sub&quot;</code>, <code>&quot;mul&quot;</code>, <code>&quot;div&quot;</code>.</li>
	<li>Tất cả các kết quả trung gian đều nằm trong phạm vi của một số nguyên long.</li>
	<li>Tất cả các phép chia đều cho kết quả nguyên.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Biểu thức có dạng lồng nhau $\mathrm{op}(a,b)$, với cấu trúc được xác định bởi các dấu ngoặc, nên đệ quy theo cú pháp là lựa chọn phù hợp. Tại chỉ số hiện tại, ta có thể phân tích một literal hoặc đọc một toán tử, đệ quy trên hai toán hạng rồi áp dụng $\mathrm{add}/\mathrm{sub}/\mathrm{mul}/\mathrm{div}$.

<!-- thinking:end -->

Ta định nghĩa một hàm đệ quy $\text{parse}(i)$ để phân tích biểu thức con bắt đầu từ chỉ số $i$ và trả về kết quả đã tính cùng với vị trí chỉ số tiếp theo chưa được xử lý. Đáp án là $\text{parse}(0)[0]$.

Cách triển khai hàm $\text{parse}(i)$ như sau:

1. Nếu vị trí hiện tại $i$ là một chữ số hoặc dấu âm `-`, tiếp tục quét về phía trước cho đến khi gặp một ký tự không phải chữ số, phân tích thành một số nguyên và trả về số nguyên đó cùng với vị trí chỉ số tiếp theo chưa được xử lý.
2. Nếu không, vị trí hiện tại $i$ là vị trí bắt đầu của một toán tử `op`. Tiếp tục quét về phía trước cho đến khi gặp dấu ngoặc mở `(`, đồng thời phân tích chuỗi toán tử `op`. Sau đó bỏ qua dấu ngoặc mở, gọi đệ quy $\text{parse}$ để phân tích tham số thứ nhất $a$, bỏ qua dấu phẩy, gọi đệ quy $\text{parse}$ để phân tích tham số thứ hai $b$, rồi cuối cùng bỏ qua dấu ngoặc đóng `)`.
3. Dựa trên toán tử `op`, tính kết quả của $a$ và $b$, rồi trả về kết quả đó cùng với vị trí chỉ số tiếp theo chưa được xử lý.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi biểu thức.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def evaluateExpression(self, expression: str) -> int:
        def parse(i: int) -> (int, int):
            if expression[i].isdigit() or expression[i] == "-":
                j = i
                if expression[j] == "-":
                    j += 1
                while j < len(expression) and expression[j].isdigit():
                    j += 1
                return int(expression[i:j]), j

            j = i
            while expression[j] != "(":
                j += 1
            op = expression[i:j]
            j += 1
            val1, j = parse(j)

            j += 1
            val2, j = parse(j)
            j += 1
            res = 0
            match op:
                case "add":
                    res = val1 + val2
                case "sub":
                    res = val1 - val2
                case "mul":
                    res = val1 * val2
                case "div":
                    res = val1 // val2
            return res, j

        return parse(0)[0]
```

#### Java

```java
class Solution {
    private String expression;

    public long evaluateExpression(String expression) {
        this.expression = expression;
        return parse(0)[0];
    }

    private long[] parse(int i) {
        if (Character.isDigit(expression.charAt(i)) || expression.charAt(i) == '-') {
            int j = i;
            if (expression.charAt(j) == '-') {
                j++;
            }
            while (j < expression.length() && Character.isDigit(expression.charAt(j))) {
                j++;
            }
            long num = Long.parseLong(expression.substring(i, j));
            return new long[] {num, j};
        }

        int j = i;
        while (expression.charAt(j) != '(') {
            j++;
        }
        String op = expression.substring(i, j);
        j++;

        long[] result1 = parse(j);
        long val1 = result1[0];
        j = (int) result1[1];
        j++;

        long[] result2 = parse(j);
        long val2 = result2[0];
        j = (int) result2[1];
        j++;

        long res = 0;
        switch (op) {
        case "add":
            res = val1 + val2;
            break;
        case "sub":
            res = val1 - val2;
            break;
        case "mul":
            res = val1 * val2;
            break;
        case "div":
            res = val1 / val2;
            break;
        }

        return new long[] {res, j};
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long evaluateExpression(string expression) {
        auto parse = [&](this auto&& parse, int i) -> pair<long long, int> {
            if (isdigit(expression[i]) || expression[i] == '-') {
                int j = i;
                if (expression[j] == '-') {
                    j++;
                }
                while (j < expression.size() && isdigit(expression[j])) {
                    j++;
                }
                long long num = stoll(expression.substr(i, j - i));
                return {num, j};
            }

            int j = i;
            while (expression[j] != '(') {
                j++;
            }
            string op = expression.substr(i, j - i);
            j++;

            auto [val1, next_j1] = parse(j);
            j = next_j1 + 1;

            auto [val2, next_j2] = parse(j);
            j = next_j2 + 1;

            long long res = 0;
            if (op == "add") {
                res = val1 + val2;
            } else if (op == "sub") {
                res = val1 - val2;
            } else if (op == "mul") {
                res = val1 * val2;
            } else if (op == "div") {
                res = val1 / val2;
            }

            return {res, j};
        };

        return parse(0).first;
    }
};
```

#### Go

```go
func evaluateExpression(expression string) int64 {
	var parse func(int) (int64, int)
	parse = func(i int) (int64, int) {
		if expression[i] >= '0' && expression[i] <= '9' || expression[i] == '-' {
			j := i
			if expression[j] == '-' {
				j++
			}
			for j < len(expression) && expression[j] >= '0' && expression[j] <= '9' {
				j++
			}
			num, _ := strconv.ParseInt(expression[i:j], 10, 64)
			return num, j
		}

		j := i
		for expression[j] != '(' {
			j++
		}
		op := expression[i:j]
		j++

		val1, nextJ1 := parse(j)
		j = nextJ1 + 1

		val2, nextJ2 := parse(j)
		j = nextJ2 + 1

		var res int64
		switch op {
		case "add":
			res = val1 + val2
		case "sub":
			res = val1 - val2
		case "mul":
			res = val1 * val2
		case "div":
			res = val1 / val2
		}

		return res, j
	}

	result, _ := parse(0)
	return result
}
```

#### TypeScript

```ts
function evaluateExpression(expression: string): number {
    function parse(i: number): [number, number] {
        if (/\d/.test(expression[i]) || expression[i] === '-') {
            let j = i;
            if (expression[j] === '-') {
                j++;
            }
            while (j < expression.length && /\d/.test(expression[j])) {
                j++;
            }
            const num = +expression.slice(i, j);
            return [num, j];
        }

        let j = i;
        while (expression[j] !== '(') {
            j++;
        }
        const op = expression.slice(i, j);
        j++;

        const [val1, nextJ1] = parse(j);
        j = nextJ1 + 1;

        const [val2, nextJ2] = parse(j);
        j = nextJ2 + 1;

        let res: number;
        switch (op) {
            case 'add':
                res = val1 + val2;
                break;
            case 'sub':
                res = val1 - val2;
                break;
            case 'mul':
                res = val1 * val2;
                break;
            case 'div':
                res = Math.floor(val1 / val2);
                break;
            default:
                res = 0;
        }

        return [res, j];
    }

    return parse(0)[0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
