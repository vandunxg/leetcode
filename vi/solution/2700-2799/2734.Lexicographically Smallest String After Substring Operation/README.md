---
comments: true
difficulty: Medium
rating: 1405
source: Weekly Contest 349 Q2
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [2734. Lexicographically Smallest String After Substring Operation](https://leetcode.com/problems/lexicographically-smallest-string-after-substring-operation)

[中文文档](/solution/2700-2799/2734.Lexicographically%20Smallest%20String%20After%20Substring%20Operation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường. Thực hiện thao tác sau:</p>

<ul>
	<li>Chọn một <span data-keyword="substring-nonempty">chuỗi con</span> không rỗng bất kỳ, sau đó thay mỗi chữ cái trong chuỗi con bằng chữ cái đứng trước nó trong bảng chữ cái tiếng Anh. Ví dụ, &#39;b&#39; được chuyển thành &#39;a&#39;, còn &#39;a&#39; được chuyển thành &#39;z&#39;.</li>
</ul>

<p>Trả về chuỗi <span data-keyword="lexicographically-smaller-string"><strong>nhỏ nhất theo thứ tự từ điển</strong></span> <strong>sau khi thực hiện thao tác</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;cbabc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;baabc&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thực hiện thao tác trên chuỗi con bắt đầu tại chỉ số 0 và kết thúc tại chỉ số 1, bao gồm cả hai đầu mút.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aa&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;az&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thực hiện thao tác trên chữ cái cuối cùng.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;acbbc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;abaab&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thực hiện thao tác trên chuỗi con bắt đầu tại chỉ số 1 và kết thúc tại chỉ số 4, bao gồm cả hai đầu mút.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;leetcode&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;kddsbncd&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thực hiện thao tác trên toàn bộ chuỗi.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 3 * 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải giảm mỗi chữ cái trong đúng một chuỗi con liên tiếp một lần ($a$ chuyển thành $z$) và muốn kết quả nhỏ nhất theo thứ tự từ điển. Thử mọi chuỗi con là không thể khi $n\le 10^5$.
>
> Vì giảm $a$ sẽ cho $z$, ta nên tránh $a$. Bắt đầu từ đoạn liên tiếp đầu tiên không chứa $a$ và dừng trước chữ $a$ tiếp theo sẽ làm giảm vị trí sớm nhất có thể. Nếu chuỗi chỉ gồm các chữ $a$, thao tác bắt buộc sẽ chuyển ký tự cuối cùng thành $z$.

<!-- thinking:end -->

Ta có thể duyệt chuỗi $s$ từ trái sang phải, tìm vị trí $i$ của ký tự đầu tiên khác 'a', sau đó tìm vị trí $j$ của ký tự 'a' đầu tiên bắt đầu từ $i$. Ta giảm mỗi ký tự trong $s[i:j]$, rồi trả về chuỗi đã xử lý.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestString(self, s: str) -> str:
        n = len(s)
        i = 0
        while i < n and s[i] == "a":
            i += 1
        if i == n:
            return s[:-1] + "z"
        j = i
        while j < n and s[j] != "a":
            j += 1
        return s[:i] + "".join(chr(ord(c) - 1) for c in s[i:j]) + s[j:]
```

#### Java

```java
class Solution {
    public String smallestString(String s) {
        int n = s.length();
        int i = 0;
        while (i < n && s.charAt(i) == 'a') {
            ++i;
        }
        if (i == n) {
            return s.substring(0, n - 1) + "z";
        }
        int j = i;
        char[] cs = s.toCharArray();
        while (j < n && cs[j] != 'a') {
            cs[j] = (char) (cs[j] - 1);
            ++j;
        }
        return String.valueOf(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string smallestString(string s) {
        int n = s.size();
        int i = 0;
        while (i < n && s[i] == 'a') {
            ++i;
        }
        if (i == n) {
            s[n - 1] = 'z';
            return s;
        }
        int j = i;
        while (j < n && s[j] != 'a') {
            s[j] = s[j] - 1;
            ++j;
        }
        return s;
    }
};
```

#### Go

```go
func smallestString(s string) string {
	n := len(s)
	i := 0
	for i < n && s[i] == 'a' {
		i++
	}
	cs := []byte(s)
	if i == n {
		cs[n-1] = 'z'
		return string(cs)
	}
	j := i
	for j < n && cs[j] != 'a' {
		cs[j] = cs[j] - 1
		j++
	}
	return string(cs)
}
```

#### TypeScript

```ts
function smallestString(s: string): string {
    const cs: string[] = s.split('');
    const n: number = cs.length;
    let i: number = 0;
    while (i < n && cs[i] === 'a') {
        i++;
    }

    if (i === n) {
        cs[n - 1] = 'z';
        return cs.join('');
    }

    let j: number = i;
    while (j < n && cs[j] !== 'a') {
        const c: number = cs[j].charCodeAt(0);
        cs[j] = String.fromCharCode(c - 1);
        j++;
    }

    return cs.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn smallest_string(s: String) -> String {
        let mut cs: Vec<char> = s.chars().collect();
        let n = cs.len();
        let mut i = 0;

        while i < n && cs[i] == 'a' {
            i += 1;
        }

        if i == n {
            cs[n - 1] = 'z';
            return cs.into_iter().collect();
        }

        let mut j = i;
        while j < n && cs[j] != 'a' {
            cs[j] = ((cs[j] as u8) - 1) as char;
            j += 1;
        }

        cs.into_iter().collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
