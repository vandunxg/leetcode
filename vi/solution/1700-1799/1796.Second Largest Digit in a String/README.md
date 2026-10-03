---
comments: true
difficulty: Easy
rating: 1341
source: Biweekly Contest 48 Q1
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [1796. Second Largest Digit in a String](https://leetcode.com/problems/second-largest-digit-in-a-string)

[中文文档](/solution/1700-1799/1796.Second%20Largest%20Digit%20in%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi chữ-số <code>s</code>, trả về <em>chữ số <strong>lớn thứ hai</strong> xuất hiện trong </em><code>s</code><em>, hoặc </em><code>-1</code><em> nếu không tồn tại</em>.</p>

<p>Chuỗi <strong>chữ-số</strong><strong> </strong> là chuỗi chỉ gồm các chữ cái tiếng Anh viết thường và chữ số.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;dfa12321afd&quot;
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Các chữ số xuất hiện trong s là [1, 2, 3]. Chữ số lớn thứ hai là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abc1111&quot;
<strong>Output:</strong> -1
<strong>Giải thích:</strong> Các chữ số xuất hiện trong s là [1]. Không có chữ số lớn thứ hai.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 500</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần chữ số lớn thứ hai phân biệt, hoặc $-1$. Chỉ cần một lần quét để giữ chữ số lớn nhất $a$ và lớn thứ hai $b$.
>
> Giá trị lớn nhất mới sẽ đẩy giá trị lớn nhất cũ xuống vị trí thứ hai; một giá trị nằm nghiêm ngặt giữa chúng chỉ cập nhật vị trí thứ hai.

<!-- thinking:end -->

Ta dùng $a$ và $b$ lần lượt biểu diễn số lớn nhất và lớn thứ hai trong chuỗi, ban đầu $a = b = -1$.

Ta duyệt chuỗi $s$. Nếu ký tự hiện tại là chữ số, chuyển nó thành số $v$. Nếu $v > a$, $v$ là số lớn nhất đang xuất hiện, nên cập nhật $b$ thành $a$ và $a$ thành $v$; nếu $v < a$, $v$ là số lớn thứ hai đang xuất hiện, nên cập nhật $b$ thành $v$.

Sau khi duyệt xong, trả về $b$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def secondHighest(self, s: str) -> int:
        a = b = -1
        for c in s:
            if c.isdigit():
                v = int(c)
                if v > a:
                    a, b = v, a
                elif b < v < a:
                    b = v
        return b
```

#### Java

```java
class Solution {
    public int secondHighest(String s) {
        int a = -1, b = -1;
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                int v = c - '0';
                if (v > a) {
                    b = a;
                    a = v;
                } else if (v > b && v < a) {
                    b = v;
                }
            }
        }
        return b;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int secondHighest(string s) {
        int a = -1, b = -1;
        for (char& c : s) {
            if (isdigit(c)) {
                int v = c - '0';
                if (v > a) {
                    b = a, a = v;
                } else if (v > b && v < a) {
                    b = v;
                }
            }
        }
        return b;
    }
};
```

#### Go

```go
func secondHighest(s string) int {
	a, b := -1, -1
	for _, c := range s {
		if c >= '0' && c <= '9' {
			v := int(c - '0')
			if v > a {
				b, a = a, v
			} else if v > b && v < a {
				b = v
			}
		}
	}
	return b
}
```

#### TypeScript

```ts
function secondHighest(s: string): number {
    let first = -1;
    let second = -1;
    for (const c of s) {
        if (c >= '0' && c <= '9') {
            const num = c.charCodeAt(0) - '0'.charCodeAt(0);
            if (first < num) {
                [first, second] = [num, first];
            } else if (first !== num && second < num) {
                second = num;
            }
        }
    }
    return second;
}
```

#### Rust

```rust
impl Solution {
    pub fn second_highest(s: String) -> i32 {
        let mut first = -1;
        let mut second = -1;
        for c in s.as_bytes() {
            if char::is_digit(*c as char, 10) {
                let num = (c - b'0') as i32;
                if first < num {
                    second = first;
                    first = num;
                } else if num < first && second < num {
                    second = num;
                }
            }
        }
        second
    }
}
```

#### C

```c
int secondHighest(char* s) {
    int first = -1;
    int second = -1;
    for (int i = 0; s[i]; i++) {
        if (isdigit(s[i])) {
            int num = s[i] - '0';
            if (num > first) {
                second = first;
                first = num;
            } else if (num < first && second < num) {
                second = num;
            }
        }
    }
    return second;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Ta cũng có thể dùng mask 10 bit để đánh dấu các chữ số đã gặp: duyệt bit từ cao xuống thấp và trả về bit thứ hai được bật.

<!-- thinking:end -->

Ta có thể dùng số nguyên $mask$ để đánh dấu các số xuất hiện trong chuỗi; bit thứ $i$ của $mask$ cho biết số $i$ đã xuất hiện hay chưa.

Ta duyệt chuỗi $s$. Nếu ký tự hiện tại là chữ số, chuyển thành số $v$ rồi đặt bit thứ $v$ của $mask$ thành $1$.

Cuối cùng, ta duyệt $mask$ từ cao xuống thấp, tìm bit thứ hai bằng $1$; số tương ứng là số lớn thứ hai. Nếu không có số lớn thứ hai, trả về $-1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def secondHighest(self, s: str) -> int:
        mask = reduce(or_, (1 << int(c) for c in s if c.isdigit()), 0)
        cnt = 0
        for i in range(9, -1, -1):
            if (mask >> i) & 1:
                cnt += 1
            if cnt == 2:
                return i
        return -1
```

#### Java

```java
class Solution {
    public int secondHighest(String s) {
        int mask = 0;
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                mask |= 1 << (c - '0');
            }
        }
        for (int i = 9, cnt = 0; i >= 0; --i) {
            if (((mask >> i) & 1) == 1 && ++cnt == 2) {
                return i;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int secondHighest(string s) {
        int mask = 0;
        for (char& c : s)
            if (isdigit(c)) mask |= 1 << c - '0';
        for (int i = 9, cnt = 0; ~i; --i)
            if (mask >> i & 1 && ++cnt == 2) return i;
        return -1;
    }
};
```

#### Go

```go
func secondHighest(s string) int {
	mask := 0
	for _, c := range s {
		if c >= '0' && c <= '9' {
			mask |= 1 << int(c-'0')
		}
	}
	for i, cnt := 9, 0; i >= 0; i-- {
		if mask>>i&1 == 1 {
			cnt++
			if cnt == 2 {
				return i
			}
		}
	}
	return -1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
