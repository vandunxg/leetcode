---
comments: true
difficulty: Medium
rating: 1426
source: Biweekly Contest 25 Q2
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [1432. Max Difference You Can Get From Changing an Integer](https://leetcode.com/problems/max-difference-you-can-get-from-changing-an-integer)

[中文文档](/solution/1400-1499/1432.Max%20Difference%20You%20Can%20Get%20From%20Changing%20an%20Integer/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>num</code>. Bạn sẽ thực hiện các bước sau với <code>num</code> trong <strong>hai</strong> lần riêng biệt:</p>

<ul>
	<li>Chọn một chữ số <code>x (0 &lt;= x &lt;= 9)</code>.</li>
	<li>Chọn một chữ số khác <code>y (0 &lt;= y &lt;= 9)</code>. Lưu ý rằng <code>y</code> có thể bằng <code>x</code>.</li>
	<li>Thay thế tất cả các lần xuất hiện của <code>x</code> trong biểu diễn thập phân của <code>num</code> bằng <code>y</code>.</li>
</ul>

<p>Gọi <code>a</code> và <code>b</code> là hai kết quả nhận được khi áp dụng phép toán này độc lập với <code>num</code>.</p>

<p>Trả về <em>hiệu lớn nhất</em> giữa <code>a</code> và <code>b</code>.</p>

<p>Lưu ý rằng cả <code>a</code> và <code>b</code> đều không được có các số 0 ở đầu, đồng thời <strong>không được</strong> bằng 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> num = 555
<strong>Output:</strong> 888
<strong>Explanation:</strong> Lần đầu tiên chọn x = 5 và y = 9 rồi lưu số nguyên mới vào a.
Lần thứ hai chọn x = 5 và y = 1 rồi lưu số nguyên mới vào b.
Khi đó a = 999 và b = 111, hiệu lớn nhất = 888
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> num = 9
<strong>Output:</strong> 8
<strong>Explanation:</strong> Lần đầu tiên chọn x = 9 và y = 9 rồi lưu số nguyên mới vào a.
Lần thứ hai chọn x = 9 và y = 1 rồi lưu số nguyên mới vào b.
Khi đó a = 9 và b = 1, hiệu lớn nhất = 8
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Vì $num\le 10^8$, số chữ số là rất ít. Một phép thay thế sẽ viết lại mọi lần xuất hiện của một chữ số; hiệu lớn nhất là giá trị lớn nhất trừ giá trị nhỏ nhất sau khi thực hiện mỗi phép thay thế một lần.
>
> Để đạt giá trị lớn nhất, thay thế chữ số đầu tiên khác `9` bằng `9` ở mọi vị trí. Để đạt giá trị nhỏ nhất, thay chữ số đầu tiên bằng `1` nếu nó chưa phải là `1`; nếu không, thay một chữ số phía sau khác `0` hoặc `1` bằng `0`, tránh tạo số 0 ở đầu.

<!-- thinking:end -->

Để thu được hiệu lớn nhất, ta cần chọn giá trị lớn nhất và nhỏ nhất, vì cách này tạo ra hiệu lớn nhất.

Do đó, trước tiên ta duyệt từng chữ số trong $\textit{nums}$ từ trái sang phải. Nếu một chữ số không phải `9`, ta thay thế tất cả các lần xuất hiện của chữ số đó bằng `9` để thu được số nguyên lớn nhất $a$.

Tiếp theo, ta lại duyệt từng chữ số trong $\textit{nums}$ từ trái sang phải. Chữ số đầu tiên không thể là `0`, nên nếu chữ số đầu tiên không phải `1`, ta thay nó bằng `1`; với các chữ số không ở đầu và khác chữ số đầu tiên, ta thay chúng bằng `0` để thu được số nguyên nhỏ nhất $b$.

Đáp án là hiệu $a - b$.

Độ phức tạp thời gian là $O(\log \textit{num})$, và độ phức tạp không gian là $O(\log \textit{num})$, trong đó $\textit{nums}$ là số nguyên đã cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDiff(self, num: int) -> int:
        a, b = str(num), str(num)
        for c in a:
            if c != "9":
                a = a.replace(c, "9")
                break
        if b[0] != "1":
            b = b.replace(b[0], "1")
        else:
            for c in b[1:]:
                if c not in "01":
                    b = b.replace(c, "0")
                    break
        return int(a) - int(b)
```

#### Java

```java
class Solution {
    public int maxDiff(int num) {
        String a = String.valueOf(num);
        String b = a;
        for (int i = 0; i < a.length(); ++i) {
            if (a.charAt(i) != '9') {
                a = a.replace(a.charAt(i), '9');
                break;
            }
        }
        if (b.charAt(0) != '1') {
            b = b.replace(b.charAt(0), '1');
        } else {
            for (int i = 1; i < b.length(); ++i) {
                if (b.charAt(i) != '0' && b.charAt(i) != '1') {
                    b = b.replace(b.charAt(i), '0');
                    break;
                }
            }
        }
        return Integer.parseInt(a) - Integer.parseInt(b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxDiff(int num) {
        auto replace = [](string& s, char a, char b) {
            for (auto& c : s) {
                if (c == a) {
                    c = b;
                }
            }
        };
        string a = to_string(num);
        string b = a;
        for (int i = 0; i < a.size(); ++i) {
            if (a[i] != '9') {
                replace(a, a[i], '9');
                break;
            }
        }
        if (b[0] != '1') {
            replace(b, b[0], '1');
        } else {
            for (int i = 1; i < b.size(); ++i) {
                if (b[i] != '0' && b[i] != '1') {
                    replace(b, b[i], '0');
                    break;
                }
            }
        }
        return stoi(a) - stoi(b);
    }
};
```

#### Go

```go
func maxDiff(num int) int {
	a, b := num, num
	s := strconv.Itoa(num)
	for i := range s {
		if s[i] != '9' {
			a, _ = strconv.Atoi(strings.ReplaceAll(s, string(s[i]), "9"))
			break
		}
	}
	if s[0] > '1' {
		b, _ = strconv.Atoi(strings.ReplaceAll(s, string(s[0]), "1"))
	} else {
		for i := 1; i < len(s); i++ {
			if s[i] != '0' && s[i] != '1' {
				b, _ = strconv.Atoi(strings.ReplaceAll(s, string(s[i]), "0"))
				break
			}
		}
	}
	return a - b
}
```

#### TypeScript

```ts
function maxDiff(num: number): number {
    let a = num.toString();
    let b = a;
    for (let i = 0; i < a.length; ++i) {
        if (a[i] !== '9') {
            a = a.split(a[i]).join('9');
            break;
        }
    }
    if (b[0] !== '1') {
        b = b.split(b[0]).join('1');
    } else {
        for (let i = 1; i < b.length; ++i) {
            if (b[i] !== '0' && b[i] !== '1') {
                b = b.split(b[i]).join('0');
                break;
            }
        }
    }
    return +a - +b;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_diff(num: i32) -> i32 {
        let a = num.to_string();
        let mut a = a.clone();
        let mut b = a.clone();

        for c in a.chars() {
            if c != '9' {
                a = a.replace(c, "9");
                break;
            }
        }

        let chars: Vec<char> = b.chars().collect();
        if chars[0] != '1' {
            b = b.replace(chars[0], "1");
        } else {
            for &c in &chars[1..] {
                if c != '0' && c != '1' {
                    b = b.replace(c, "0");
                    break;
                }
            }
        }

        a.parse::<i32>().unwrap() - b.parse::<i32>().unwrap()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
