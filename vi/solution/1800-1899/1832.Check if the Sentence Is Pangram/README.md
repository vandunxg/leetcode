---
comments: true
difficulty: Easy
rating: 1166
source: Weekly Contest 237 Q1
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [1832. Check if the Sentence Is Pangram](https://leetcode.com/problems/check-if-the-sentence-is-pangram)

[中文文档](/solution/1800-1899/1832.Check%20if%20the%20Sentence%20Is%20Pangram/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Pangram</strong> là một câu trong đó mỗi chữ cái của bảng chữ cái tiếng Anh xuất hiện ít nhất một lần.</p>

<p>Cho một chuỗi <code>sentence</code> chỉ gồm các chữ cái tiếng Anh viết thường, trả về<em> </em><code>true</code><em> nếu </em><code>sentence</code><em> là một <strong>pangram</strong>, ngược lại trả về </em><code>false</code><em> nếu không.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;thequickbrownfoxjumpsoverthelazydog&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> sentence chứa ít nhất một lần mỗi chữ cái của bảng chữ cái tiếng Anh.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;leetcode&quot;
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= sentence.length &lt;= 1000</code></li>
	<li><code>sentence</code> gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hoặc Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần kiểm tra xem mọi chữ cái viết thường có xuất hiện hay không. Chỉ cần duyệt tuyến tính vì ta chỉ quan tâm đến tập các chữ cái đã xuất hiện.
>
> Thêm các ký tự vào một tập hợp rồi kiểm tra xem kích thước của tập có bằng $26$ hay không.

<!-- thinking:end -->

Ta duyệt chuỗi `sentence`, dùng một mảng hoặc hash table để ghi nhận các chữ cái đã xuất hiện, rồi kiểm tra xem mảng hoặc hash table có chứa đủ $26$ chữ cái hay không.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(C)$, trong đó $n$ là độ dài chuỗi `sentence`, còn $C$ là kích thước của tập ký tự. Trong bài này, $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkIfPangram(self, sentence: str) -> bool:
        return len(set(sentence)) == 26
```

#### Java

```java
class Solution {
    public boolean checkIfPangram(String sentence) {
        boolean[] vis = new boolean[26];
        for (int i = 0; i < sentence.length(); ++i) {
            vis[sentence.charAt(i) - 'a'] = true;
        }
        for (boolean v : vis) {
            if (!v) {
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
    bool checkIfPangram(string sentence) {
        int vis[26] = {0};
        for (char& c : sentence) vis[c - 'a'] = 1;
        for (int& v : vis)
            if (!v) return false;
        return true;
    }
};
```

#### Go

```go
func checkIfPangram(sentence string) bool {
	vis := [26]bool{}
	for _, c := range sentence {
		vis[c-'a'] = true
	}
	for _, v := range vis {
		if !v {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function checkIfPangram(sentence: string): boolean {
    const vis = new Array(26).fill(false);
    for (const c of sentence) {
        vis[c.charCodeAt(0) - 'a'.charCodeAt(0)] = true;
    }
    return vis.every(v => v);
}
```

#### Rust

```rust
impl Solution {
    pub fn check_if_pangram(sentence: String) -> bool {
        let mut vis = [false; 26];
        for c in sentence.as_bytes() {
            vis[(*c - b'a') as usize] = true;
        }
        vis.iter().all(|v| *v)
    }
}
```

#### C

```c
bool checkIfPangram(char* sentence) {
    int vis[26] = {0};
    for (int i = 0; sentence[i]; i++) {
        vis[sentence[i] - 'a'] = 1;
    }
    for (int i = 0; i < 26; i++) {
        if (!vis[i]) {
            return 0;
        }
    }
    return 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 lưu một hash set. Vì chỉ có $26$ chữ cái, bit $i$ của số nguyên $mask$ có thể đánh dấu chữ cái thứ $i$. Câu là pangram khi và chỉ khi $mask=2^{26}-1$, đồng thời chỉ dùng thêm không gian hằng số.

<!-- thinking:end -->

Ta cũng có thể dùng một số nguyên $mask$ để ghi nhận các chữ cái đã xuất hiện, trong đó bit thứ $i$ của $mask$ cho biết chữ cái thứ $i$ đã xuất hiện hay chưa.

Cuối cùng, kiểm tra biểu diễn nhị phân của $mask$ có đủ $26$ bit $1$ hay không, tức là kiểm tra xem $mask$ có bằng $2^{26} - 1$ hay không. Nếu có, trả về `true`, ngược lại trả về `false`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi `sentence`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkIfPangram(self, sentence: str) -> bool:
        mask = 0
        for c in sentence:
            mask |= 1 << (ord(c) - ord('a'))
        return mask == (1 << 26) - 1
```

#### Java

```java
class Solution {
    public boolean checkIfPangram(String sentence) {
        int mask = 0;
        for (int i = 0; i < sentence.length(); ++i) {
            mask |= 1 << (sentence.charAt(i) - 'a');
        }
        return mask == (1 << 26) - 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkIfPangram(string sentence) {
        int mask = 0;
        for (char& c : sentence) mask |= 1 << (c - 'a');
        return mask == (1 << 26) - 1;
    }
};
```

#### Go

```go
func checkIfPangram(sentence string) bool {
	mask := 0
	for _, c := range sentence {
		mask |= 1 << int(c-'a')
	}
	return mask == 1<<26-1
}
```

#### TypeScript

```ts
function checkIfPangram(sentence: string): boolean {
    let mark = 0;
    for (const c of sentence) {
        mark |= 1 << (c.charCodeAt(0) - 'a'.charCodeAt(0));
    }
    return mark === (1 << 26) - 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn check_if_pangram(sentence: String) -> bool {
        let mut mark = 0;
        for c in sentence.as_bytes() {
            mark |= 1 << (*c - b'a');
        }
        mark == (1 << 26) - 1
    }
}
```

#### C

```c
bool checkIfPangram(char* sentence) {
    int mark = 0;
    for (int i = 0; sentence[i]; i++) {
        mark |= 1 << (sentence[i] - 'a');
    }
    return mark == (1 << 26) - 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
