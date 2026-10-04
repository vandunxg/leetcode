---
comments: true
difficulty: Medium
rating: 1604
source: Weekly Contest 329 Q3
tags:
    - Bit Manipulation
    - String
---

<!-- problem:start -->

# [2546. Apply Bitwise Operations to Make Strings Equal](https://leetcode.com/problems/apply-bitwise-operations-to-make-strings-equal)

[中文文档](/solution/2500-2599/2546.Apply%20Bitwise%20Operations%20to%20Make%20Strings%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi <strong>nhị phân được đánh chỉ số từ 0</strong> <code>s</code> và <code>target</code> có cùng độ dài <code>n</code>. Bạn có thể thực hiện thao tác sau trên <code>s</code> <strong>bất kỳ</strong> số lần nào:</p>

<ul>
	<li>Chọn hai chỉ số <strong>khác nhau</strong> <code>i</code> và <code>j</code> sao cho <code>0 &lt;= i, j &lt; n</code>.</li>
	<li>Đồng thời thay <code>s[i]</code> bằng (<code>s[i]</code> <strong>OR</strong> <code>s[j]</code>) và thay <code>s[j]</code> bằng (<code>s[i]</code> <strong>XOR</strong> <code>s[j]</code>).</li>
</ul>

<p>Ví dụ, nếu <code>s = &quot;0110&quot;</code>, ta có thể chọn <code>i = 0</code> và <code>j = 2</code>, sau đó đồng thời thay <code>s[0]</code> bằng (<code>s[0]</code> <strong>OR</strong> <code>s[2]</code> = <code>0</code> <strong>OR</strong> <code>1</code> = <code>1</code>) và thay <code>s[2]</code> bằng (<code>s[0]</code> <strong>XOR</strong> <code>s[2]</code> = <code>0</code> <strong>XOR</strong> <code>1</code> = <code>1</code>), khi đó ta được <code>s = &quot;1110&quot;</code>.</p>

<p>Trả về <code>true</code> <em>nếu có thể biến chuỗi </em><code>s</code><em> thành </em><code>target</code><em>, hoặc </em><code>false</code><em> nếu không thể</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1010&quot;, target = &quot;0110&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác sau:
- Chọn i = 2 và j = 0. Khi đó s = &quot;<strong><u>0</u></strong>0<strong><u>1</u></strong>0&quot;.
- Chọn i = 2 và j = 1. Khi đó s = &quot;0<strong><u>11</u></strong>0&quot;.
Vì có thể biến s thành target nên ta trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;11&quot;, target = &quot;00&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể biến s thành target với bất kỳ số lần thực hiện thao tác nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == s.length == target.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> và <code>target</code> chỉ gồm các chữ số <code>0</code> và <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tư duy khác biệt

<!-- thinking:start -->

> **Tư duy**
>
> Các phép ghi được cho phép là $s[i]\lor s[j]$ và $s[i]\oplus s[j]$ vào một trong hai bit. Hai số 0 không thể tạo ra số 1, nhưng chỉ cần có một số 1 thì ta có thể sao chép nó đến bất kỳ vị trí nào hoặc XOR nó với một bit 0 để tạo thành 1.
>
> Vì vậy, $s$ và $\textit{target}$ có thể biến đổi qua lại khi và chỉ khi cả hai cùng chứa $1$ hoặc cùng không chứa số nào.

<!-- thinking:end -->

Ta nhận thấy rằng $1$ thực chất là một “công cụ” để biến đổi chuỗi. Vì vậy, chỉ cần cả hai chuỗi đều có $1$ hoặc đều không có $1$ thì ta có thể thực hiện các thao tác để biến chúng thành hai chuỗi giống nhau.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makeStringsEqual(self, s: str, target: str) -> bool:
        return ("1" in s) == ("1" in target)
```

#### Java

```java
class Solution {
    public boolean makeStringsEqual(String s, String target) {
        return s.contains("1") == target.contains("1");
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool makeStringsEqual(string s, string target) {
        auto a = count(s.begin(), s.end(), '1') > 0;
        auto b = count(target.begin(), target.end(), '1') > 0;
        return a == b;
    }
};
```

#### Go

```go
func makeStringsEqual(s string, target string) bool {
	return strings.Contains(s, "1") == strings.Contains(target, "1")
}
```

#### TypeScript

```ts
function makeStringsEqual(s: string, target: string): boolean {
    return s.includes('1') === target.includes('1');
}
```

#### Rust

```rust
impl Solution {
    pub fn make_strings_equal(s: String, target: String) -> bool {
        s.contains('1') == target.contains('1')
    }
}
```

#### C

```c
bool makeStringsEqual(char* s, char* target) {
    int count = 0;
    for (int i = 0; s[i]; i++) {
        if (s[i] == '1') {
            count++;
            break;
        }
    }
    for (int i = 0; target[i]; i++) {
        if (target[i] == '1') {
            count++;
            break;
        }
    }
    return !(count & 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
