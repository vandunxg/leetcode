---
comments: true
difficulty: Easy
rating: 1333
source: Weekly Contest 234 Q1
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [1805. Number of Different Integers in a String](https://leetcode.com/problems/number-of-different-integers-in-a-string)

[中文文档](/solution/1800-1899/1805.Number%20of%20Different%20Integers%20in%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>word</code> chỉ gồm các chữ số và chữ cái tiếng Anh viết thường.</p>

<p>Ta thay mọi ký tự không phải chữ số bằng một khoảng trắng. Ví dụ, <code>&quot;a123bc34d8ef34&quot;</code> sẽ trở thành <code>&quot; 123&nbsp; 34 8&nbsp; 34&quot;</code>. Khi đó còn lại các số nguyên được ngăn cách bởi ít nhất một khoảng trắng: <code>&quot;123&quot;</code>, <code>&quot;34&quot;</code>, <code>&quot;8&quot;</code> và <code>&quot;34&quot;</code>.</p>

<p>Hãy trả về <em>số lượng số nguyên <strong>khác nhau</strong> sau khi thực hiện các phép thay thế trên </em><code>word</code>.</p>

<p>Hai số nguyên được xem là khác nhau nếu biểu diễn thập phân <strong>không có các số 0 ở đầu</strong> của chúng khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;a<u>123</u>bc<u>34</u>d<u>8</u>ef<u>34</u>&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Ba số nguyên khác nhau là &quot;123&quot;, &quot;34&quot; và &quot;8&quot;. Lưu ý rằng &quot;34&quot; chỉ được đếm một lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;leet<u>1234</u>code<u>234</u>&quot;
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;a<u>1</u>b<u>01</u>c<u>001</u>&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích: </strong>Ba số nguyên &quot;1&quot;, &quot;01&quot; và &quot;001&quot; đều biểu diễn cùng một số nguyên vì
các số 0 ở đầu bị bỏ qua khi so sánh giá trị thập phân của chúng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 1000</code></li>
	<li><code>word</code> gồm các chữ số và chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải đếm các số nguyên khác nhau được tạo bởi các đoạn chữ số, trong đó các giá trị có số 0 ở đầu được xem là giống nhau. Việc chuyển từng đoạn thành số nguyên có thể gây tràn số vì một đoạn có thể dài bằng cả chuỗi.
>
> Hai con trỏ tách từng đoạn chữ số liên tiếp, bỏ qua các số 0 ở đầu rồi chèn phần chuỗi còn lại (rỗng nếu cả đoạn chỉ gồm số 0) vào hash set. Kích thước của set là số lượng số nguyên khác nhau, và ta không bao giờ cần chuyển token thành kiểu số.

<!-- thinking:end -->

Duyệt chuỗi `word`, tìm vị trí bắt đầu và kết thúc của từng số nguyên, cắt lấy chuỗi con đó rồi lưu vào hash set $s$.

Sau khi duyệt xong, trả về kích thước của hash set $s$.

> Lưu ý, số nguyên được biểu diễn bởi mỗi chuỗi con có thể rất lớn nên ta không thể chuyển trực tiếp nó thành số nguyên. Vì vậy, có thể xóa các số 0 ở đầu của mỗi chuỗi con trước khi lưu vào hash set.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài chuỗi `word`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numDifferentIntegers(self, word: str) -> int:
        s = set()
        i, n = 0, len(word)
        while i < n:
            if word[i].isdigit():
                while i < n and word[i] == '0':
                    i += 1
                j = i
                while j < n and word[j].isdigit():
                    j += 1
                s.add(word[i:j])
                i = j
            i += 1
        return len(s)
```

#### Java

```java
class Solution {
    public int numDifferentIntegers(String word) {
        Set<String> s = new HashSet<>();
        int n = word.length();
        for (int i = 0; i < n; ++i) {
            if (Character.isDigit(word.charAt(i))) {
                while (i < n && word.charAt(i) == '0') {
                    ++i;
                }
                int j = i;
                while (j < n && Character.isDigit(word.charAt(j))) {
                    ++j;
                }
                s.add(word.substring(i, j));
                i = j;
            }
        }
        return s.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numDifferentIntegers(string word) {
        unordered_set<string> s;
        int n = word.size();
        for (int i = 0; i < n; ++i) {
            if (isdigit(word[i])) {
                while (i < n && word[i] == '0') ++i;
                int j = i;
                while (j < n && isdigit(word[j])) ++j;
                s.insert(word.substr(i, j - i));
                i = j;
            }
        }
        return s.size();
    }
};
```

#### Go

```go
func numDifferentIntegers(word string) int {
	s := map[string]struct{}{}
	n := len(word)
	for i := 0; i < n; i++ {
		if word[i] >= '0' && word[i] <= '9' {
			for i < n && word[i] == '0' {
				i++
			}
			j := i
			for j < n && word[j] >= '0' && word[j] <= '9' {
				j++
			}
			s[word[i:j]] = struct{}{}
			i = j
		}
	}
	return len(s)
}
```

#### TypeScript

```ts
function numDifferentIntegers(word: string): number {
    return new Set(
        word
            .replace(/\D+/g, ' ')
            .trim()
            .split(' ')
            .filter(v => v !== '')
            .map(v => v.replace(/^0+/g, '')),
    ).size;
}
```

#### Rust

```rust
use std::collections::HashSet;
impl Solution {
    pub fn num_different_integers(word: String) -> i32 {
        let s = word.as_bytes();
        let n = s.len();
        let mut set = HashSet::new();
        let mut i = 0;
        while i < n {
            if s[i] >= b'0' && s[i] <= b'9' {
                let mut j = i;
                while j < n && s[j] >= b'0' && s[j] <= b'9' {
                    j += 1;
                }
                while i < j - 1 && s[i] == b'0' {
                    i += 1;
                }
                set.insert(String::from_utf8(s[i..j].to_vec()).unwrap());
                i = j;
            } else {
                i += 1;
            }
        }
        set.len() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
