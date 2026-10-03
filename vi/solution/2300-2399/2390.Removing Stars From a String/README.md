---
comments: true
difficulty: Medium
rating: 1347
source: Weekly Contest 308 Q2
tags:
    - Stack
    - String
    - Simulation
---

<!-- problem:start -->

# [2390. Removing Stars From a String](https://leetcode.com/problems/removing-stars-from-a-string)

[中文文档](/solution/2300-2399/2390.Removing%20Stars%20From%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một chuỗi <code>s</code> chứa các dấu sao <code>*</code>.</p>

<p>Trong một thao tác, bạn có thể:</p>

<ul>
	<li>Chọn một dấu sao trong <code>s</code>.</li>
	<li>Xóa ký tự <strong>không phải dấu sao</strong> gần nó nhất về phía <strong>bên trái</strong>, đồng thời xóa chính dấu sao đó.</li>
</ul>

<p>Trả về <em>chuỗi sau khi <strong>tất cả</strong> các dấu sao đã được xóa</em>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Dữ liệu đầu vào được tạo sao cho thao tác luôn có thể thực hiện.</li>
	<li>Có thể chứng minh rằng chuỗi kết quả luôn là duy nhất.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;leet**cod*e&quot;
<strong>Đầu ra:</strong> &quot;lecoe&quot;
<strong>Giải thích:</strong> Thực hiện các lần xóa từ trái sang phải:
- Ký tự gần dấu sao thứ 1<sup>st</sup> nhất là &#39;t&#39; trong &quot;lee<strong><u>t</u></strong>**cod*e&quot;. s trở thành &quot;lee*cod*e&quot;.
- Ký tự gần dấu sao thứ 2<sup>nd</sup> nhất là &#39;e&#39; trong &quot;le<strong><u>e</u></strong>*cod*e&quot;. s trở thành &quot;lecod*e&quot;.
- Ký tự gần dấu sao thứ 3<sup>rd</sup> nhất là &#39;d&#39; trong &quot;leco<strong><u>d</u></strong>*e&quot;. s trở thành &quot;lecoe&quot;.
Không còn dấu sao nào, nên ta trả về &quot;lecoe&quot;.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;erase*****&quot;
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong> Toàn bộ chuỗi bị xóa, nên ta trả về một chuỗi rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và các dấu sao <code>*</code>.</li>
	<li>Thao tác trên có thể được thực hiện trên <code>s</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng bằng Stack

<!-- thinking:start -->

> **Tư duy**
>
> Một dấu sao xóa chữ cái gần nhất về phía bên trái. $n \le 10^5$, vì vậy việc quét đi quét lại sẽ khiến cùng những ký tự đó bị sắp xếp lại nhiều lần.
>
> Một stack lưu các chữ cái vẫn còn tồn tại: thêm một chữ cái vào stack, lấy ra phần tử trên cùng khi gặp dấu sao. Ghép các phần tử trong stack sẽ cho đáp án.

<!-- thinking:end -->

Ta có thể dùng một stack để mô phỏng quá trình thao tác. Duyệt chuỗi $s$, nếu ký tự hiện tại không phải dấu sao thì thêm nó vào stack; nếu ký tự hiện tại là dấu sao thì lấy phần tử trên cùng của stack ra.

Cuối cùng, nối các phần tử trong stack thành một chuỗi và trả về chuỗi đó.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Không tính phần bộ nhớ dùng cho chuỗi kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeStars(self, s: str) -> str:
        ans = []
        for c in s:
            if c == '*':
                ans.pop()
            else:
                ans.append(c)
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String removeStars(String s) {
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) == '*') {
                ans.deleteCharAt(ans.length() - 1);
            } else {
                ans.append(s.charAt(i));
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string removeStars(string s) {
        string ans;
        for (char c : s) {
            if (c == '*') {
                ans.pop_back();
            } else {
                ans.push_back(c);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func removeStars(s string) string {
	ans := []rune{}
	for _, c := range s {
		if c == '*' {
			ans = ans[:len(ans)-1]
		} else {
			ans = append(ans, c)
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function removeStars(s: string): string {
    const ans: string[] = [];
    for (const c of s) {
        if (c === '*') {
            ans.pop();
        } else {
            ans.push(c);
        }
    }
    return ans.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn remove_stars(s: String) -> String {
        let mut ans = String::new();
        for &c in s.as_bytes().iter() {
            if c == b'*' {
                ans.pop();
            } else {
                ans.push(char::from(c));
            }
        }
        ans
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $s
     * @return String
     */
    function removeStars($s) {
        $ans = [];
        $n = strlen($s);
        for ($i = 0; $i < $n; $i++) {
            $c = $s[$i];
            if ($c === '*') {
                array_pop($ans);
            } else {
                $ans[] = $c;
            }
        }
        return implode('', $ans);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
