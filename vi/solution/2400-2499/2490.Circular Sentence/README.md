---
comments: true
difficulty: Easy
rating: 1262
source: Weekly Contest 322 Q1
tags:
    - String
---

<!-- problem:start -->

# [2490. Circular Sentence](https://leetcode.com/problems/circular-sentence)

[中文文档](/solution/2400-2499/2490.Circular%20Sentence/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>câu</strong> là một danh sách các từ được phân tách bằng<strong> một</strong> dấu cách duy nhất, không có dấu cách ở đầu hoặc cuối.</p>

<ul>
	<li>Ví dụ, <code>&quot;Hello World&quot;</code>, <code>&quot;HELLO&quot;</code>, <code>&quot;hello world hello world&quot;</code> đều là các câu.</li>
</ul>

<p>Các từ <strong>chỉ</strong> gồm các chữ cái tiếng Anh viết hoa và viết thường. Chữ cái tiếng Anh viết hoa và viết thường được xem là khác nhau.</p>

<p>Một câu là <strong>câu tuần hoàn</strong> nếu:</p>

<ul>
	<li>Ký tự cuối của mỗi từ trong câu bằng ký tự đầu của từ tiếp theo.</li>
	<li>Ký tự cuối của từ cuối cùng bằng ký tự đầu của từ đầu tiên.</li>
</ul>

<p>Ví dụ, <code>&quot;leetcode exercises sound delightful&quot;</code>, <code>&quot;eetcode&quot;</code>, <code>&quot;leetcode eats soul&quot; </code> đều là các câu tuần hoàn. Tuy nhiên, <code>&quot;Leetcode is cool&quot;</code>, <code>&quot;happy Leetcode&quot;</code>, <code>&quot;Leetcode&quot;</code> và <code>&quot;I like Leetcode&quot;</code> <strong>không</strong> phải là các câu tuần hoàn.</p>

<p>Cho một chuỗi <code>sentence</code>, hãy trả về <code>true</code><em> nếu đó là câu tuần hoàn</em>. Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;leetcode exercises sound delightful&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Các từ trong sentence là [&quot;leetcode&quot;, &quot;exercises&quot;, &quot;sound&quot;, &quot;delightful&quot;].
- Ký tự cuối của leetcod<u>e</u> bằng ký tự đầu của <u>e</u>xercises.
- Ký tự cuối của exercise<u>s</u> bằng ký tự đầu của <u>s</u>ound.
- Ký tự cuối của soun<u>d</u> bằng ký tự đầu của <u>d</u>elightful.
- Ký tự cuối của delightfu<u>l</u> bằng ký tự đầu của <u>l</u>eetcode.
Câu này là câu tuần hoàn.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;eetcode&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Từ trong sentence là [&quot;eetcode&quot;].
- Ký tự cuối của eetcod<u>e</u> bằng ký tự đầu của <u>e</u>etcode.
Câu này là câu tuần hoàn.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;Leetcode is cool&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Các từ trong sentence là [&quot;Leetcode&quot;, &quot;is&quot;, &quot;cool&quot;].
- Ký tự cuối của Leetcod<u>e</u> <strong>không</strong> bằng ký tự đầu của <u>i</u>s.
Câu này <strong>không phải</strong> là câu tuần hoàn.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= sentence.length &lt;= 500</code></li>
	<li><code>sentence</code> chỉ gồm các chữ cái tiếng Anh viết thường, viết hoa và dấu cách.</li>
	<li>Các từ trong <code>sentence</code> được phân tách bằng một dấu cách duy nhất.</li>
	<li>Không có dấu cách ở đầu hoặc cuối.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một câu tuần hoàn cần các từ liền kề có chung ký tự nối, đồng thời từ cuối cùng phải nối với từ đầu tiên. Tách chuỗi theo dấu cách rồi kiểm tra $s[-1]$ với ký tự đầu của từ tiếp theo, trong đó vị trí cuối cùng quay vòng về vị trí đầu tiên.

<!-- thinking:end -->

Ta tách chuỗi thành các từ theo dấu cách, sau đó kiểm tra xem ký tự cuối của mỗi từ có bằng ký tự đầu của từ tiếp theo hay không. Nếu không bằng, trả về `false`. Ngược lại, sau khi duyệt qua tất cả các từ, trả về `true`.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isCircularSentence(self, sentence: str) -> bool:
        ss = sentence.split()
        n = len(ss)
        return all(s[-1] == ss[(i + 1) % n][0] for i, s in enumerate(ss))
```

#### Java

```java
class Solution {
    public boolean isCircularSentence(String sentence) {
        var ss = sentence.split(" ");
        int n = ss.length;
        for (int i = 0; i < n; ++i) {
            if (ss[i].charAt(ss[i].length() - 1) != ss[(i + 1) % n].charAt(0)) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isCircularSentence(string sentence) {
        auto ss = split(sentence, ' ');
        int n = ss.size();
        for (int i = 0; i < n; ++i) {
            if (ss[i].back() != ss[(i + 1) % n][0]) {
                return false;
            }
        }
        return true;
    }

    vector<string> split(string& s, char delim) {
        stringstream ss(s);
        string item;
        vector<string> res;
        while (getline(ss, item, delim)) {
            res.emplace_back(item);
        }
        return res;
    }
};
```

#### Go

```go
func isCircularSentence(sentence string) bool {
	ss := strings.Split(sentence, " ")
	n := len(ss)
	for i, s := range ss {
		if s[len(s)-1] != ss[(i+1)%n][0] {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function isCircularSentence(sentence: string): boolean {
    const ss = sentence.split(' ');
    const n = ss.length;
    for (let i = 0; i < n; ++i) {
        if (ss[i][ss[i].length - 1] !== ss[(i + 1) % n][0]) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_circular_sentence(sentence: String) -> bool {
        let ss: Vec<String> = sentence.split(' ').map(String::from).collect();
        let n = ss.len();
        for i in 0..n {
            if ss[i].as_bytes()[ss[i].len() - 1] != ss[(i + 1) % n].as_bytes()[0] {
                return false;
            }
        }
        return true;
    }
}
```

#### JavaScript

```js
/**
 * @param {string} sentence
 * @return {boolean}
 */
var isCircularSentence = function (sentence) {
    const ss = sentence.split(' ');
    const n = ss.length;
    for (let i = 0; i < n; ++i) {
        if (ss[i][ss[i].length - 1] !== ss[(i + 1) % n][0]) {
            return false;
        }
    }
    return true;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Mô phỏng (Tối ưu bộ nhớ)

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 tạo danh sách các từ. Vì không có dấu cách ở đầu hoặc cuối, $s[0]=s[-1]$ nối hai đầu, còn mỗi dấu cách có các ký tự ở hai bên bằng nhau. Bộ nhớ phụ cần dùng là $O(1)$.

<!-- thinking:end -->

Trước tiên, ta kiểm tra xem ký tự đầu và ký tự cuối của chuỗi có bằng nhau hay không. Nếu không bằng, trả về `false`. Ngược lại, duyệt qua chuỗi. Nếu ký tự hiện tại là dấu cách, kiểm tra xem ký tự trước đó và ký tự tiếp theo có bằng nhau hay không. Nếu không bằng, trả về `false`. Ngược lại, sau khi duyệt qua tất cả các ký tự, trả về `true`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isCircularSentence(self, s: str) -> bool:
        return s[0] == s[-1] and all(
            c != " " or s[i - 1] == s[i + 1] for i, c in enumerate(s)
        )
```

#### Java

```java
class Solution {
    public boolean isCircularSentence(String s) {
        int n = s.length();
        if (s.charAt(0) != s.charAt(n - 1)) {
            return false;
        }
        for (int i = 1; i < n; ++i) {
            if (s.charAt(i) == ' ' && s.charAt(i - 1) != s.charAt(i + 1)) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isCircularSentence(string s) {
        int n = s.size();
        if (s[0] != s.back()) {
            return false;
        }
        for (int i = 1; i < n; ++i) {
            if (s[i] == ' ' && s[i - 1] != s[i + 1]) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func isCircularSentence(s string) bool {
	n := len(s)
	if s[0] != s[n-1] {
		return false
	}
	for i := 1; i < n; i++ {
		if s[i] == ' ' && s[i-1] != s[i+1] {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function isCircularSentence(s: string): boolean {
    const n = s.length;
    if (s[0] !== s[n - 1]) {
        return false;
    }
    for (let i = 1; i < n; ++i) {
        if (s[i] === ' ' && s[i - 1] !== s[i + 1]) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_circular_sentence(sentence: String) -> bool {
        let n = sentence.len();
        let chars: Vec<char> = sentence.chars().collect();

        if chars[0] != chars[n - 1] {
            return false;
        }

        for i in 1..n - 1 {
            if chars[i] == ' ' && chars[i - 1] != chars[i + 1] {
                return false;
            }
        }

        true
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var isCircularSentence = function (s) {
    const n = s.length;
    if (s[0] !== s[n - 1]) {
        return false;
    }
    for (let i = 1; i < n; ++i) {
        if (s[i] === ' ' && s[i - 1] !== s[i + 1]) {
            return false;
        }
    }
    return true;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
