---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [01.03. String to URL](https://leetcode.cn/problems/string-to-url-lcci)

[Tài liệu tiếng Trung](/lcci/01.03.String%20to%20URL/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một method để thay thế tất cả khoảng trắng trong một chuỗi bằng &#39;%20&#39;. Bạn có thể giả sử rằng ở cuối chuỗi có đủ không gian để chứa các ký tự bổ sung, đồng thời bạn được cung cấp độ dài &quot;thực&quot; của chuỗi. (Lưu ý: Nếu triển khai bằng Java, hãy sử dụng một mảng ký tự để có thể thực hiện thao tác này in-place.)</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong>Đầu vào: </strong>&quot;Mr John Smith &quot;, 13

<strong>Đầu ra: </strong>&quot;Mr%20John%20Smith&quot;

<strong>Giải thích: </strong>

Các số còn thiếu là [5,6,8,...], do đó số còn thiếu thứ ba là 8.

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong>Đầu vào: </strong>&quot;               &quot;, 5

<strong>Đầu ra: </strong>&quot;%20%20%20%20%20&quot;

</pre>

<p>&nbsp;</p>

<p><strong>Lưu ý:</strong></p>

<ol>
	<li><code>0 &lt;= S.length &lt;= 500000</code></li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sử dụng hàm `replace()`

<!-- thinking:start -->

> **Tư duy**
>
> Các khoảng trắng trong $length$ ký tự đầu tiên phải trở thành `%20`. $S$ có thể chứa phần đệm ở cuối, vì vậy gọi replace trên toàn bộ chuỗi là sai.
>
> Hàm `replace` của thư viện hoàn tất việc thay thế trong thời gian tuyến tính. Trước tiên cắt $S[:length]$, sau đó thay thế các khoảng trắng, nhờ đó xử lý cả phần tiền tố cần dùng và phần đệm.
>
> Điều này khớp với code Python: một lần cắt và một lần replace, với độ phức tạp tuyến tính theo độ dài đầu ra.

<!-- thinking:end -->

Trực tiếp sử dụng `replace` để thay thế mọi ` ` bằng `%20`:

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def replaceSpaces(self, S: str, length: int) -> str:
        return S[:length].replace(' ', '%20')
```

#### TypeScript

```ts
function replaceSpaces(S: string, length: number): string {
    return S.slice(0, length).replace(/\s/g, '%20');
}
```

#### Rust

```rust
impl Solution {
    pub fn replace_spaces(s: String, length: i32) -> String {
        s[..length as usize].replace(' ', "%20")
    }
}
```

#### JavaScript

```js
/**
 * @param {string} S
 * @param {number} length
 * @return {string}
 */
var replaceSpaces = function (S, length) {
    return encodeURI(S.substring(0, length));
};
```

#### Swift

```swift
class Solution {
    func replaceSpaces(_ S: String, _ length: Int) -> String {
        let substring = S.prefix(length)
        var result = ""

        for character in substring {
            if character == " " {
                result += "%20"
            } else {
                result.append(character)
            }
        }

        return result
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một helper của thư viện không portable giữa các ngôn ngữ mà bài toán có thể được giải bằng.
>
> Duyệt từng ký tự trong phần tiền tố hợp lệ: ghi `%20` khi gặp khoảng trắng, nếu không thì ghi ký tự ban đầu. Buffer dài tối đa gấp ba lần, vẫn chỉ cần một lượt duyệt tuyến tính.

<!-- thinking:end -->

Duyệt từng ký tự $c$ trong chuỗi. Khi gặp khoảng trắng, thêm `%20` vào kết quả; nếu không thì thêm $c$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def replaceSpaces(self, S: str, length: int) -> str:
        return ''.join(['%20' if c == ' ' else c for c in S[:length]])
```

#### Java

```java
class Solution {
    public String replaceSpaces(String S, int length) {
        char[] cs = S.toCharArray();
        int j = cs.length;
        for (int i = length - 1; i >= 0; --i) {
            if (cs[i] == ' ') {
                cs[--j] = '0';
                cs[--j] = '2';
                cs[--j] = '%';
            } else {
                cs[--j] = cs[i];
            }
        }
        return new String(cs, j, cs.length - j);
    }
}
```

#### Go

```go
func replaceSpaces(S string, length int) string {
	// return url.PathEscape(S[:length])
	j := len(S)
	b := []byte(S)
	for i := length - 1; i >= 0; i-- {
		if b[i] == ' ' {
			b[j-1] = '0'
			b[j-2] = '2'
			b[j-3] = '%'
			j -= 3
		} else {
			b[j-1] = b[i]
			j--
		}
	}
	return string(b[j:])
}
```

#### Rust

```rust
impl Solution {
    pub fn replace_spaces(s: String, length: i32) -> String {
        s.chars()
            .take(length as usize)
            .map(|c| {
                if c == ' ' {
                    "%20".to_string()
                } else {
                    c.to_string()
                }
            })
            .collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
