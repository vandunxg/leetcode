---
comments: true
difficulty: Easy
rating: 1217
source: Weekly Contest 216 Q1
tags:
    - Array
    - String
---

<!-- problem:start -->

# [1662. Check If Two String Arrays are Equivalent](https://leetcode.com/problems/check-if-two-string-arrays-are-equivalent)

[中文文档](/solution/1600-1699/1662.Check%20If%20Two%20String%20Arrays%20are%20Equivalent/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng chuỗi <code>word1</code> và <code>word2</code>, trả về<em> </em><code>true</code><em> nếu hai mảng <strong>biểu diễn</strong> cùng một chuỗi, ngược lại trả về </em><code>false</code><em>.</em></p>

<p>Một chuỗi được <strong>biểu diễn</strong> bởi một mảng nếu các phần tử của mảng khi nối lại <strong>theo thứ tự</strong> tạo thành chuỗi đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> word1 = [&quot;ab&quot;, &quot;c&quot;], word2 = [&quot;a&quot;, &quot;bc&quot;]
<strong>Output:</strong> true
<strong>Explanation:</strong>
word1 represents string &quot;ab&quot; + &quot;c&quot; -&gt; &quot;abc&quot;
word2 represents string &quot;a&quot; + &quot;bc&quot; -&gt; &quot;abc&quot;
Hai chuỗi giống nhau nên trả về true.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> word1 = [&quot;a&quot;, &quot;cb&quot;], word2 = [&quot;ab&quot;, &quot;c&quot;]
<strong>Output:</strong> false
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> word1  = [&quot;abc&quot;, &quot;d&quot;, &quot;defg&quot;], word2 = [&quot;abcddefg&quot;]
<strong>Output:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word1.length, word2.length &lt;= 10<sup>3</sup></code></li>
	<li><code>1 &lt;= word1[i].length, word2[i].length &lt;= 10<sup>3</sup></code></li>
	<li><code>1 &lt;= sum(word1[i].length), sum(word2[i].length) &lt;= 10<sup>3</sup></code></li>
	<li><code>word1[i]</code> and <code>word2[i]</code> consist of lowercase letters.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nối chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Ta kiểm tra xem nối hai mảng có tạo ra cùng một chuỗi hay không. Tổng độ dài nhỏ, nên chỉ cần dùng $\texttt{join}$ rồi so sánh.

<!-- thinking:end -->

Nối các chuỗi trong hai mảng thành hai chuỗi, sau đó so sánh xem hai chuỗi có bằng nhau hay không.

Độ phức tạp thời gian là $O(m)$ và độ phức tạp không gian là $O(m)$, trong đó $m$ là tổng độ dài các chuỗi trong hai mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def arrayStringsAreEqual(self, word1: List[str], word2: List[str]) -> bool:
        return ''.join(word1) == ''.join(word2)
```

#### Java

```java
class Solution {
    public boolean arrayStringsAreEqual(String[] word1, String[] word2) {
        return String.join("", word1).equals(String.join("", word2));
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool arrayStringsAreEqual(vector<string>& word1, vector<string>& word2) {
        return reduce(word1.cbegin(), word1.cend()) == reduce(word2.cbegin(), word2.cend());
    }
};
```

#### Go

```go
func arrayStringsAreEqual(word1 []string, word2 []string) bool {
	return strings.Join(word1, "") == strings.Join(word2, "")
}
```

#### TypeScript

```ts
function arrayStringsAreEqual(word1: string[], word2: string[]): boolean {
    return word1.join('') === word2.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn array_strings_are_equal(word1: Vec<String>, word2: Vec<String>) -> bool {
        word1.join("") == word2.join("")
    }
}
```

#### C

```c
bool arrayStringsAreEqual(char** word1, int word1Size, char** word2, int word2Size) {
    int i = 0;
    int j = 0;
    int x = 0;
    int y = 0;
    while (i < word1Size && j < word2Size) {
        if (word1[i][x++] != word2[j][y++]) {
            return 0;
        }

        if (word1[i][x] == '\0') {
            x = 0;
            i++;
        }
        if (word2[j][y] == '\0') {
            y = 0;
            j++;
        }
    }
    return i == word1Size && j == word2Size;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tạo hai chuỗi mới. Bốn con trỏ duyệt từng ký tự trong hai mảng ban đầu với $O(1)$ không gian phụ: khác nhau thì thất bại, hết một từ thì chuyển sang mảnh tiếp theo.

<!-- thinking:end -->

Ở Lời giải 1, ta nối các chuỗi trong hai mảng thành hai chuỗi mới nên tốn thêm không gian. Ta cũng có thể duyệt trực tiếp hai mảng và so sánh từng ký tự.

Ta dùng hai con trỏ $i$ và $j$ trỏ đến hai mảng chuỗi, cùng hai con trỏ $x$ và $y$ trỏ đến các ký tự tương ứng trong chuỗi. Ban đầu, $i = j = x = y = 0$.

Mỗi lần ta so sánh $word1[i][x]$ và $word2[j][y]$. Nếu chúng khác nhau, ta trả về `false`. Ngược lại, tăng $x$ và $y$ thêm $1$. Nếu $x$ hoặc $y$ đạt độ dài chuỗi tương ứng, tăng con trỏ mảng tương ứng $i$ hoặc $j$ thêm $1$, rồi đặt lại $x$ và $y$ về $0$.

Nếu đã duyệt hết cả hai mảng chuỗi, ta trả về `true`, ngược lại trả về `false`.

Độ phức tạp thời gian là $O(m)$ và độ phức tạp không gian là $O(1)$, trong đó $m$ là tổng độ dài các chuỗi trong hai mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def arrayStringsAreEqual(self, word1: List[str], word2: List[str]) -> bool:
        i = j = x = y = 0
        while i < len(word1) and j < len(word2):
            if word1[i][x] != word2[j][y]:
                return False
            x, y = x + 1, y + 1
            if x == len(word1[i]):
                x, i = 0, i + 1
            if y == len(word2[j]):
                y, j = 0, j + 1
        return i == len(word1) and j == len(word2)
```

#### Java

```java
class Solution {
    public boolean arrayStringsAreEqual(String[] word1, String[] word2) {
        int i = 0, j = 0;
        int x = 0, y = 0;
        while (i < word1.length && j < word2.length) {
            if (word1[i].charAt(x++) != word2[j].charAt(y++)) {
                return false;
            }
            if (x == word1[i].length()) {
                x = 0;
                ++i;
            }
            if (y == word2[j].length()) {
                y = 0;
                ++j;
            }
        }
        return i == word1.length && j == word2.length;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool arrayStringsAreEqual(vector<string>& word1, vector<string>& word2) {
        int i = 0, j = 0, x = 0, y = 0;
        while (i < word1.size() && j < word2.size()) {
            if (word1[i][x++] != word2[j][y++]) return false;
            if (x == word1[i].size()) x = 0, i++;
            if (y == word2[j].size()) y = 0, j++;
        }
        return i == word1.size() && j == word2.size();
    }
};
```

#### Go

```go
func arrayStringsAreEqual(word1 []string, word2 []string) bool {
	var i, j, x, y int
	for i < len(word1) && j < len(word2) {
		if word1[i][x] != word2[j][y] {
			return false
		}
		x, y = x+1, y+1
		if x == len(word1[i]) {
			x, i = 0, i+1
		}
		if y == len(word2[j]) {
			y, j = 0, j+1
		}
	}
	return i == len(word1) && j == len(word2)
}
```

#### TypeScript

```ts
function arrayStringsAreEqual(word1: string[], word2: string[]): boolean {
    let [i, j, x, y] = [0, 0, 0, 0];
    while (i < word1.length && j < word2.length) {
        if (word1[i][x++] !== word2[j][y++]) {
            return false;
        }
        if (x === word1[i].length) {
            x = 0;
            ++i;
        }
        if (y === word2[j].length) {
            y = 0;
            ++j;
        }
    }
    return i === word1.length && j === word2.length;
}
```

#### Rust

```rust
impl Solution {
    pub fn array_strings_are_equal(word1: Vec<String>, word2: Vec<String>) -> bool {
        let (n, m) = (word1.len(), word2.len());
        let (mut i, mut j, mut x, mut y) = (0, 0, 0, 0);
        while i < n && j < m {
            if word1[i].as_bytes()[x] != word2[j].as_bytes()[y] {
                return false;
            }
            x += 1;
            y += 1;
            if x == word1[i].len() {
                x = 0;
                i += 1;
            }
            if y == word2[j].len() {
                y = 0;
                j += 1;
            }
        }
        i == n && j == m
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
