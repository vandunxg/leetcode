---
comments: true
difficulty: Hard
rating: 1949
source: Biweekly Contest 6 Q4
tags:
    - Graph
    - Hash Table
    - String
---

<!-- problem:start -->

# [1153. String Transforms Into Another String 🔒](https://leetcode.com/problems/string-transforms-into-another-string)

[中文文档](/solution/1100-1199/1153.String%20Transforms%20Into%20Another%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi có cùng độ dài là <code>str1</code> và <code>str2</code>. Hãy xác định liệu có thể biến đổi <code>str1</code> thành <code>str2</code> bằng <strong>không hoặc nhiều</strong> lần <em>chuyển đổi</em> hay không.</p>

<p>Trong mỗi lần chuyển đổi, bạn có thể đổi <strong>tất cả</strong> lần xuất hiện của một ký tự trong <code>str1</code> thành <strong>bất kỳ</strong> ký tự chữ thường tiếng Anh nào khác.</p>

<p>Trả về <code>true</code> khi và chỉ khi có thể biến đổi <code>str1</code> thành <code>str2</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> str1 = &quot;aabcc&quot;, str2 = &quot;ccdee&quot;
<strong>Output:</strong> true
<strong>Giải thích: </strong>Đổi &#39;c&#39; thành &#39;e&#39;, sau đó đổi &#39;b&#39; thành &#39;d&#39; rồi đổi &#39;a&#39; thành &#39;c&#39;. Lưu ý thứ tự chuyển đổi rất quan trọng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> str1 = &quot;leetcode&quot;, str2 = &quot;codeleet&quot;
<strong>Output:</strong> false
<strong>Giải thích: </strong>Không có cách nào biến đổi str1 thành str2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= str1.length == str2.length &lt;= 10<sup>4</sup></code></li>
	<li><code>str1</code> và <code>str2</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần chuyển đổi thay thế mọi lần xuất hiện của một ký tự, nên ánh xạ phải là một hàm: một ký tự nguồn không thể ánh xạ tới hai ký tự đích khác nhau. Nếu hai chuỗi bằng nhau thì đã đạt kết quả.
>
> Nếu không, cần một ký tự chưa được dùng làm chỗ tạm khi ánh xạ có chu trình. Nếu `str2` dùng đủ $26$ chữ cái thì không còn ký tự nào như vậy. Dùng map để lưu và kiểm tra ánh xạ.

<!-- thinking:end -->

Trước tiên, kiểm tra `str1` và `str2` có bằng nhau không. Nếu có, trả về `true` ngay.

Tiếp theo, đếm số chữ cái phân biệt trong `str2`. Nếu số lượng là $26$, tức `str2` chứa đủ mọi chữ cái thường. Khi đó, dù biến đổi `str1` thế nào cũng không thể thu được `str2`, nên trả về `false` ngay.

Nếu không, dùng mảng hoặc hash table `d` để ghi lại mỗi chữ cái trong `str1` được biến đổi thành chữ cái nào. Duyệt đồng thời `str1` và `str2`. Nếu một chữ cái trong `str1` đã có ánh xạ, chữ cái đích phải trùng với ký tự tương ứng trong `str2`; nếu không thì trả về `false`.

Sau khi duyệt xong, trả về `true`.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(C)$, với $n$ là độ dài chuỗi `str1` và $C$ là kích thước bảng ký tự. Trong bài này, $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canConvert(self, str1: str, str2: str) -> bool:
        if str1 == str2:
            return True
        if len(set(str2)) == 26:
            return False
        d = {}
        for a, b in zip(str1, str2):
            if a not in d:
                d[a] = b
            elif d[a] != b:
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean canConvert(String str1, String str2) {
        if (str1.equals(str2)) {
            return true;
        }
        int m = 0;
        int[] cnt = new int[26];
        int n = str1.length();
        for (int i = 0; i < n; ++i) {
            if (++cnt[str2.charAt(i) - 'a'] == 1) {
                ++m;
            }
        }
        if (m == 26) {
            return false;
        }
        int[] d = new int[26];
        for (int i = 0; i < n; ++i) {
            int a = str1.charAt(i) - 'a';
            int b = str2.charAt(i) - 'a';
            if (d[a] == 0) {
                d[a] = b + 1;
            } else if (d[a] != b + 1) {
                return false;
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
    bool canConvert(string str1, string str2) {
        if (str1 == str2) {
            return true;
        }
        int cnt[26]{};
        int m = 0;
        for (char& c : str2) {
            if (++cnt[c - 'a'] == 1) {
                ++m;
            }
        }
        if (m == 26) {
            return false;
        }
        int d[26]{};
        for (int i = 0; i < str1.size(); ++i) {
            int a = str1[i] - 'a';
            int b = str2[i] - 'a';
            if (d[a] == 0) {
                d[a] = b + 1;
            } else if (d[a] != b + 1) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func canConvert(str1 string, str2 string) bool {
	if str1 == str2 {
		return true
	}
	s := map[rune]bool{}
	for _, c := range str2 {
		s[c] = true
		if len(s) == 26 {
			return false
		}
	}
	d := [26]int{}
	for i, c := range str1 {
		a, b := int(c-'a'), int(str2[i]-'a')
		if d[a] == 0 {
			d[a] = b + 1
		} else if d[a] != b+1 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function canConvert(str1: string, str2: string): boolean {
    if (str1 === str2) {
        return true;
    }
    if (new Set(str2).size === 26) {
        return false;
    }
    const d: Map<string, string> = new Map();
    for (const [i, c] of str1.split('').entries()) {
        if (!d.has(c)) {
            d.set(c, str2[i]);
        } else if (d.get(c) !== str2[i]) {
            return false;
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
