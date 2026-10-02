---
comments: true
difficulty: Medium
rating: 1868
source: Weekly Contest 210 Q3
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [1616. Split Two Strings to Make Palindrome](https://leetcode.com/problems/split-two-strings-to-make-palindrome)

[中文文档](/solution/1600-1699/1616.Split%20Two%20Strings%20to%20Make%20Palindrome/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>a</code> và <code>b</code> có cùng độ dài. Chọn một chỉ số và chia cả hai chuỗi <strong>tại cùng chỉ số đó</strong>: <code>a</code> thành <code>a<sub>prefix</sub></code> và <code>a<sub>suffix</sub></code> với <code>a = a<sub>prefix</sub> + a<sub>suffix</sub></code>, còn <code>b</code> thành <code>b<sub>prefix</sub></code> và <code>b<sub>suffix</sub></code> với <code>b = b<sub>prefix</sub> + b<sub>suffix</sub></code>. Kiểm tra xem <code>a<sub>prefix</sub> + b<sub>suffix</sub></code> hoặc <code>b<sub>prefix</sub> + a<sub>suffix</sub></code> có tạo thành palindrome hay không.</p>

<p>Khi chia chuỗi <code>s</code> thành <code>s<sub>prefix</sub></code> và <code>s<sub>suffix</sub></code>, một trong hai phần có thể rỗng. Ví dụ, nếu <code>s = &quot;abc&quot;</code> thì <code>&quot;&quot; + &quot;abc&quot;</code>, <code>&quot;a&quot; + &quot;bc&quot;</code>, <code>&quot;ab&quot; + &quot;c&quot;</code> và <code>&quot;abc&quot; + &quot;&quot;</code> đều là cách chia hợp lệ. Các phần <code>s<sub>prefix</sub></code> và <code>s<sub>suffix</sub></code> được giữ nguyên trong phép chia.</p>

<p>Trả về <code>true</code><em> nếu có thể tạo thành</em><em> chuỗi palindrome, ngược lại trả về </em><code>false</code>.</p>

<p><strong>Lưu ý</strong> rằng&nbsp;<code>x + y</code> biểu thị phép nối chuỗi <code>x</code> và <code>y</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> a = &quot;x&quot;, b = &quot;y&quot;
<strong>Output:</strong> true
<strong>Giải thích:</strong> Nếu a hoặc b là palindrome thì đáp án là true vì có thể chia như sau:
a<sub>prefix</sub> = &quot;&quot;, a<sub>suffix</sub> = &quot;x&quot;
b<sub>prefix</sub> = &quot;&quot;, b<sub>suffix</sub> = &quot;y&quot;
Then, a<sub>prefix</sub> + b<sub>suffix</sub> = &quot;&quot; + &quot;y&quot; = &quot;y&quot;, which is a palindrome.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> a = &quot;xbdef&quot;, b = &quot;xecab&quot;
<strong>Output:</strong> false
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> a = &quot;ulacfd&quot;, b = &quot;jizalu&quot;
<strong>Output:</strong> true
<strong>Giải thích:</strong> Chia chúng tại chỉ số 3:
a<sub>prefix</sub> = &quot;ula&quot;, a<sub>suffix</sub> = &quot;cfd&quot;
b<sub>prefix</sub> = &quot;jiz&quot;, b<sub>suffix</sub> = &quot;alu&quot;
Then, a<sub>prefix</sub> + b<sub>suffix</sub> = &quot;ula&quot; + &quot;alu&quot; = &quot;ulaalu&quot;, which is a palindrome.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= a.length, b.length &lt;= 10<sup>5</sup></code></li>
	<li><code>a.length == b.length</code></li>
	<li><code>a</code> and <code>b</code> consist of lowercase English letters</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Có $n$ vị trí chia và $n$ có thể bằng $10^5$, nên không thể kiểm tra lại palindrome từ đầu tại mỗi vị trí.
>
> Ghép phần đầu của $a$ với phần cuối của $b$ lâu nhất có thể; phần giữa còn lại phải là palindrome và hoàn toàn thuộc về $a$ hoặc hoàn toàn thuộc về $b$. Sau đó đổi chỗ hai chuỗi và lặp lại.
>
> Hai con trỏ thu hẹp khi hai đầu khớp nhau; sau đó kiểm tra $a[i..j]$ hoặc $b[i..j]$ có phải palindrome hay không. Một trong hai hướng ghép có thể thành công.

<!-- thinking:end -->

Ta dùng hai con trỏ: con trỏ $i$ bắt đầu ở đầu chuỗi $a$, còn $j$ bắt đầu ở cuối chuỗi $b$. Nếu hai ký tự được trỏ tới bằng nhau, cả hai con trỏ tiến về giữa cho đến khi gặp ký tự khác nhau hoặc vượt qua nhau.

Nếu hai con trỏ vượt qua nhau, tức là $i \geq j$, phần $prefix$ và $suffix$ đã có thể tạo thành palindrome nên ta trả về `true`. Nếu không, cần kiểm tra $a[i,...j]$ hoặc $b[i,...j]$ có phải palindrome hay không; nếu đúng, trả về `true`.

Nếu không, ta đổi chỗ hai chuỗi $a$ và $b$ rồi lặp lại quy trình.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của chuỗi $a$ hoặc $b$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkPalindromeFormation(self, a: str, b: str) -> bool:
        def check1(a: str, b: str) -> bool:
            i, j = 0, len(b) - 1
            while i < j and a[i] == b[j]:
                i, j = i + 1, j - 1
            return i >= j or check2(a, i, j) or check2(b, i, j)

        def check2(a: str, i: int, j: int) -> bool:
            return a[i : j + 1] == a[i : j + 1][::-1]

        return check1(a, b) or check1(b, a)
```

#### Java

```java
class Solution {
    public boolean checkPalindromeFormation(String a, String b) {
        return check1(a, b) || check1(b, a);
    }

    private boolean check1(String a, String b) {
        int i = 0;
        int j = b.length() - 1;
        while (i < j && a.charAt(i) == b.charAt(j)) {
            i++;
            j--;
        }
        return i >= j || check2(a, i, j) || check2(b, i, j);
    }

    private boolean check2(String a, int i, int j) {
        while (i < j && a.charAt(i) == a.charAt(j)) {
            i++;
            j--;
        }
        return i >= j;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkPalindromeFormation(string a, string b) {
        return check1(a, b) || check1(b, a);
    }

private:
    bool check1(string& a, string& b) {
        int i = 0, j = b.size() - 1;
        while (i < j && a[i] == b[j]) {
            ++i;
            --j;
        }
        return i >= j || check2(a, i, j) || check2(b, i, j);
    }

    bool check2(string& a, int i, int j) {
        while (i <= j && a[i] == a[j]) {
            ++i;
            --j;
        }
        return i >= j;
    }
};
```

#### Go

```go
func checkPalindromeFormation(a string, b string) bool {
	return check1(a, b) || check1(b, a)
}

func check1(a, b string) bool {
	i, j := 0, len(b)-1
	for i < j && a[i] == b[j] {
		i++
		j--
	}
	return i >= j || check2(a, i, j) || check2(b, i, j)
}

func check2(a string, i, j int) bool {
	for i < j && a[i] == a[j] {
		i++
		j--
	}
	return i >= j
}
```

#### TypeScript

```ts
function checkPalindromeFormation(a: string, b: string): boolean {
    const check1 = (a: string, b: string) => {
        let i = 0;
        let j = b.length - 1;
        while (i < j && a.charAt(i) === b.charAt(j)) {
            i++;
            j--;
        }
        return i >= j || check2(a, i, j) || check2(b, i, j);
    };

    const check2 = (a: string, i: number, j: number) => {
        while (i < j && a.charAt(i) === a.charAt(j)) {
            i++;
            j--;
        }
        return i >= j;
    };
    return check1(a, b) || check1(b, a);
}
```

#### Rust

```rust
impl Solution {
    pub fn check_palindrome_formation(a: String, b: String) -> bool {
        fn check1(a: &[u8], b: &[u8]) -> bool {
            let (mut i, mut j) = (0, b.len() - 1);
            while i < j && a[i] == b[j] {
                i += 1;
                j -= 1;
            }
            if i >= j {
                return true;
            }
            check2(a, i, j) || check2(b, i, j)
        }

        fn check2(a: &[u8], mut i: usize, mut j: usize) -> bool {
            while i < j && a[i] == a[j] {
                i += 1;
                j -= 1;
            }
            i >= j
        }

        let a = a.as_bytes();
        let b = b.as_bytes();
        check1(a, b) || check1(b, a)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
