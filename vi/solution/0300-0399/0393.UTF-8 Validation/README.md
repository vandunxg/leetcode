---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [393. UTF-8 Validation](https://leetcode.com/problems/utf-8-validation)

[中文文档](/solution/0300-0399/0393.UTF-8%20Validation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>data</code> biểu diễn dữ liệu. Hãy xác định dữ liệu có phải là encoding <strong>UTF-8</strong> hợp lệ hay không (tức có thể giải mã thành một chuỗi ký tự được mã hóa UTF-8 hợp lệ).</p>

<p>Một ký tự trong <strong>UTF8</strong> có độ dài từ <strong>1 đến 4 byte</strong>, theo các quy tắc sau:</p>

<ol>
	<li>Với ký tự dài <strong>1 byte</strong>, bit đầu tiên là <code>0</code>, theo sau là mã Unicode của ký tự.</li>
	<li>Với ký tự dài <strong>n byte</strong>, <code>n</code> bit đầu tiên đều là <code>1</code>, bit thứ <code>n + 1</code> là <code>0</code>, sau đó là <code>n - 1</code> byte có <code>2</code> bit cao nhất là <code>10</code>.</li>
</ol>

<p>Encoding UTF-8 hoạt động như sau:</p>

<pre>
     Số byte          |        Dãy octet UTF-8
                       |          (nhị phân)
   --------------------+-----------------------------------------
            1          |   0xxxxxxx
            2          |   110xxxxx 10xxxxxx
            3          |   1110xxxx 10xxxxxx 10xxxxxx
            4          |   11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
</pre>

<p><code>x</code> biểu thị một bit trong biểu diễn nhị phân của byte, có thể là <code>0</code> hoặc <code>1</code>.</p>

<p><strong>Lưu ý: </strong>Đầu vào là một mảng số nguyên. Chỉ <strong>8 bit thấp nhất</strong> của mỗi số nguyên được dùng để lưu dữ liệu, nghĩa là mỗi số nguyên chỉ biểu diễn 1 byte dữ liệu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> data = [197,130,1]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> data biểu diễn dãy octet: 11000101 10000010 00000001.
Đây là encoding utf-8 hợp lệ của một ký tự 2 byte theo sau bởi một ký tự 1 byte.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> data = [235,140,4]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> data biểu diễn dãy octet: 11101011 10001100 00000100.
3 bit đầu tiên đều là 1 và bit thứ 4 là 0, nên đây là ký tự dài 3 byte.
Byte tiếp theo là byte tiếp nối bắt đầu bằng 10, như vậy là đúng.
Tuy nhiên, byte tiếp nối thứ hai không bắt đầu bằng 10 nên encoding không hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= data.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= data[i] &lt;= 255</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lượt

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra một luồng byte UTF-8 được biểu diễn bằng các số nguyên. Prefix đầu tiên xác định cần bao nhiêu byte `10xxxxxx` theo sau. Chỉ cần duyệt một lượt.
>
> `cnt` là số byte tiếp nối còn phải đọc; các byte này phải có dạng `10xxxxxx`. Nếu không, giải mã header 1–4 byte rồi cập nhật `cnt`. Prefix sai thì trả về không hợp lệ; để hợp lệ, `cnt=0` khi kết thúc.

<!-- thinking:end -->

Dùng biến $cnt$ để ghi lại số byte tiếp nối còn cần đọc, các byte này phải bắt đầu bằng $10$. Ban đầu, $cnt = 0$.

Với mỗi số nguyên $v$ trong mảng:

- Nếu $cnt > 0$, kiểm tra xem $v$ có bắt đầu bằng $10$ hay không. Nếu không, trả về `false`; nếu có, giảm $cnt$.
- Nếu bit cao nhất của $v$ là $0$, đặt $cnt = 0$.
- Nếu hai bit cao nhất của $v$ là $110$, đặt $cnt = 1$.
- Nếu ba bit cao nhất của $v$ là $1110$, đặt $cnt = 2$.
- Nếu bốn bit cao nhất của $v$ là $11110$, đặt $cnt = 3$.
- Nếu không khớp trường hợp nào ở trên, trả về `false`.

Cuối cùng, nếu $cnt = 0$ thì trả về `true`, còn lại trả về `false`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng `data`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validUtf8(self, data: List[int]) -> bool:
        cnt = 0
        for v in data:
            if cnt > 0:
                if v >> 6 != 0b10:
                    return False
                cnt -= 1
            elif v >> 7 == 0:
                cnt = 0
            elif v >> 5 == 0b110:
                cnt = 1
            elif v >> 4 == 0b1110:
                cnt = 2
            elif v >> 3 == 0b11110:
                cnt = 3
            else:
                return False
        return cnt == 0
```

#### Java

```java
class Solution {
    public boolean validUtf8(int[] data) {
        int cnt = 0;
        for (int v : data) {
            if (cnt > 0) {
                if (v >> 6 != 0b10) {
                    return false;
                }
                --cnt;
            } else if (v >> 7 == 0) {
                cnt = 0;
            } else if (v >> 5 == 0b110) {
                cnt = 1;
            } else if (v >> 4 == 0b1110) {
                cnt = 2;
            } else if (v >> 3 == 0b11110) {
                cnt = 3;
            } else {
                return false;
            }
        }
        return cnt == 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool validUtf8(vector<int>& data) {
        int cnt = 0;
        for (int& v : data) {
            if (cnt > 0) {
                if (v >> 6 != 0b10) {
                    return false;
                }
                --cnt;
            } else if (v >> 7 == 0) {
                cnt = 0;
            } else if (v >> 5 == 0b110) {
                cnt = 1;
            } else if (v >> 4 == 0b1110) {
                cnt = 2;
            } else if (v >> 3 == 0b11110) {
                cnt = 3;
            } else {
                return false;
            }
        }
        return cnt == 0;
    }
};
```

#### Go

```go
func validUtf8(data []int) bool {
	cnt := 0
	for _, v := range data {
		if cnt > 0 {
			if v>>6 != 0b10 {
				return false
			}
			cnt--
		} else if v>>7 == 0 {
			cnt = 0
		} else if v>>5 == 0b110 {
			cnt = 1
		} else if v>>4 == 0b1110 {
			cnt = 2
		} else if v>>3 == 0b11110 {
			cnt = 3
		} else {
			return false
		}
	}
	return cnt == 0
}
```

#### TypeScript

```ts
function validUtf8(data: number[]): boolean {
    let cnt = 0;
    for (const v of data) {
        if (cnt > 0) {
            if (v >> 6 !== 0b10) {
                return false;
            }
            --cnt;
        } else if (v >> 7 === 0) {
            cnt = 0;
        } else if (v >> 5 === 0b110) {
            cnt = 1;
        } else if (v >> 4 === 0b1110) {
            cnt = 2;
        } else if (v >> 3 === 0b11110) {
            cnt = 3;
        } else {
            return false;
        }
    }
    return cnt === 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
