---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
    - Math
    - String
---

<!-- problem:start -->

# [405. Convert a Number to Hexadecimal](https://leetcode.com/problems/convert-a-number-to-hexadecimal)

[中文文档](/solution/0400-0499/0405.Convert%20a%20Number%20to%20Hexadecimal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên 32-bit <code>num</code>, hãy trả về <em>chuỗi biểu diễn số đó ở hệ thập lục phân</em>. Với số nguyên âm, sử dụng phương pháp <a href="https://en.wikipedia.org/wiki/Two%27s_complement" target="_blank">bù 2</a>.</p>

<p>Tất cả chữ cái trong chuỗi kết quả phải viết thường. Kết quả không được có số 0 ở đầu, ngoại trừ trường hợp kết quả là chính số 0.</p>

<p><strong>Lưu ý:&nbsp;</strong>Không được sử dụng hàm có sẵn trong thư viện để giải trực tiếp bài toán này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> num = 26
<strong>Đầu ra:</strong> "1a"
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> num = -1
<strong>Đầu ra:</strong> "ffffffff"
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>-2<sup>31</sup> &lt;= num &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần biểu diễn số nguyên bù 2 gồm $32$ bit ở hệ thập lục phân, kể cả số âm. Chuyển từng chữ số thập phân sẽ phải xử lý dấu; nhóm mỗi $4$ bit thì không cần.
>
> Duyệt tám nhóm bit từ cao xuống thấp, dùng mask $0\text{xF}$ để lấy từng nhóm rồi tra bảng chữ số. Bỏ qua các số 0 ở đầu cho đến khi gặp nibble khác 0; xử lý riêng trường hợp giá trị bằng $0$.
>
> Duyệt từ bit cao giúp bỏ số 0 ở đầu mà không cần đảo buffer.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def toHex(self, num: int) -> str:
        if num == 0:
            return '0'
        chars = '0123456789abcdef'
        s = []
        for i in range(7, -1, -1):
            x = (num >> (4 * i)) & 0xF
            if s or x != 0:
                s.append(chars[x])
        return ''.join(s)
```

#### Java

```java
class Solution {
    public String toHex(int num) {
        if (num == 0) {
            return "0";
        }
        StringBuilder sb = new StringBuilder();
        while (num != 0) {
            int x = num & 15;
            if (x < 10) {
                sb.append(x);
            } else {
                sb.append((char) (x - 10 + 'a'));
            }
            num >>>= 4;
        }
        return sb.reverse().toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string toHex(int num) {
        if (num == 0) return "0";
        string s = "";
        for (int i = 7; i >= 0; --i) {
            int x = (num >> (4 * i)) & 0xf;
            if (s.size() > 0 || x != 0) {
                char c = x < 10 ? (char) (x + '0') : (char) (x - 10 + 'a');
                s += c;
            }
        }
        return s;
    }
};
```

#### Go

```go
func toHex(num int) string {
	if num == 0 {
		return "0"
	}
	sb := &strings.Builder{}
	for i := 7; i >= 0; i-- {
		x := num >> (4 * i) & 0xf
		if x > 0 || sb.Len() > 0 {
			var c byte
			if x < 10 {
				c = '0' + byte(x)
			} else {
				c = 'a' + byte(x-10)
			}
			sb.WriteByte(c)
		}
	}
	return sb.String()
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã duyệt theo từng nhóm $4$ bit. Lời giải 2 tạo các chữ số tương tự bằng phép toán số học thay vì tra bảng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Java

```java
class Solution {
    public String toHex(int num) {
        if (num == 0) {
            return "0";
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 7; i >= 0; --i) {
            int x = (num >> (4 * i)) & 0xf;
            if (sb.length() > 0 || x != 0) {
                char c = x < 10 ? (char) (x + '0') : (char) (x - 10 + 'a');
                sb.append(c);
            }
        }
        return sb.toString();
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
