---
comments: true
difficulty: Hard
tags:
    - Stack
    - String
---

<!-- problem:start -->

# [591. Tag Validator](https://leetcode.com/problems/tag-validator)

[中文文档](/solution/0500-0599/0591.Tag%20Validator/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi biểu diễn một đoạn mã, hãy triển khai bộ kiểm tra thẻ để phân tích mã và xác định nó có hợp lệ hay không.</p>

<p>Đoạn mã hợp lệ nếu thỏa mãn tất cả các quy tắc sau:</p>

<ol>
	<li>Mã phải được bao trong một <b>cặp thẻ mở/đóng hợp lệ</b>. Nếu không, mã không hợp lệ.</li>
	<li>Một <b>cặp thẻ mở/đóng</b> (chưa chắc hợp lệ) có đúng định dạng sau: <code>&lt;TAG_NAME&gt;TAG_CONTENT&lt;/TAG_NAME&gt;</code>. Trong đó, <code>&lt;TAG_NAME&gt;</code> là thẻ mở và <code>&lt;/TAG_NAME&gt;</code> là thẻ đóng. TAG_NAME trong thẻ mở và thẻ đóng phải giống nhau. Cặp thẻ này <b>hợp lệ</b> khi và chỉ khi TAG_NAME và TAG_CONTENT đều hợp lệ.</li>
	<li><code>TAG_NAME</code> <b>hợp lệ</b> chỉ chứa <b>chữ cái viết hoa</b> và có độ dài trong khoảng [1,9]. Nếu không, <code>TAG_NAME</code> <b>không hợp lệ</b>.</li>
	<li><code>TAG_CONTENT</code> <b>hợp lệ</b> có thể chứa các <b>cặp thẻ hợp lệ</b> khác, <b>cdata</b> và mọi ký tự (xem lưu ý 1), <b>NGOẠI TRỪ</b> ký tự <code>&lt;</code> không khớp, thẻ mở hoặc thẻ đóng không khớp, và thẻ không khớp hoặc thẻ đóng có TAG_NAME không hợp lệ. Nếu không, <code>TAG_CONTENT</code> <b>không hợp lệ</b>.</li>
	<li>Thẻ mở không khớp nếu không có thẻ đóng nào cùng TAG_NAME, và ngược lại. Ngoài ra, cần xét trường hợp các thẻ lồng nhau không cân bằng.</li>
	<li>Ký tự <code>&lt;</code> không khớp nếu không tìm thấy ký tự <code>&gt;</code> nào theo sau. Khi gặp <code>&lt;</code> hoặc <code>&lt;/</code>, mọi ký tự tiếp theo cho đến <code>&gt;</code> kế tiếp đều phải được phân tích thành TAG_NAME (chưa chắc hợp lệ).</li>
	<li>cdata có định dạng sau: <code>&lt;![CDATA[CDATA_CONTENT]]&gt;</code>. <code>CDATA_CONTENT</code> là các ký tự nằm giữa <code>&lt;![CDATA[</code> và chuỗi <code>]]&gt;</code> <b>đầu tiên xuất hiện sau đó</b>.</li>
	<li><code>CDATA_CONTENT</code> có thể chứa <b>bất kỳ ký tự nào</b>. cdata ngăn bộ kiểm tra phân tích nội dung <code>CDATA_CONTENT</code>; vì vậy, dù phần này có chứa ký tự có thể được phân tích thành thẻ (hợp lệ hay không), hãy xem chúng là <b>ký tự thông thường</b>.</li>
</ol>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> code = &quot;&lt;DIV&gt;This is the first line &lt;![CDATA[&lt;div&gt;]]&gt;&lt;/DIV&gt;&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 
Mã được bao trong một cặp thẻ: &lt;DIV&gt; và &lt;/DIV&gt;. 
TAG_NAME hợp lệ, còn TAG_CONTENT gồm một số ký tự và cdata. 
Dù CDATA_CONTENT chứa một thẻ mở không khớp với TAG_NAME không hợp lệ, nội dung này vẫn được xem là văn bản thường, không được phân tích thành thẻ.
Do đó TAG_CONTENT hợp lệ, nên đoạn mã cũng hợp lệ. Kết quả là true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> code = &quot;&lt;DIV&gt;&gt;&gt;  ![cdata[]] &lt;![CDATA[&lt;div&gt;]&gt;]]&gt;]]&gt;&gt;]&lt;/DIV&gt;&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Trước tiên, ta tách mã thành: start_tag|tag_content|end_tag.
start_tag -&gt; <b>&quot;&lt;DIV&gt;&quot;</b>
end_tag -&gt; <b>&quot;&lt;/DIV&gt;&quot;</b>
tag_content cũng có thể được tách thành: text1|cdata|text2.
text1 -&gt; <b>&quot;&gt;&gt;  ![cdata[]] &quot;</b>
cdata -&gt; <b>&quot;&lt;![CDATA[&lt;div&gt;]&gt;]]&gt;&quot;</b>, trong đó CDATA_CONTENT là <b>&quot;&lt;div&gt;]&gt;&quot;</b>
text2 -&gt; <b>&quot;]]&gt;&gt;]&quot;</b>
start_tag KHÔNG phải là <b>&quot;&lt;DIV&gt;&gt;&gt;&quot;</b> vì quy tắc 6.
cdata KHÔNG phải là <b>&quot;&lt;![CDATA[&lt;div&gt;]&gt;]]&gt;]]&gt;&quot;</b> vì quy tắc 7.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> code = &quot;&lt;A&gt;  &lt;B&gt; &lt;/A&gt;   &lt;/B&gt;&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không cân bằng. Nếu &quot;&lt;A&gt;&quot; được đóng thì &quot;&lt;B&gt;&quot; sẽ không khớp, và ngược lại.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= code.length &lt;= 500</code></li>
	<li><code>code</code> chỉ gồm chữ cái tiếng Anh, chữ số và các ký tự <code>&#39;&lt;&#39;</code>, <code>&#39;&gt;&#39;</code>, <code>&#39;/&#39;</code>, <code>&#39;!&#39;</code>, <code>&#39;[&#39;</code>, <code>&#39;]&#39;</code>, <code>&#39;.&#39;</code> và <code>&#39; &#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các tag phải lồng đúng thứ tự, CDATA không được phân tích, và toàn bộ chuỗi phải tạo thành một cấu trúc thẻ lồng nhau hoàn chỉnh. Regex khó xử lý cấu trúc lồng nhau và CDATA.
