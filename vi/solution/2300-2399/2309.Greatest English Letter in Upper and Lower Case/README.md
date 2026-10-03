---
comments: true
difficulty: Easy
rating: 1242
source: Weekly Contest 298 Q1
tags:
    - Hash Table
    - String
    - Enumeration
---

<!-- problem:start -->

# [2309. Greatest English Letter in Upper and Lower Case](https://leetcode.com/problems/greatest-english-letter-in-upper-and-lower-case)

[中文文档](/solution/2300-2399/2309.Greatest%20English%20Letter%20in%20Upper%20and%20Lower%20Case/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi gồm các chữ cái tiếng Anh <code>s</code>, hãy trả về <em>chữ cái tiếng Anh <strong>lớn nhất</strong> xuất hiện ở <strong>cả</strong> dạng chữ thường và chữ hoa trong</em> <code>s</code>. Chữ cái được trả về phải ở dạng <strong>chữ hoa</strong>. Nếu không có chữ cái nào như vậy, hãy trả về <em>một chuỗi rỗng</em>.</p>

<p>Một chữ cái tiếng Anh <code>b</code> được xem là <strong>lớn hơn</strong> một chữ cái khác <code>a</code> nếu <code>b</code> xuất hiện <strong>sau</strong> <code>a</code> trong bảng chữ cái tiếng Anh.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;l<strong><u>Ee</u></strong>TcOd<u><strong>E</strong></u>&quot;
<strong>Đầu ra:</strong> &quot;E&quot;
<strong>Giải thích:</strong>
Chữ cái &#39;E&#39; là chữ cái duy nhất xuất hiện ở cả dạng chữ thường và chữ hoa.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;a<strong><u>rR</u></strong>AzFif&quot;
<strong>Đầu ra:</strong> &quot;R&quot;
<strong>Giải thích:</strong>
Chữ cái &#39;R&#39; là chữ cái lớn nhất xuất hiện ở cả dạng chữ thường và chữ hoa.
Lưu ý rằng &#39;A&#39; và &#39;F&#39; cũng xuất hiện ở cả dạng chữ thường và chữ hoa, nhưng &#39;R&#39; lớn hơn &#39;F&#39; hoặc &#39;A&#39;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;AbCdEfGhIjK&quot;
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong>
Không có chữ cái nào xuất hiện ở cả dạng chữ thường và chữ hoa.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và viết hoa.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> $s$ có độ dài tối đa là $1000$; ta cần tìm chữ cái lớn nhất xuất hiện ở cả hai dạng. Đưa mọi ký tự vào một set, sau đó duyệt từ $Z$ xuống $A$. Ký tự đầu tiên thỏa mãn là đáp án.
>
> Set cho phép kiểm tra mỗi dạng trong thời gian hằng số kỳ vọng. Nếu không có ký tự nào thỏa mãn, trả về chuỗi rỗng.

<!-- thinking:end -->

Đầu tiên, ta dùng một bảng băm $ss$ để ghi nhận tất cả các chữ cái xuất hiện trong chuỗi $s$. Sau đó, ta bắt đầu liệt kê từ chữ cái cuối cùng của bảng chữ cái viết hoa. Nếu cả dạng chữ hoa và chữ thường của chữ cái hiện tại đều có trong $ss$, ta trả về chữ cái đó.

Sau khi liệt kê xong, nếu không tìm thấy chữ cái nào thỏa mãn điều kiện, ta trả về một chuỗi rỗng.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(C)$. Trong đó, $n$ và $C$ lần lượt là độ dài của chuỗi $s$ và kích thước của tập ký tự.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def greatestLetter(self, s: str) -> str:
        ss = set(s)
        for c in ascii_uppercase[::-1]:
            if c in ss and c.lower() in ss:
                return c
        return ''
```

#### Java

```java
class Solution {
    public String greatestLetter(String s) {
        Set<Character> ss = new HashSet<>();
        for (char c : s.toCharArray()) {
            ss.add(c);
        }
        for (char a = 'Z'; a >= 'A'; --a) {
            if (ss.contains(a) && ss.contains((char) (a + 32))) {
                return String.valueOf(a);
            }
        }
        return "";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string greatestLetter(string s) {
        unordered_set<char> ss(s.begin(), s.end());
        for (char c = 'Z'; c >= 'A'; --c) {
            if (ss.count(c) && ss.count(char(c + 32))) {
                return string(1, c);
            }
        }
        return "";
    }
};
```

#### Go

```go
func greatestLetter(s string) string {
	ss := map[rune]bool{}
	for _, c := range s {
		ss[c] = true
	}
	for c := 'Z'; c >= 'A'; c-- {
		if ss[c] && ss[rune(c+32)] {
			return string(c)
		}
	}
	return ""
}
```

#### TypeScript

```ts
function greatestLetter(s: string): string {
    const ss = new Array(128).fill(false);
    for (const c of s) {
        ss[c.charCodeAt(0)] = true;
    }
    for (let i = 90; i >= 65; --i) {
        if (ss[i] && ss[i + 32]) {
            return String.fromCharCode(i);
        }
    }
    return '';
}
```

#### Rust

```rust
impl Solution {
    pub fn greatest_letter(s: String) -> String {
        let mut arr = [0; 26];
        for &c in s.as_bytes().iter() {
            if c >= b'a' {
                arr[(c - b'a') as usize] |= 1;
            } else {
                arr[(c - b'A') as usize] |= 2;
            }
        }
        for i in (0..26).rev() {
            if arr[i] == 3 {
                return char::from(b'A' + (i as u8)).to_string();
            }
        }
        "".to_string()
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {string}
 */
var greatestLetter = function (s) {
    const ss = new Array(128).fill(false);
    for (const c of s) {
        ss[c.charCodeAt(0)] = true;
    }
    for (let i = 90; i >= 65; --i) {
        if (ss[i] && ss[i + 32]) {
            return String.fromCharCode(i);
        }
    }
    return '';
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thao tác bit (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sử dụng không gian tuyến tính theo bảng chữ cái. Hai mươi sáu chữ cái có thể được biểu diễn trong hai bitmask; phép giao của chúng cho biết bit cao nhất đang được bật là chữ cái lớn nhất, vì vậy không gian bổ sung trở thành $O(1)$.

<!-- thinking:end -->

Ta có thể dùng hai số nguyên $mask1$ và $mask2$ để ghi nhận các chữ cái viết thường và viết hoa xuất hiện trong chuỗi $s$. Bit thứ $i$ của $mask1$ cho biết chữ cái viết thường thứ $i$ có xuất hiện hay không, còn bit thứ $i$ của $mask2$ cho biết chữ cái viết hoa thứ $i$ có xuất hiện hay không.

Sau đó, ta thực hiện phép AND bit trên $mask1$ và $mask2$. Bit thứ $i$ của $mask$ kết quả cho biết chữ cái thứ $i$ có xuất hiện ở cả dạng chữ hoa và chữ thường hay không.

Tiếp theo, ta chỉ cần tìm vị trí của bit $1$ cao nhất trong biểu diễn nhị phân của $mask$, rồi chuyển nó thành chữ cái viết hoa tương ứng. Nếu tất cả các bit nhị phân đều không phải là $1$, nghĩa là không có chữ cái nào xuất hiện ở cả dạng chữ hoa và chữ thường, nên ta trả về một chuỗi rỗng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def greatestLetter(self, s: str) -> str:
        mask1 = mask2 = 0
        for c in s:
            if c.islower():
                mask1 |= 1 << (ord(c) - ord("a"))
            else:
                mask2 |= 1 << (ord(c) - ord("A"))
        mask = mask1 & mask2
        return chr(mask.bit_length() - 1 + ord("A")) if mask else ""
```

#### Java

```java
class Solution {
    public String greatestLetter(String s) {
        int mask1 = 0, mask2 = 0;
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (Character.isLowerCase(c)) {
                mask1 |= 1 << (c - 'a');
            } else {
                mask2 |= 1 << (c - 'A');
            }
        }
        int mask = mask1 & mask2;
        return mask > 0 ? String.valueOf((char) (31 - Integer.numberOfLeadingZeros(mask) + 'A'))
                        : "";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string greatestLetter(string s) {
        int mask1 = 0, mask2 = 0;
        for (char& c : s) {
            if (islower(c)) {
                mask1 |= 1 << (c - 'a');
            } else {
                mask2 |= 1 << (c - 'A');
            }
        }
        int mask = mask1 & mask2;
        return mask ? string(1, 31 - __builtin_clz(mask) + 'A') : "";
    }
};
```

#### Go

```go
func greatestLetter(s string) string {
	mask1, mask2 := 0, 0
	for _, c := range s {
		if unicode.IsLower(c) {
			mask1 |= 1 << (c - 'a')
		} else {
			mask2 |= 1 << (c - 'A')
		}
	}
	mask := mask1 & mask2
	if mask == 0 {
		return ""
	}
	return string(byte(bits.Len(uint(mask))-1) + 'A')
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
