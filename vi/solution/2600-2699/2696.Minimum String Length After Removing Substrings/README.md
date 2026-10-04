---
comments: true
difficulty: Easy
rating: 1282
source: Weekly Contest 346 Q1
tags:
    - Stack
    - String
    - Simulation
---

<!-- problem:start -->

# [2696. Minimum String Length After Removing Substrings](https://leetcode.com/problems/minimum-string-length-after-removing-substrings)

[中文文档](/solution/2600-2699/2696.Minimum%20String%20Length%20After%20Removing%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh <strong>viết hoa</strong>.</p>

<p>Bạn có thể thực hiện một số thao tác trên chuỗi này; trong mỗi thao tác, bạn có thể xóa <strong>bất kỳ</strong> lần xuất hiện nào của một trong hai chuỗi con <code>&quot;AB&quot;</code> hoặc <code>&quot;CD&quot;</code> khỏi <code>s</code>.</p>

<p>Hãy trả về <em>độ dài <strong>nhỏ nhất</strong> có thể có của chuỗi kết quả</em>.</p>

<p><strong>Lưu ý</strong> rằng chuỗi sẽ nối lại sau khi xóa chuỗi con và có thể tạo ra các chuỗi con <code>&quot;AB&quot;</code> hoặc <code>&quot;CD&quot;</code> mới.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ABFCACDB&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác sau:
- Xóa chuỗi con &quot;<u>AB</u>FCACDB&quot;, khi đó s = &quot;FCACDB&quot;.
- Xóa chuỗi con &quot;FCA<u>CD</u>B&quot;, khi đó s = &quot;FCAB&quot;.
- Xóa chuỗi con &quot;FC<u>AB</u>&quot;, khi đó s = &quot;FC&quot;.
Vì vậy, độ dài chuỗi kết quả là 2.
Có thể chứng minh rằng đây là độ dài nhỏ nhất có thể đạt được.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ACBBD&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Ta không thể thực hiện thao tác nào trên chuỗi, nên độ dài vẫn là 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code>&nbsp;chỉ gồm các chữ cái tiếng Anh viết hoa.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ngăn xếp

<!-- thinking:start -->

> **Tư duy**
>
> `AB` và `CD` có thể bị xóa liên tiếp. Việc gọi `replace` lặp lại có thể có độ phức tạp bậc hai trong trường hợp xấu nhất. Quy tắc này tương tự như xử lý dấu ngoặc: lấy phần tử trên cùng ra khi phần tử trên cùng và ký tự hiện tại tạo thành một trong hai cặp, nếu không thì đưa ký tự vào ngăn xếp.
>
> Đặt sẵn một ký tự rỗng ở đỉnh ngăn xếp để không cần kiểm tra ngăn xếp có rỗng hay không; đáp án là độ dài còn lại trừ đi một.

<!-- thinking:end -->

Ta duyệt qua chuỗi $s$. Với ký tự hiện tại $c$, nếu ngăn xếp không rỗng và phần tử trên cùng của ngăn xếp $top$ có thể tạo thành $AB$ hoặc $CD$ với $c$, ta lấy phần tử trên cùng ra khỏi ngăn xếp; nếu không, ta đưa $c$ vào ngăn xếp.

Số phần tử còn lại trong ngăn xếp chính là độ dài của chuỗi cuối cùng.

> Khi cài đặt, ta có thể đặt sẵn một ký tự rỗng trong ngăn xếp, nhờ đó không cần kiểm tra ngăn xếp có rỗng trong quá trình duyệt chuỗi. Cuối cùng, ta trả về kích thước của ngăn xếp trừ đi một.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minLength(self, s: str) -> int:
        stk = [""]
        for c in s:
            if (c == "B" and stk[-1] == "A") or (c == "D" and stk[-1] == "C"):
                stk.pop()
            else:
                stk.append(c)
        return len(stk) - 1
```

#### Java

```java
class Solution {
    public int minLength(String s) {
        Deque<Character> stk = new ArrayDeque<>();
        stk.push(' ');
        for (char c : s.toCharArray()) {
            if ((c == 'B' && stk.peek() == 'A') || (c == 'D' && stk.peek() == 'C')) {
                stk.pop();
            } else {
                stk.push(c);
            }
        }
        return stk.size() - 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minLength(string s) {
        string stk = " ";
        for (char& c : s) {
            if ((c == 'B' && stk.back() == 'A') || (c == 'D' && stk.back() == 'C')) {
                stk.pop_back();
            } else {
                stk.push_back(c);
            }
        }
        return stk.size() - 1;
    }
};
```

#### Go

```go
func minLength(s string) int {
	stk := []byte{' '}
	for _, c := range s {
		if (c == 'B' && stk[len(stk)-1] == 'A') || (c == 'D' && stk[len(stk)-1] == 'C') {
			stk = stk[:len(stk)-1]
		} else {
			stk = append(stk, byte(c))
		}
	}
	return len(stk) - 1
}
```

#### TypeScript

```ts
function minLength(s: string): number {
    const stk: string[] = [];
    for (const c of s) {
        if ((stk.at(-1) === 'A' && c === 'B') || (stk.at(-1) === 'C' && c === 'D')) {
            stk.pop();
        } else {
            stk.push(c);
        }
    }
    return stk.length;
}
```

#### JavaScript

```js
function minLength(s) {
    const stk = [];
    for (const c of s) {
        if ((stk.at(-1) === 'A' && c === 'B') || (stk.at(-1) === 'C' && c === 'D')) {
            stk.pop();
        } else {
            stk.push(c);
        }
    }
    return stk.length;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_length(s: String) -> i32 {
        let mut ans: Vec<u8> = Vec::new();

        for c in s.bytes() {
            if let Some(last) = ans.last() {
                if *last == b'A' && c == b'B' {
                    ans.pop();
                } else if *last == b'C' && c == b'D' {
                    ans.pop();
                } else {
                    ans.push(c);
                }
            } else {
                ans.push(c);
            }
        }

        ans.len() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Một dòng

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sử dụng một ngăn xếp tường minh. Với độ dài $\le 100$, ta cũng có thể xóa `AB|CD` bằng regular expression cho đến khi độ dài ổn định, viết thành một lời gọi đệ quy trên một dòng. Với kích thước này, việc quét thêm vài lần vẫn chấp nhận được.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
const minLength = (s: string, n = s.length): number =>
    ((s = s.replace(/AB|CD/g, '')), s.length === n) ? n : minLength(s);
```

#### JavaScript

```js
const minLength = (s, n = s.length) =>
    ((s = s.replace(/AB|CD/g, '')), s.length === n) ? n : minLength(s);
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
