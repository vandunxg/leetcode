---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [16.26. Calculator](https://leetcode.cn/problems/calculator-lcci)

[中文文档](/lcci/16.26.Calculator/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một biểu thức số học gồm các số nguyên dương, +, -, * và / (không có dấu ngoặc), hãy tính kết quả.</p>
<p>Chuỗi biểu thức chỉ chứa các số nguyên không âm, các toán tử +, -, *, / và khoảng trắng. Phép chia lấy phần nguyên phải cắt phần thập phân theo hướng về 0.</p>
<p><strong>Ví dụ&nbsp;1:</strong></p>
<pre>

<strong>Đầu vào: </strong>&quot;3+2\*2&quot;

<strong>Đầu ra:</strong> 7

</pre>
<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong>Đầu vào:</strong> &quot; 3/2 &quot;

<strong>Đầu ra:</strong> 1</pre>

<p><strong>Ví dụ 3:</strong></p>
<pre>

<strong>Đầu vào:</strong> &quot; 3+5 / 2 &quot;

<strong>Đầu ra:</strong> 5

</pre>

<p><strong>Lưu ý:</strong></p>
<ul>
	<li>Có thể giả định rằng biểu thức được cho luôn hợp lệ.</li>
	<li>Không được sử dụng hàm thư viện eval tích hợp sẵn.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack

<!-- thinking:start -->

> **Tư duy**
>
> Biểu thức $+,-,*,/$ không có dấu ngoặc. Có thể tách đệ quy hoặc dùng RPN, nhưng cách viết sẽ dài hơn.
>
> Biểu thức là tổng của các hạng tử, trong đó mỗi hạng tử đã áp dụng $*$ và $/$.
>
> Stack lưu các hạng tử: `+`/`-` đưa một số có dấu vào stack; `*`/`/` lập tức kết hợp với phần tử trên cùng. Kết quả là tổng các phần tử trong stack. Các số có nhiều chữ số được tích lũy theo $x=x*10+d$.

<!-- thinking:end -->

Ta có thể dùng một stack để lưu các số. Mỗi khi gặp một toán tử, ta đưa số vào stack. Với phép cộng và phép trừ, vì chúng có độ ưu tiên thấp nhất, ta có thể trực tiếp đưa các số vào stack. Với phép nhân và phép chia, vì chúng có độ ưu tiên cao hơn, ta cần lấy phần tử trên cùng của stack ra, thực hiện phép nhân hoặc chia với số hiện tại, rồi đưa kết quả trở lại stack.

Cuối cùng, tổng của tất cả các phần tử trong stack chính là đáp án.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def calculate(self, s: str) -> int:
        n = len(s)
        x = 0
        sign = "+"
        stk = []
        for i, c in enumerate(s):
            if c.isdigit():
                x = x * 10 + ord(c) - ord("0")
            if i == n - 1 or c in "+-*/":
                match sign:
                    case "+":
                        stk.append(x)
                    case "-":
                        stk.append(-x)
                    case "*":
                        stk.append(stk.pop() * x)
                    case "/":
                        stk.append(int(stk.pop() / x))
                x = 0
                sign = c
        return sum(stk)
```

#### Java

```java
class Solution {
    public int calculate(String s) {
        int n = s.length();
        int x = 0;
        char sign = '+';
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                x = x * 10 + (c - '0');
            }
            if (i == n - 1 || !Character.isDigit(c) && c != ' ') {
                switch (sign) {
                    case '+' -> stk.push(x);
                    case '-' -> stk.push(-x);
                    case '*' -> stk.push(stk.pop() * x);
                    case '/' -> stk.push(stk.pop() / x);
                }
                x = 0;
                sign = c;
            }
        }
        int ans = 0;
        while (!stk.isEmpty()) {
            ans += stk.pop();
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int calculate(string s) {
        int n = s.size();
        int x = 0;
        char sign = '+';
        stack<int> stk;
        for (int i = 0; i < n; ++i) {
            char c = s[i];
            if (isdigit(c)) {
                x = x * 10 + (c - '0');
            }
            if (i == n - 1 || !isdigit(c) && c != ' ') {
                if (sign == '+') {
                    stk.push(x);
                } else if (sign == '-') {
                    stk.push(-x);
                } else if (sign == '*') {
                    int y = stk.top();
                    stk.pop();
                    stk.push(y * x);
                } else if (sign == '/') {
                    int y = stk.top();
                    stk.pop();
                    stk.push(y / x);
                }
                x = 0;
                sign = c;
            }
        }
        int ans = 0;
        while (!stk.empty()) {
            ans += stk.top();
            stk.pop();
        }
        return ans;
    }
};
```

#### Go

```go
func calculate(s string) (ans int) {
	n := len(s)
	x := 0
	sign := '+'
	stk := []int{}
	for i := range s {
		if s[i] >= '0' && s[i] <= '9' {
			x = x*10 + int(s[i]-'0')
		}
		if i == n-1 || (s[i] != ' ' && (s[i] < '0' || s[i] > '9')) {
			switch sign {
			case '+':
				stk = append(stk, x)
			case '-':
				stk = append(stk, -x)
			case '*':
				stk[len(stk)-1] *= x
			case '/':
				stk[len(stk)-1] /= x
			}
			x = 0
			sign = rune(s[i])
		}
	}
	for _, x := range stk {
		ans += x
	}
	return
}
```

#### TypeScript

```ts
function calculate(s: string): number {
    const n = s.length;
    let x = 0;
    let sign = '+';
    const stk: number[] = [];
    for (let i = 0; i < n; ++i) {
        if (!isNaN(Number(s[i])) && s[i] !== ' ') {
            x = x * 10 + s[i].charCodeAt(0) - '0'.charCodeAt(0);
        }
        if (i === n - 1 || (isNaN(Number(s[i])) && s[i] !== ' ')) {
            switch (sign) {
                case '+':
                    stk.push(x);
                    break;
                case '-':
                    stk.push(-x);
                    break;
                case '*':
                    stk.push(stk.pop()! * x);
                    break;
                default:
                    stk.push((stk.pop()! / x) | 0);
            }
            x = 0;
            sign = s[i];
        }
    }
    return stk.reduce((x, y) => x + y);
}
```

#### Swift

```swift
class Solution {
    func calculate(_ s: String) -> Int {
        let n = s.count
        var x = 0
        var sign: Character = "+"
        var stk = [Int]()
        let sArray = Array(s)

        for i in 0..<n {
            let c = sArray[i]
            if c.isNumber {
                x = x * 10 + Int(String(c))!
            }
            if i == n - 1 || (!c.isNumber && c != " ") {
                switch sign {
                case "+":
                    stk.append(x)
                case "-":
                    stk.append(-x)
                case "*":
                    if let last = stk.popLast() {
                        stk.append(last * x)
                    }
                case "/":
                    if let last = stk.popLast() {
                        stk.append(last / x)
                    }
                default:
                    break
                }
                x = 0
                sign = c
            }
        }

        return stk.reduce(0, +)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
