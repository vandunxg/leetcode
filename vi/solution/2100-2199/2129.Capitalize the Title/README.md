---
comments: true
difficulty: Easy
rating: 1274
source: Biweekly Contest 69 Q1
tags:
    - String
---

<!-- problem:start -->

# [2129. Capitalize the Title](https://leetcode.com/problems/capitalize-the-title)

[中文文档](/solution/2100-2199/2129.Capitalize%20the%20Title/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>title</code> gồm một hoặc nhiều từ, các từ được phân tách bằng một dấu cách, mỗi từ chỉ gồm các chữ cái tiếng Anh. Hãy <strong>viết hoa</strong> chuỗi bằng cách thay đổi kiểu viết hoa của từng từ theo các quy tắc sau:</p>

<ul>
	<li>Nếu từ có độ dài là <code>1</code> hoặc <code>2</code> chữ cái, chuyển tất cả chữ cái thành chữ thường.</li>
	<li>Nếu không, chuyển chữ cái đầu tiên thành chữ hoa và các chữ cái còn lại thành chữ thường.</li>
</ul>

<p>Trả về <em><code>title</code> đã được <strong>viết hoa</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> title = &quot;capiTalIze tHe titLe&quot;
<strong>Đầu ra:</strong> &quot;Capitalize The Title&quot;
<strong>Giải thích:</strong>
Vì tất cả các từ đều có độ dài ít nhất là 3, chữ cái đầu tiên của mỗi từ được viết hoa, còn các chữ cái còn lại được viết thường.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> title = &quot;First leTTeR of EACH Word&quot;
<strong>Đầu ra:</strong> &quot;First Letter of Each Word&quot;
<strong>Giải thích:</strong>
Từ &quot;of&quot; có độ dài là 2, nên toàn bộ từ được viết thường.
Các từ còn lại có độ dài ít nhất là 3, nên chữ cái đầu tiên của mỗi từ được viết hoa, còn các chữ cái còn lại được viết thường.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> title = &quot;i lOve leetcode&quot;
<strong>Đầu ra:</strong> &quot;i Love Leetcode&quot;
<strong>Giải thích:</strong>
Từ &quot;i&quot; có độ dài là 1, nên được viết thường.
Các từ còn lại có độ dài ít nhất là 3, nên chữ cái đầu tiên của mỗi từ được viết hoa, còn các chữ cái còn lại được viết thường.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= title.length &lt;= 100</code></li>
	<li><code>title</code> gồm các từ được phân tách bằng một dấu cách, không có dấu cách ở đầu hoặc cuối.</li>
	<li>Mỗi từ chỉ gồm các chữ cái tiếng Anh viết hoa hoặc viết thường và là <strong>không rỗng</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi tách theo dấu cách, các từ ngắn hơn $3$ được chuyển thành chữ thường, còn các từ dài hơn được viết hoa chữ cái đầu. Quy tắc này độc lập trên từng từ, nên chỉ cần mô phỏng trực tiếp.
>
> Chuyển mọi token thành chữ thường, viết hoa các token có độ dài ít nhất $3$, rồi nối chúng bằng dấu cách.
>
> Độ phức tạp là tuyến tính theo độ dài của title.

<!-- thinking:end -->

Mô phỏng trực tiếp quá trình. Tách chuỗi theo dấu cách để lấy từng từ, sau đó chuyển mỗi từ về kiểu chữ phù hợp theo đề bài. Cuối cùng, nối các từ bằng dấu cách.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi `title`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def capitalizeTitle(self, title: str) -> str:
        words = [w.lower() if len(w) < 3 else w.capitalize() for w in title.split()]
        return " ".join(words)
```

#### Java

```java
class Solution {
    public String capitalizeTitle(String title) {
        List<String> ans = new ArrayList<>();
        for (String s : title.split(" ")) {
            if (s.length() < 3) {
                ans.add(s.toLowerCase());
            } else {
                ans.add(s.substring(0, 1).toUpperCase() + s.substring(1).toLowerCase());
            }
        }
        return String.join(" ", ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string capitalizeTitle(string title) {
        transform(title.begin(), title.end(), title.begin(), ::tolower);
        istringstream ss(title);
        string ans;
        while (ss >> title) {
            if (title.size() > 2) {
                title[0] = toupper(title[0]);
            }
            ans += title;
            ans += " ";
        }
        ans.pop_back();
        return ans;
    }
};
```

#### Go

```go
func capitalizeTitle(title string) string {
	title = strings.ToLower(title)
	words := strings.Split(title, " ")
	for i, s := range words {
		if len(s) > 2 {
			words[i] = strings.Title(s)
		}
	}
	return strings.Join(words, " ")
}
```

#### TypeScript

```ts
function capitalizeTitle(title: string): string {
    return title
        .split(' ')
        .map(s =>
            s.length < 3 ? s.toLowerCase() : s.slice(0, 1).toUpperCase() + s.slice(1).toLowerCase(),
        )
        .join(' ');
}
```

#### C#

```cs
public class Solution {
    public string CapitalizeTitle(string title) {
        List<string> ans = new List<string>();
        foreach (string s in title.Split(' ')) {
            if (s.Length < 3) {
                ans.Add(s.ToLower());
            } else {
                ans.Add(char.ToUpper(s[0]) + s.Substring(1).ToLower());
            }
        }
        return string.Join(" ", ans);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
