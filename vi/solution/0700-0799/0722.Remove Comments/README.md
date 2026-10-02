---
comments: true
difficulty: Medium
tags:
    - Array
    - String
---

<!-- problem:start -->

# [722. Remove Comments](https://leetcode.com/problems/remove-comments)

[中文文档](/solution/0700-0799/0722.Remove%20Comments/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chương trình C++, hãy xóa các comment khỏi chương trình. Source của chương trình là mảng chuỗi <code>source</code>, trong đó <code>source[i]</code> là dòng thứ <code>i<sup>th</sup></code> của source code. Mảng này được tạo bằng cách tách chuỗi source code ban đầu tại ký tự xuống dòng <code>&#39;\n&#39;</code>.</p>

<p>Trong C++ có hai loại comment: comment một dòng và comment dạng block.</p>

<ul>
	<li>Chuỗi <code>&quot;//&quot;</code> đánh dấu comment một dòng; bản thân chuỗi này và mọi ký tự phía sau nó trên cùng dòng đều bị bỏ qua.</li>
	<li>Chuỗi <code>&quot;/*&quot;</code> đánh dấu comment dạng block; mọi ký tự cho đến lần xuất hiện tiếp theo (không chồng lấp) của <code>&quot;*/&quot;</code> đều bị bỏ qua. (Các lần xuất hiện được xét theo thứ tự đọc: từ trái sang phải, lần lượt từng dòng.) Lưu ý, chuỗi <code>&quot;/*/&quot;</code> chưa kết thúc comment block vì dấu kết thúc sẽ chồng lấp với dấu bắt đầu.</li>
</ul>

<p>Comment hợp lệ đầu tiên được ưu tiên xử lý.</p>

<ul>
	<li>Ví dụ, nếu chuỗi <code>&quot;//&quot;</code> nằm trong comment block thì nó bị bỏ qua.</li>
	<li>Tương tự, nếu chuỗi <code>&quot;/*&quot;</code> nằm trong comment một dòng hoặc comment block thì nó cũng bị bỏ qua.</li>
</ul>

<p>Nếu một dòng code trở thành rỗng sau khi xóa comment, không đưa dòng đó vào kết quả: mọi chuỗi trong danh sách kết quả đều không rỗng.</p>

<p>Đầu vào không chứa ký tự điều khiển, dấu nháy đơn hoặc dấu nháy kép.</p>

<ul>
	<li>Ví dụ, <code>source = &quot;string s = &quot;/* Not a comment. */&quot;;&quot;</code> sẽ không xuất hiện trong test case.</li>
</ul>

<p>Ngoài ra, không có thành phần nào khác như define hay macro ảnh hưởng đến comment.</p>

<p>Đảm bảo mọi comment block đã mở cuối cùng đều được đóng, vì vậy <code>&quot;/*&quot;</code> nằm ngoài comment một dòng hoặc comment block luôn bắt đầu một comment mới.</p>

<p>Cuối cùng, comment block có thể xóa cả ký tự xuống dòng ngầm định. Xem ví dụ bên dưới để biết thêm chi tiết.</p>

<p>Sau khi xóa comment khỏi source code, hãy trả về <em>source code theo cùng định dạng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> source = [&quot;/*Test program */&quot;, &quot;int main()&quot;, &quot;{ &quot;, &quot;  // variable declaration &quot;, &quot;int a, b, c;&quot;, &quot;/* This is a test&quot;, &quot;   multiline  &quot;, &quot;   comment for &quot;, &quot;   testing */&quot;, &quot;a = b + c;&quot;, &quot;}&quot;]
<strong>Đầu ra:</strong> [&quot;int main()&quot;,&quot;{ &quot;,&quot;  &quot;,&quot;int a, b, c;&quot;,&quot;a = b + c;&quot;,&quot;}&quot;]
<strong>Giải thích:</strong> Code theo từng dòng được minh họa như sau:
/*Test program */
int main()
{ 
  // variable declaration 
int a, b, c;
/* This is a test
   multiline  
   comment for 
   testing */
a = b + c;
}
Chuỗi /* đánh dấu comment block, bao gồm dòng 1 và các dòng 6-9. Chuỗi // đánh dấu comment một dòng ở dòng 4.
Code đầu ra theo từng dòng được minh họa như sau:
int main()
{ 
  
int a, b, c;
a = b + c;
}
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> source = [&quot;a/*comment&quot;, &quot;line&quot;, &quot;more_comment*/b&quot;]
<strong>Đầu ra:</strong> [&quot;ab&quot;]
<strong>Giải thích:</strong> Chuỗi source ban đầu là &quot;a/*comment\nline\nmore_comment*/b&quot;, trong đó các ký tự xuống dòng được in đậm. Sau khi xóa comment, các ký tự xuống dòng ngầm định cũng bị xóa, còn lại chuỗi &quot;ab&quot;; khi tách chuỗi này bằng ký tự xuống dòng, ta được [&quot;ab&quot;].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= source.length &lt;= 100</code></li>
	<li><code>0 &lt;= source[i].length &lt;= 80</code></li>
	<li><code>source[i]</code> chỉ gồm các ký tự <strong>ASCII</strong> có thể in được.</li>
	<li>Mọi comment block đã mở cuối cùng đều được đóng.</li>
	<li>Đầu vào không chứa dấu nháy đơn hoặc&nbsp;dấu nháy kép.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích các trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Xóa comment một dòng và comment block, đồng thời giữ các ký tự còn hiệu lực, kể cả khi chúng được nối qua một comment block. Kích thước đầu vào không lớn; điểm cần xử lý là trạng thái đang ở trong block hay không, và việc `//` loại bỏ phần còn lại của dòng.
>
> Comment block có thể kéo dài qua nhiều dòng nên cần giữ trạng thái giữa các dòng. Cả hai loại comment đều bắt đầu bằng hai ký tự, vì vậy scanner cần nhận diện `/*`, `*/` và `//` trước khi xem ký tự là một phần của source code.
>
> Duy trì $\textit{blockComment}$ và buffer $t$. Bên trong block, chỉ tìm dấu kết thúc; bên ngoài, `//` kết thúc dòng, `/*` bắt đầu block, còn trường hợp khác thì thêm ký tự vào buffer. Chỉ đưa $t$ vào kết quả khi dòng kết thúc ngoài block và $t$ không rỗng; cách này cũng nối các phần nằm ở hai phía của block.

<!-- thinking:end -->

Ta dùng biến $\textit{blockComment}$ để cho biết hiện tại có đang ở trong comment block hay không. Ban đầu, $\textit{blockComment}$ là `false`. Biến $t$ dùng để lưu các ký tự hợp lệ của dòng hiện tại.

Tiếp theo, ta duyệt từng dòng và xử lý các trường hợp sau:

Nếu đang ở trong comment block và ký tự hiện tại cùng ký tự tiếp theo tạo thành `'*/'`, comment block kết thúc. Ta đặt $\textit{blockComment}$ thành `false` và bỏ qua hai ký tự này. Nếu không, tiếp tục giữ trạng thái comment block và không làm gì thêm.

Nếu không ở trong comment block và ký tự hiện tại cùng ký tự tiếp theo tạo thành `'/*'`, một comment block bắt đầu. Ta đặt $\textit{blockComment}$ thành `true` và bỏ qua hai ký tự này. Nếu chúng tạo thành `'//'`, comment một dòng bắt đầu và ta kết thúc việc duyệt dòng hiện tại. Nếu không thuộc các trường hợp trên, ký tự hiện tại hợp lệ và được thêm vào $t$.

Sau khi duyệt xong dòng hiện tại, nếu $\textit{blockComment}$ là `false` và $t$ không rỗng, dòng hiện tại có nội dung hợp lệ. Ta thêm dòng đó vào mảng kết quả rồi xóa nội dung $t$. Sau đó tiếp tục duyệt dòng kế tiếp.

Độ phức tạp thời gian là $O(L)$ và độ phức tạp không gian là $O(L)$, với $L$ là tổng độ dài source code.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeComments(self, source: List[str]) -> List[str]:
        ans = []
        t = []
        block_comment = False
        for s in source:
            i, m = 0, len(s)
            while i < m:
                if block_comment:
                    if i + 1 < m and s[i : i + 2] == "*/":
                        block_comment = False
                        i += 1
                else:
                    if i + 1 < m and s[i : i + 2] == "/*":
                        block_comment = True
                        i += 1
                    elif i + 1 < m and s[i : i + 2] == "//":
                        break
                    else:
                        t.append(s[i])
                i += 1
            if not block_comment and t:
                ans.append("".join(t))
                t.clear()
        return ans
```

#### Java

```java
class Solution {
    public List<String> removeComments(String[] source) {
        List<String> ans = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        boolean blockComment = false;
        for (String s : source) {
            int m = s.length();
            for (int i = 0; i < m; ++i) {
                if (blockComment) {
                    if (i + 1 < m && s.charAt(i) == '*' && s.charAt(i + 1) == '/') {
                        blockComment = false;
                        ++i;
                    }
                } else {
                    if (i + 1 < m && s.charAt(i) == '/' && s.charAt(i + 1) == '*') {
                        blockComment = true;
                        ++i;
                    } else if (i + 1 < m && s.charAt(i) == '/' && s.charAt(i + 1) == '/') {
                        break;
                    } else {
                        sb.append(s.charAt(i));
                    }
                }
            }
            if (!blockComment && sb.length() > 0) {
                ans.add(sb.toString());
                sb.setLength(0);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> removeComments(vector<string>& source) {
        vector<string> ans;
        string t;
        bool blockComment = false;
        for (auto& s : source) {
            int m = s.size();
            for (int i = 0; i < m; ++i) {
                if (blockComment) {
                    if (i + 1 < m && s[i] == '*' && s[i + 1] == '/') {
                        blockComment = false;
                        ++i;
                    }
                } else {
                    if (i + 1 < m && s[i] == '/' && s[i + 1] == '*') {
                        blockComment = true;
                        ++i;
                    } else if (i + 1 < m && s[i] == '/' && s[i + 1] == '/') {
                        break;
                    } else {
                        t.push_back(s[i]);
                    }
                }
            }
            if (!blockComment && !t.empty()) {
                ans.emplace_back(t);
                t.clear();
            }
        }
        return ans;
    }
};
```

#### Go

```go
func removeComments(source []string) (ans []string) {
	t := []byte{}
	blockComment := false
	for _, s := range source {
		m := len(s)
		for i := 0; i < m; i++ {
			if blockComment {
				if i+1 < m && s[i] == '*' && s[i+1] == '/' {
					blockComment = false
					i++
				}
			} else {
				if i+1 < m && s[i] == '/' && s[i+1] == '*' {
					blockComment = true
					i++
				} else if i+1 < m && s[i] == '/' && s[i+1] == '/' {
					break
				} else {
					t = append(t, s[i])
				}
			}
		}
		if !blockComment && len(t) > 0 {
			ans = append(ans, string(t))
			t = []byte{}
		}
	}
	return
}
```

#### TypeScript

```ts
function removeComments(source: string[]): string[] {
    const ans: string[] = [];
    const t: string[] = [];
    let blockComment = false;
    for (const s of source) {
        const m = s.length;
        for (let i = 0; i < m; ++i) {
            if (blockComment) {
                if (i + 1 < m && s.slice(i, i + 2) === '*/') {
                    blockComment = false;
                    ++i;
                }
            } else {
                if (i + 1 < m && s.slice(i, i + 2) === '/*') {
                    blockComment = true;
                    ++i;
                } else if (i + 1 < m && s.slice(i, i + 2) === '//') {
                    break;
                } else {
                    t.push(s[i]);
                }
            }
        }
        if (!blockComment && t.length) {
            ans.push(t.join(''));
            t.length = 0;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn remove_comments(source: Vec<String>) -> Vec<String> {
        let mut ans: Vec<String> = Vec::new();
        let mut t: Vec<String> = Vec::new();
        let mut blockComment = false;

        for s in &source {
            let m = s.len();
            let mut i = 0;
            while i < m {
                if blockComment {
                    if i + 1 < m && &s[i..i + 2] == "*/" {
                        blockComment = false;
                        i += 2;
                    } else {
                        i += 1;
                    }
                } else {
                    if i + 1 < m && &s[i..i + 2] == "/*" {
                        blockComment = true;
                        i += 2;
                    } else if i + 1 < m && &s[i..i + 2] == "//" {
                        break;
                    } else {
                        t.push(s.chars().nth(i).unwrap().to_string());
                        i += 1;
                    }
                }
            }
            if !blockComment && !t.is_empty() {
                ans.push(t.join(""));
                t.clear();
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