>
> Stack lưu tên các thẻ đang mở. Gặp `<![CDATA[` thì bỏ qua đến `]]>`; thẻ đóng phải khớp với phần tử trên cùng của stack; tên thẻ mở gồm từ một đến chín chữ cái viết hoa. Nếu stack rỗng khi chuỗi chưa kết thúc thì có văn bản thừa. Khi kết thúc, stack phải rỗng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isValid(self, code: str) -> bool:
        def check(tag):
            return 1 <= len(tag) <= 9 and all(c.isupper() for c in tag)

        stk = []
        i, n = 0, len(code)
        while i < n:
            if i and not stk:
                return False
            if code[i : i + 9] == '<![CDATA[':
                i = code.find(']]>', i + 9)
                if i < 0:
                    return False
                i += 2
            elif code[i : i + 2] == '</':
                j = i + 2
                i = code.find('>', j)
                if i < 0:
                    return False
                t = code[j:i]
                if not check(t) or not stk or stk.pop() != t:
                    return False
            elif code[i] == '<':
                j = i + 1
                i = code.find('>', j)
                if i < 0:
                    return False
                t = code[j:i]
                if not check(t):
                    return False
                stk.append(t)
            i += 1
        return not stk
```

#### Java

```java
class Solution {
    public boolean isValid(String code) {
        Deque<String> stk = new ArrayDeque<>();
        for (int i = 0; i < code.length(); ++i) {
            if (i > 0 && stk.isEmpty()) {
                return false;
            }
            if (code.startsWith("<![CDATA[", i)) {
                i = code.indexOf("]]>", i + 9);
                if (i < 0) {
                    return false;
                }
                i += 2;
            } else if (code.startsWith("</", i)) {
                int j = i + 2;
                i = code.indexOf(">", j);
                if (i < 0) {
                    return false;
                }
                String t = code.substring(j, i);
                if (!check(t) || stk.isEmpty() || !stk.pop().equals(t)) {
                    return false;
                }
            } else if (code.startsWith("<", i)) {
                int j = i + 1;
                i = code.indexOf(">", j);
                if (i < 0) {
                    return false;
                }
                String t = code.substring(j, i);
                if (!check(t)) {
                    return false;
                }
                stk.push(t);
            }
        }
        return stk.isEmpty();
    }

