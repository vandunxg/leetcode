---
comments: true
difficulty: Easy
rating: 1316
source: Weekly Contest 454 Q1
tags:
    - String
    - Simulation
---

<!-- problem:start -->

# [3582. Generate Tag for Video Caption](https://leetcode.com/problems/generate-tag-for-video-caption)

[中文文档](/solution/3500-3599/3582.Generate%20Tag%20for%20Video%20Caption/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code><font face="monospace">caption</font></code> biểu diễn chú thích cho một video.</p>

<p>Để tạo một <strong>thẻ hợp lệ</strong> cho video, cần thực hiện các thao tác sau <strong>theo đúng thứ tự</strong>:</p>

<ol>
	<li>
	<p><strong>Gộp tất cả các từ</strong> trong chuỗi thành một <em>chuỗi camelCase</em> duy nhất, thêm tiền tố <code>&#39;#&#39;</code>. <em>Chuỗi camelCase</em> là chuỗi trong đó chữ cái đầu tiên của mọi từ <em>trừ từ đầu tiên</em> được viết hoa. Tất cả các ký tự sau ký tự đầu tiên trong <strong>mỗi</strong> từ phải được viết thường.</p>
	</li>
	<li>
	<p><b>Xóa</b> mọi ký tự không phải là chữ cái tiếng Anh, <strong>ngoại trừ</strong> ký tự <code>&#39;#&#39;</code> đầu tiên.</p>
	</li>
	<li>
	<p><strong>Cắt ngắn</strong> kết quả xuống tối đa 100 ký tự.</p>
	</li>
</ol>

<p>Trả về <strong>thẻ</strong> sau khi thực hiện các thao tác trên <code>caption</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">caption = &quot;Leetcode daily streak achieved&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;#leetcodeDailyStreakAchieved&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chữ cái đầu tiên của mọi từ, ngoại trừ <code>&quot;leetcode&quot;</code>, cần được viết hoa.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">caption = &quot;can I Go There&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;#canIGoThere&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chữ cái đầu tiên của mọi từ, ngoại trừ <code>&quot;can&quot;</code>, cần được viết hoa.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">caption = &quot;hhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhh&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;#hhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhh&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì từ đầu tiên có độ dài 101, ta cần cắt bỏ hai chữ cái cuối của từ này.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= caption.length &lt;= 150</code></li>
	<li><code>caption</code> chỉ gồm các chữ cái tiếng Anh và <code>&#39; &#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Thẻ gồm `#` cộng với chuỗi camel case: từ đầu tiên viết thường, các từ sau viết hoa chữ cái đầu, tổng độ dài không quá $100$. Tách theo khoảng trắng, dùng `capitalize` cho mỗi từ, chuyển từ đầu tiên thành chữ thường, nối lại và giữ $99$ chữ cái sau `#`.
>
> Với caption rỗng, kết quả chỉ là `#`. Chỉ cần tách một lần.

<!-- thinking:end -->

Đầu tiên, chúng ta tách chuỗi tiêu đề thành các từ, sau đó xử lý từng từ. Từ đầu tiên cần được viết thường toàn bộ, còn với các từ tiếp theo, chữ cái đầu tiên được viết hoa và phần còn lại được viết thường. Tiếp theo, chúng ta nối tất cả các từ đã xử lý rồi thêm ký hiệu # ở đầu. Cuối cùng, nếu thẻ được tạo ra dài hơn 100 ký tự, chúng ta cắt lấy 100 ký tự đầu tiên.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi tiêu đề.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def generateTag(self, caption: str) -> str:
        words = [s.capitalize() for s in caption.split()]
        if words:
            words[0] = words[0].lower()
        return "#" + "".join(words)[:99]
```

#### Java

```java
class Solution {
    public String generateTag(String caption) {
        String[] words = caption.trim().split("\\s+");
        StringBuilder sb = new StringBuilder("#");

        for (int i = 0; i < words.length; i++) {
            String word = words[i];
            if (word.isEmpty()) {
                continue;
            }

            word = word.toLowerCase();
            if (i == 0) {
                sb.append(word);
            } else {
                sb.append(Character.toUpperCase(word.charAt(0)));
                if (word.length() > 1) {
                    sb.append(word.substring(1));
                }
            }

            if (sb.length() >= 100) {
                break;
            }
        }

        return sb.length() > 100 ? sb.substring(0, 100) : sb.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string generateTag(string caption) {
        istringstream iss(caption);
        string word;
        ostringstream oss;
        oss << "#";
        bool first = true;
        while (iss >> word) {
            transform(word.begin(), word.end(), word.begin(), ::tolower);
            if (first) {
                oss << word;
                first = false;
            } else {
                word[0] = toupper(word[0]);
                oss << word;
            }
            if (oss.str().length() >= 100) {
                break;
            }
        }

        string ans = oss.str();
        if (ans.length() > 100) {
            ans = ans.substr(0, 100);
        }
        return ans;
    }
};
```

#### Go

```go
func generateTag(caption string) string {
	words := strings.Fields(caption)
	var builder strings.Builder
	builder.WriteString("#")

	for i, word := range words {
		word = strings.ToLower(word)
		if i == 0 {
			builder.WriteString(word)
		} else {
			runes := []rune(word)
			if len(runes) > 0 {
				runes[0] = unicode.ToUpper(runes[0])
			}
			builder.WriteString(string(runes))
		}
		if builder.Len() >= 100 {
			break
		}
	}

	ans := builder.String()
	if len(ans) > 100 {
		ans = ans[:100]
	}
	return ans
}
```

#### TypeScript

```ts
function generateTag(caption: string): string {
    const words = caption.trim().split(/\s+/);
    let ans = '#';
    for (let i = 0; i < words.length; i++) {
        const word = words[i].toLowerCase();
        if (i === 0) {
            ans += word;
        } else {
            ans += word.charAt(0).toUpperCase() + word.slice(1);
        }
        if (ans.length >= 100) {
            ans = ans.slice(0, 100);
            break;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
