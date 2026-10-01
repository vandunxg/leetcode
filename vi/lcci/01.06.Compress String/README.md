---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [01.06. Compress String](https://leetcode.cn/problems/compress-string-lcci)

[中文文档](/lcci/01.06.Compress%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy triển khai một method để thực hiện việc nén chuỗi cơ bản bằng cách sử dụng số lần các ký tự lặp lại. Ví dụ, chuỗi aabcccccaaa sẽ trở thành a2blc5a3. Nếu chuỗi &quot;đã nén&quot; không ngắn hơn chuỗi ban đầu, method của bạn phải trả về chuỗi ban đầu. Có thể giả sử chuỗi chỉ chứa các chữ cái viết hoa và viết thường (a - z).</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong>Đầu vào: </strong>&quot;aabcccccaaa&quot;

<strong>Đầu ra: </strong>&quot;a2b1c5a3&quot;

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong>Đầu vào: </strong>&quot;abbccd&quot;

<strong>Đầu ra: </strong>&quot;abbccd&quot;

<strong>Giải thích: </strong>

Chuỗi đã nén là &quot;a1b2c2d1&quot;, dài hơn chuỗi ban đầu.

</pre>

<p><strong>Lưu ý:</strong></p>

- `0 <= S.length <= 50000`

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Dạng nén nối ký tự và độ dài của từng đoạn liên tiếp, và chỉ được giữ lại nếu ngắn hơn. Việc đếm một đoạn từ mọi chỉ số vẫn có độ phức tạp tuyến tính nhưng lặp lại công việc.
>
> Mỗi đoạn liên tiếp cực đại chỉ cần được xử lý một lần, nên bài toán quy về việc xác định ranh giới các đoạn.
>
> `groupby` (hoặc hai con trỏ tường minh) phát ra từng đoạn dưới dạng một ký tự cùng độ dài của nó vào $t$, sau đó trả về chuỗi ngắn hơn giữa $S$ và $t$. Mỗi ký tự được duyệt đúng một lần, tương ứng với cách nhóm bằng hai con trỏ được mô tả trong phần trình bày.

<!-- thinking:end -->

Ta có thể sử dụng hai con trỏ để tìm vị trí bắt đầu và kết thúc của mỗi đoạn ký tự liên tiếp, tính độ dài của đoạn đó, rồi nối ký tự và độ dài vào chuỗi $t$.

Cuối cùng, chúng ta so sánh độ dài của $t$ và $S$. Nếu độ dài của $t$ nhỏ hơn $S$, ta trả về $t$; nếu không, ta trả về $S$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def compressString(self, S: str) -> str:
        t = "".join(a + str(len(list(b))) for a, b in groupby(S))
        return min(S, t, key=len)
```

#### Java

```java
class Solution {
    public String compressString(String S) {
        int n = S.length();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && S.charAt(j) == S.charAt(i)) {
                ++j;
            }
            sb.append(S.charAt(i)).append(j - i);
            i = j;
        }
        String t = sb.toString();
        return t.length() < n ? t : S;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string compressString(string S) {
        int n = S.size();
        string t;
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && S[j] == S[i]) {
                ++j;
            }
            t += S[i];
            t += to_string(j - i);
            i = j;
        }
        return t.size() < n ? t : S;
    }
};
```

#### Go

```go
func compressString(S string) string {
	n := len(S)
	sb := strings.Builder{}
	for i := 0; i < n; {
		j := i + 1
		for j < n && S[j] == S[i] {
			j++
		}
		sb.WriteByte(S[i])
		sb.WriteString(strconv.Itoa(j - i))
		i = j
	}
	if t := sb.String(); len(t) < n {
		return t
	}
	return S
}
```

#### Rust

```rust
impl Solution {
    pub fn compress_string(s: String) -> String {
        let mut cs: Vec<char> = s.chars().collect();
        let mut t = Vec::new();
        let mut i = 0;
        let n = s.len();
        while i < n {
            let mut j = i + 1;
            while j < n && cs[j] == cs[i] {
                j += 1;
            }
            t.push(cs[i]);
            t.extend((j - i).to_string().chars());
            i = j;
        }

        let t = t.into_iter().collect::<String>();
        if s.len() <= t.len() {
            s
        } else {
            t
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {string} S
 * @return {string}
 */
var compressString = function (S) {
    const n = S.length;
    const t = [];
    for (let i = 0; i < n;) {
        let j = i + 1;
        while (j < n && S.charAt(j) === S.charAt(i)) {
            ++j;
        }
        t.push(S.charAt(i), j - i);
        i = j;
    }
    return t.length < n ? t.join('') : S;
};
```

#### Swift

```swift
class Solution {
    func compressString(_ S: String) -> String {
        let n = S.count
        var compressed = ""
        var i = 0

        while i < n {
            var j = i
            let currentChar = S[S.index(S.startIndex, offsetBy: i)]
            while j < n && S[S.index(S.startIndex, offsetBy: j)] == currentChar {
                j += 1
            }
            compressed += "\(currentChar)\(j - i)"
            i = j
        }

        return compressed.count < n ? compressed : S
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