    private boolean check(String tag) {
        int n = tag.length();
        if (n < 1 || n > 9) {
            return false;
        }
        for (char c : tag.toCharArray()) {
            if (!Character.isUpperCase(c)) {
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
    bool isValid(string code) {
        stack<string> stk;
        for (int i = 0; i < code.size(); ++i) {
            if (i && stk.empty()) return false;
            if (code.substr(i, 9) == "<![CDATA[") {
                i = code.find("]]>", i + 9);
                if (i < 0) return false;
                i += 2;
            } else if (code.substr(i, 2) == "</") {
                int j = i + 2;
                i = code.find('>', j);
                if (i < 0) return false;
                string t = code.substr(j, i - j);
                if (!check(t) || stk.empty() || stk.top() != t) return false;
                stk.pop();
            } else if (code.substr(i, 1) == "<") {
                int j = i + 1;
                i = code.find('>', j);
                if (i < 0) return false;
                string t = code.substr(j, i - j);
                if (!check(t)) return false;
                stk.push(t);
            }
        }
        return stk.empty();
    }

    bool check(string tag) {
        int n = tag.size();
        if (n < 1 || n > 9) return false;
        for (char& c : tag)
            if (!isupper(c))
                return false;
        return true;
    }
};
```

#### Go

```go
func isValid(code string) bool {
	var stk []string
	for i := 0; i < len(code); i++ {
		if i > 0 && len(stk) == 0 {
			return false
		}
		if strings.HasPrefix(code[i:], "<![CDATA[") {
			n := strings.Index(code[i+9:], "]]>")
			if n == -1 {
				return false
			}
			i += n + 11
		} else if strings.HasPrefix(code[i:], "</") {
			if len(stk) == 0 {
				return false
			}
			j := i + 2
			n := strings.IndexByte(code[j:], '>')
			if n == -1 {
				return false
			}
			t := code[j : j+n]
			last := stk[len(stk)-1]
			stk = stk[:len(stk)-1]
			if !check(t) || last != t {
				return false
			}
			i += n + 2
		} else if strings.HasPrefix(code[i:], "<") {
			j := i + 1
			n := strings.IndexByte(code[j:], '>')
			if n == -1 {
				return false
			}
			t := code[j : j+n]
			if !check(t) {
				return false
			}
			stk = append(stk, t)
			i += n + 1
		}
	}
	return len(stk) == 0
}

func check(tag string) bool {
	n := len(tag)
	if n < 1 || n > 9 {
		return false
	}
	for _, c := range tag {
		if c < 'A' || c > 'Z' {
			return false
		}
	}
	return true
}
```

#### Rust

```rust
impl Solution {
    pub fn is_valid(code: String) -> bool {
        fn check(tag: &str) -> bool {
            let n = tag.len();
            n >= 1 && n <= 9 && tag.as_bytes().iter().all(|b| b.is_ascii_uppercase())
        }

        let mut stk = Vec::new();
        let mut i = 0;
        while i < code.len() {
            if i > 0 && stk.is_empty() {
                return false;
            }
            if code[i..].starts_with("<![CDATA[") {
                match code[i + 9..].find("]]>") {
                    Some(n) => {
                        i += n + 11;
                    }
                    None => {
                        return false;
                    }
                };
            } else if code[i..].starts_with("</") {
                let j = i + 2;
                match code[j..].find('>') {
                    Some(n) => {
                        let t = &code[j..j + n];
                        if !check(t) || stk.is_empty() || stk.pop().unwrap() != t {
                            return false;
                        }
                        i += n + 2;
                    }
                    None => {
                        return false;
                    }
                };
            } else if code[i..].starts_with("<") {
                let j = i + 1;
                match code[j..].find('>') {
                    Some(n) => {
                        let t = &code[j..j + n];
                        if !check(t) {
                            return false;
                        }
                        stk.push(t);
                    }
                    None => {
                        return false;
                    }
                };
            }
            i += 1;
        }
        stk.is_empty()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
