---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [500. Keyboard Row](https://leetcode.com/problems/keyboard-row)

[中文文档](/solution/0500-0599/0500.Keyboard%20Row/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>words</code>, hãy trả về <em>những từ có thể được gõ bằng các chữ cái chỉ nằm trên một hàng của bàn phím Mỹ như hình bên dưới</em>.</p>

<p><strong>Lưu ý</strong> rằng các chuỗi <strong>không phân biệt chữ hoa chữ thường</strong>; chữ hoa và chữ thường của cùng một chữ cái được xem là nằm trên cùng một hàng.</p>

<p>Trên <strong>bàn phím Mỹ</strong>:</p>

<ul>
	<li>hàng đầu tiên gồm các ký tự <code>&quot;qwertyuiop&quot;</code>,</li>
	<li>hàng thứ hai gồm các ký tự <code>&quot;asdfghjkl&quot;</code>, và</li>
	<li>hàng thứ ba gồm các ký tự <code>&quot;zxcvbnm&quot;</code>.</li>
</ul>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0500.Keyboard%20Row/images/keyboard.png" style="width: 800px; max-width: 600px; height: 267px;" />
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;Hello&quot;,&quot;Alaska&quot;,&quot;Dad&quot;,&quot;Peace&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;Alaska&quot;,&quot;Dad&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cả <code>&quot;a&quot;</code> và <code>&quot;A&quot;</code> đều nằm trên hàng thứ hai của bàn phím Mỹ vì không phân biệt chữ hoa chữ thường.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;omk&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;adsdf&quot;,&quot;sfd&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;adsdf&quot;,&quot;sfd&quot;]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 20</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 100</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh (cả chữ thường và chữ hoa).&nbsp;</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Kiểm tra bằng set

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra từng chữ cái của mỗi từ với ba hàng phím tốn thời gian tuyến tính theo tổng số chữ cái, phù hợp với các ràng buộc. Tạo lại set ký tự của một hàng ở mỗi lần kiểm tra sẽ lặp lại cùng một công việc.
>
> Kết quả chỉ phụ thuộc vào việc các chữ cái của từ có nằm trên cùng một hàng hay không, chứ không phụ thuộc thứ tự của chúng. Lưu ba hàng dưới dạng set, chuyển từ về chữ thường rồi kiểm tra tập con. Duyệt một lượt là đủ để thu thập mọi từ hợp lệ.

<!-- thinking:end -->

Đưa ba hàng phím vào các set. Với mỗi từ, nếu set chữ cái của từ là tập con của một hàng thì thêm từ đó vào kết quả.

Độ phức tạp thời gian là $O(L)$ và độ phức tạp không gian là $O(C)$, trong đó $L$ là tổng độ dài của tất cả các từ, còn $C$ là số chữ cái trong bảng chữ cái (ở đây $C = 26$).

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findWords(self, words: List[str]) -> List[str]:
        s1 = set('qwertyuiop')
        s2 = set('asdfghjkl')
        s3 = set('zxcvbnm')
        ans = []
        for w in words:
            s = set(w.lower())
            if s <= s1 or s <= s2 or s <= s3:
                ans.append(w)
        return ans
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Ánh xạ ký tự

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra bằng set vốn đã có độ phức tạp tuyến tính, nhưng mỗi từ phải tạo một set và thực hiện ba lần kiểm tra tập con.
>
> Trước tiên, ánh xạ mỗi chữ cái sang mã hàng, rồi kiểm tra mọi chữ cái có cùng hàng với chữ cái đầu tiên hay không. Bảng ánh xạ có kích thước không đổi; vẫn chỉ cần duyệt một lượt và hệ số thời gian nhỏ hơn.

<!-- thinking:end -->

Ánh xạ mỗi chữ cái sang hàng phím tương ứng, rồi kiểm tra xem mọi chữ cái trong một từ có nằm trên cùng một hàng hay không.

Độ phức tạp thời gian là $O(L)$ và độ phức tạp không gian là $O(C)$, trong đó $L$ là tổng độ dài của tất cả các từ, còn $C$ là số chữ cái trong bảng chữ cái (ở đây $C = 26$).

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findWords(self, words: List[str]) -> List[str]:
        ans = []
        s = "12210111011122000010020202"
        for w in words:
            x = s[ord(w[0].lower()) - ord('a')]
            if all(s[ord(c.lower()) - ord('a')] == x for c in w):
                ans.append(w)
        return ans
```

#### Java

```java
class Solution {
    public String[] findWords(String[] words) {
        String s = "12210111011122000010020202";
        List<String> ans = new ArrayList<>();
        for (var w : words) {
            String t = w.toLowerCase();
            char x = s.charAt(t.charAt(0) - 'a');
            boolean ok = true;
            for (char c : t.toCharArray()) {
                if (s.charAt(c - 'a') != x) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                ans.add(w);
            }
        }
        return ans.toArray(new String[0]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> findWords(vector<string>& words) {
        string s = "12210111011122000010020202";
        vector<string> ans;
        for (auto& w : words) {
            char x = s[tolower(w[0]) - 'a'];
            bool ok = true;
            for (char& c : w) {
                if (s[tolower(c) - 'a'] != x) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                ans.emplace_back(w);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findWords(words []string) (ans []string) {
	s := "12210111011122000010020202"
	for _, w := range words {
		x := s[unicode.ToLower(rune(w[0]))-'a']
		ok := true
		for _, c := range w[1:] {
			if s[unicode.ToLower(c)-'a'] != x {
				ok = false
				break
			}
		}
		if ok {
			ans = append(ans, w)
		}
	}
	return
}
```

#### TypeScript

```ts
function findWords(words: string[]): string[] {
    const s = '12210111011122000010020202';
    const ans: string[] = [];
    for (const w of words) {
        const t = w.toLowerCase();
        const x = s[t.charCodeAt(0) - 'a'.charCodeAt(0)];
        let ok = true;
        for (const c of t) {
            if (s[c.charCodeAt(0) - 'a'.charCodeAt(0)] !== x) {
                ok = false;
                break;
            }
        }
        if (ok) {
            ans.push(w);
        }
    }
    return ans;
}
```

#### C#

```cs
public class Solution {
    public string[] FindWords(string[] words) {
        string s = "12210111011122000010020202";
        IList<string> ans = new List<string>();
        foreach (string w in words) {
            char x = s[char.ToLower(w[0]) - 'a'];
            bool ok = true;
            for (int i = 1; i < w.Length; ++i) {
                if (s[char.ToLower(w[i]) - 'a'] != x) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                ans.Add(w);
            }
        }
        return ans.ToArray();
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
