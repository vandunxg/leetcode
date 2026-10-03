---
comments: true
difficulty: Easy
rating: 1257
source: Weekly Contest 263 Q1
tags:
    - String
---

<!-- problem:start -->

# [2042. Check if Numbers Are Ascending in a Sentence](https://leetcode.com/problems/check-if-numbers-are-ascending-in-a-sentence)

[中文文档](/solution/2000-2099/2042.Check%20if%20Numbers%20Are%20Ascending%20in%20a%20Sentence/README.md)

## Mô tả

<!-- description:start -->

<p>Một câu là một danh sách các <strong>token</strong> được phân tách bằng một dấu cách <strong>duy nhất</strong>, không có dấu cách ở đầu hoặc cuối. Mỗi token là một <strong>số dương</strong> gồm các chữ số <code>0-9</code> không có số 0 ở đầu, hoặc một <strong>từ</strong> gồm các chữ cái tiếng Anh viết thường.</p>

<ul>
	<li>Ví dụ, <code>&quot;a puppy has 2 eyes 4 legs&quot;</code> là một câu gồm bảy token: <code>&quot;2&quot;</code> và <code>&quot;4&quot;</code> là các số, còn những token khác như <code>&quot;puppy&quot;</code> là các từ.</li>
</ul>

<p>Cho một chuỗi <code>s</code> biểu diễn một câu, hãy kiểm tra xem <strong>tất cả</strong> các số trong <code>s</code> có <strong>tăng nghiêm ngặt</strong> từ trái sang phải hay không (tức là, ngoại trừ số cuối cùng, <strong>mỗi</strong> số phải <strong>nhỏ hơn nghiêm ngặt</strong> số <strong>ngay bên phải</strong> nó trong <code>s</code>).</p>

<p>Trả về <code>true</code><em> nếu đúng, hoặc </em><code>false</code><em> nếu ngược lại</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="example-1" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2042.Check%20if%20Numbers%20Are%20Ascending%20in%20a%20Sentence/images/example1.png" style="width: 637px; height: 48px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;1 box has 3 blue 4 red 6 green and 12 yellow marbles&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Các số trong s là: 1, 3, 4, 6, 12.
Chúng tăng nghiêm ngặt từ trái sang phải: 1 &lt; 3 &lt; 4 &lt; 6 &lt; 12.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;hello world 5 x 5&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Các số trong s là: <u><strong>5</strong></u>, <strong><u>5</u></strong>. Chúng không tăng nghiêm ngặt.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="example-3" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2042.Check%20if%20Numbers%20Are%20Ascending%20in%20a%20Sentence/images/example3.png" style="width: 794px; height: 48px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;sunset is at 7 51 pm overnight lows will be in the low 50 and 60 s&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Các số trong s là: 7, <u><strong>51</strong></u>, <u><strong>50</strong></u>, 60. Chúng không tăng nghiêm ngặt.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 200</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường, dấu cách và các chữ số từ <code>0</code> đến <code>9</code>.</li>
	<li>Số lượng token trong <code>s</code> nằm trong khoảng từ <code>2</code> đến <code>100</code>.</li>
	<li>Các token trong <code>s</code> được phân tách bằng một dấu cách.</li>
	<li><code>s</code> có ít nhất <strong>hai</strong> số.</li>
	<li>Mỗi số trong <code>s</code> là một số <strong>dương</strong> <strong>nhỏ hơn</strong> <code>100</code>, không có số 0 ở đầu.</li>
	<li><code>s</code> không có dấu cách ở đầu hoặc cuối.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Câu có tối đa $200$ ký tự. Sau khi tách theo dấu cách, ta kiểm tra các token dạng số có tăng nghiêm ngặt hay không và bỏ qua các token còn lại.
>
> Duy trì số trước đó $pre$; trả về kết quả sai nếu giá trị hiện tại $\le pre$.

<!-- thinking:end -->

Ta có thể tách chuỗi $s$ thành các từ bằng dấu cách. Sau đó, với mỗi từ, kiểm tra xem đó có phải là một số hay không. Nếu là số, chuyển nó thành số nguyên và so sánh với số trước đó. Nếu dãy không tăng nghiêm ngặt, trả về `false`. Ngược lại, gán số hiện tại cho số trước đó và tiếp tục duyệt.

Nếu duyệt hết chuỗi, điều đó có nghĩa là các số trong chuỗi tăng nghiêm ngặt, nên trả về `true`.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def areNumbersAscending(self, s: str) -> bool:
        pre = 0
        for t in s.split():
            if t[0].isdigit():
                if (cur := int(t)) <= pre:
                    return False
                pre = cur
        return True
```

#### Java

```java
class Solution {
    public boolean areNumbersAscending(String s) {
        int pre = 0;
        for (var t : s.split(" ")) {
            if (t.charAt(0) <= '9') {
                int cur = Integer.parseInt(t);
                if (pre >= cur) {
                    return false;
                }
                pre = cur;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool areNumbersAscending(string s) {
        int pre = 0;
        istringstream is(s);
        string t;
        while (is >> t) {
            if (isdigit(t[0])) {
                int cur = stoi(t);
                if (pre >= cur) {
                    return false;
                }
                pre = cur;
            }
        }
        return true;
    }
};
```

#### Go

```go
func areNumbersAscending(s string) bool {
	pre := 0
	for _, t := range strings.Split(s, " ") {
		if t[0] <= '9' {
			cur, _ := strconv.Atoi(t)
			if pre >= cur {
				return false
			}
			pre = cur
		}
	}
	return true
}
```

#### TypeScript

```ts
function areNumbersAscending(s: string): boolean {
    let pre = -1;
    for (const cur of s.split(' ')) {
        if (cur[0] <= '9') {
            const num = Number(cur);
            if (num <= pre) {
                return false;
            }
            pre = num;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn are_numbers_ascending(s: String) -> bool {
        let mut pre = -1;
        for cur in s.split(' ') {
            if cur.as_bytes()[0] <= b'9' {
                let num = cur.parse::<i32>().unwrap();
                if num <= pre {
                    return false;
                }
                pre = num;
            }
        }
        true
    }
}
```

#### C

```c
bool areNumbersAscending(char* s) {
    int pre = -1;
    int cur = 0;
    for (int i = 0; s[i]; i++) {
        if (isdigit(s[i])) {
            cur = cur * 10 + s[i] - '0';
        } else {
            if (cur != 0) {
                if (cur <= pre) {
                    return 0;
                }
                pre = cur;
                cur = 0;
            }
        }
    }
    if (cur != 0 && cur <= pre) {
        return 0;
    }
    return 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
