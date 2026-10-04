---
comments: true
difficulty: Easy
rating: 1242
source: Weekly Contest 406 Q1
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [3216. Lexicographically Smallest String After a Swap](https://leetcode.com/problems/lexicographically-smallest-string-after-a-swap)

[Tài liệu tiếng Trung](/solution/3200-3299/3216.Lexicographically%20Smallest%20String%20After%20a%20Swap/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> chỉ chứa các chữ số, hãy trả về <span data-keyword="lexicographically-smaller-string">chuỗi nhỏ nhất theo thứ tự từ điển</span> có thể nhận được sau khi hoán đổi các chữ số <strong>liền kề</strong> trong <code>s</code> có cùng <strong>tính chẵn lẻ</strong> không quá <strong>một lần</strong>.</p>

<p>Hai chữ số có cùng tính chẵn lẻ nếu cả hai đều lẻ hoặc đều chẵn. Ví dụ, 5 và 9, cũng như 2 và 4, có cùng tính chẵn lẻ, còn 6 và 9 thì không.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;45320&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;43520&quot;</span></p>

<p><strong>Giải thích: </strong></p>

<p><code>s[1] == &#39;5&#39;</code> và <code>s[2] == &#39;3&#39;</code> có cùng tính chẵn lẻ, và việc hoán đổi chúng tạo ra chuỗi nhỏ nhất theo thứ tự từ điển.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;001&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;001&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không cần thực hiện phép hoán đổi vì <code>s</code> đã là chuỗi nhỏ nhất theo thứ tự từ điển.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Vì ta chỉ có thể hoán đổi các chữ số liền kề cùng tính chẵn lẻ không quá một lần, $n\le 100$ cho phép thử mọi phép hoán đổi hợp lệ, nhưng phép hoán đổi tốt nhất theo thứ tự từ điển là phép hoán đổi đầu tiên từ bên trái.
>
> Duyệt từ trái sang phải để tìm cặp liền kề đầu tiên có cùng tính chẵn lẻ và chữ số bên trái lớn hơn chữ số bên phải, sau đó hoán đổi và dừng lại: phép hoán đổi ở phía sau không thể cải thiện prefix đã tốt hơn. Nếu không tìm thấy cặp nào như vậy, chuỗi đã là chuỗi nhỏ nhất.

<!-- thinking:end -->

Ta có thể duyệt chuỗi $\textit{s}$ từ trái sang phải. Với mỗi cặp chữ số liền kề, nếu chúng có cùng tính chẵn lẻ và chữ số trước lớn hơn chữ số sau, ta hoán đổi hai chữ số này để làm cho thứ tự từ điển của chuỗi $\textit{s}$ nhỏ hơn, sau đó trả về chuỗi đã hoán đổi.

Sau khi duyệt xong, nếu không tìm thấy cặp chữ số nào có thể hoán đổi, điều đó có nghĩa là chuỗi $\textit{s}$ đã ở thứ tự từ điển nhỏ nhất, và ta có thể trả về chuỗi đó.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $\textit{s}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getSmallestString(self, s: str) -> str:
        for i, (a, b) in enumerate(pairwise(map(ord, s))):
            if (a + b) % 2 == 0 and a > b:
                return s[:i] + s[i + 1] + s[i] + s[i + 2 :]
        return s
```

#### Java

```java
class Solution {
    public String getSmallestString(String s) {
        char[] cs = s.toCharArray();
        int n = cs.length;
        for (int i = 1; i < n; ++i) {
            char a = cs[i - 1], b = cs[i];
            if (a > b && a % 2 == b % 2) {
                cs[i] = a;
                cs[i - 1] = b;
                return new String(cs);
            }
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string getSmallestString(string s) {
        int n = s.length();
        for (int i = 1; i < n; ++i) {
            char a = s[i - 1], b = s[i];
            if (a > b && a % 2 == b % 2) {
                s[i - 1] = b;
                s[i] = a;
                break;
            }
        }
        return s;
    }
};
```

#### Go

```go
func getSmallestString(s string) string {
	cs := []byte(s)
	n := len(cs)
	for i := 1; i < n; i++ {
		a, b := cs[i-1], cs[i]
		if a > b && a%2 == b%2 {
			cs[i-1], cs[i] = b, a
			return string(cs)
		}
	}
	return s
}
```

#### TypeScript

```ts
function getSmallestString(s: string): string {
    const n = s.length;
    const cs: string[] = s.split('');
    for (let i = 1; i < n; ++i) {
        const a = cs[i - 1];
        const b = cs[i];
        if (a > b && +a % 2 === +b % 2) {
            cs[i - 1] = b;
            cs[i] = a;
            return cs.join('');
        }
    }
    return s;
}
```

#### Rust

```rust
impl Solution {
    pub fn get_smallest_string(s: String) -> String {
        let mut cs: Vec<u8> = s.into_bytes();
        let n = cs.len();
        for i in 1..n {
            let (a, b) = (cs[i - 1], cs[i]);
            if a > b && a % 2 == b % 2 {
                cs.swap(i - 1, i);
                return String::from_utf8(cs).unwrap();
            }
        }
        String::from_utf8(cs).unwrap()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
