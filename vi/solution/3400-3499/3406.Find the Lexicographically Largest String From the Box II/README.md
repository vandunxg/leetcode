---
comments: true
difficulty: Hard
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [3406. Find the Lexicographically Largest String From the Box II 🔒](https://leetcode.com/problems/find-the-lexicographically-largest-string-from-the-box-ii)

[中文文档](/solution/3400-3499/3406.Find%20the%20Lexicographically%20Largest%20String%20From%20the%20Box%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>word</code> và một số nguyên <code>numFriends</code>.</p>

<p>Alice đang tổ chức một trò chơi cho <code>numFriends</code> người bạn. Trò chơi gồm nhiều vòng, trong mỗi vòng:</p>

<ul>
    <li><code>word</code> được tách thành <code>numFriends</code> chuỗi <strong>không rỗng</strong>, sao cho chưa có vòng nào trước đó có cách tách <strong>hoàn toàn</strong> giống vậy.</li>
    <li>Tất cả các chuỗi sau khi tách được cho vào một chiếc hộp.</li>
</ul>

<p>Hãy tìm chuỗi <strong>lớn nhất theo thứ tự từ điển</strong> trong hộp sau khi tất cả các vòng kết thúc.</p>

<p>Một chuỗi <code>a</code> được gọi là <strong>nhỏ hơn theo thứ tự từ điển</strong> so với chuỗi <code>b</code> nếu tại vị trí đầu tiên mà <code>a</code> và <code>b</code> khác nhau, chuỗi <code>a</code> có ký tự xuất hiện sớm hơn trong bảng chữ cái so với ký tự tương ứng trong <code>b</code>.<br />
Nếu <code>min(a.length, b.length)</code> ký tự đầu tiên không khác nhau, thì chuỗi ngắn hơn sẽ nhỏ hơn theo thứ tự từ điển.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;dbca&quot;, numFriends = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;dbc&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả các cách tách có thể là:</p>

<ul>
    <li><code>&quot;d&quot;</code> và <code>&quot;bca&quot;</code>.</li>
    <li><code>&quot;db&quot;</code> và <code>&quot;ca&quot;</code>.</li>
    <li><code>&quot;dbc&quot;</code> và <code>&quot;a&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;gggg&quot;, numFriends = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;g&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cách tách duy nhất là: <code>&quot;g&quot;</code>, <code>&quot;g&quot;</code>, <code>&quot;g&quot;</code> và <code>&quot;g&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= word.length &lt;= 2 * 10<sup>5</sup></code></li>
    <li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
    <li><code>1 &lt;= numFriends &lt;= word.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tương tự Box I, mảnh lớn nhất theo thứ tự từ điển là tiền tố của một hậu tố nào đó, có độ dài không quá $n-\textit{numFriends}+1$. So sánh từng cặp vị trí bắt đầu vẫn dựa trên cùng ý tưởng nhưng có hằng số lớn hơn.
>
> Khi đã biết hậu tố lớn nhất theo thứ tự từ điển của toàn bộ chuỗi, đáp án là tiền tố của hậu tố đó với độ dài cho phép.
>
> Vì vậy, chúng ta tính $\textit{lastSubstring}$ bằng hai con trỏ: vị trí bắt đầu tốt nhất hiện tại $i$ và một vị trí đang được so sánh $j$, bỏ qua phần tiền tố chung rồi loại bỏ phía yếu hơn khi gặp ký tự khác nhau. Nếu $\textit{numFriends}=1$, chúng ta vẫn trả về word ban đầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def answerString(self, word: str, numFriends: int) -> str:
        if numFriends == 1:
            return word
        s = self.lastSubstring(word)
        return s[: len(word) - numFriends + 1]

    def lastSubstring(self, s: str) -> str:
        i, j, k = 0, 1, 0
        while j + k < len(s):
            if s[i + k] == s[j + k]:
                k += 1
            elif s[i + k] < s[j + k]:
                i += k + 1
                k = 0
                if i >= j:
                    j = i + 1
            else:
                j += k + 1
                k = 0
        return s[i:]
```

#### Java

```java
class Solution {
    public String answerString(String word, int numFriends) {
        if (numFriends == 1) {
            return word;
        }
        String s = lastSubstring(word);
        return s.substring(0, Math.min(s.length(), word.length() - numFriends + 1));
    }

    public String lastSubstring(String s) {
        int n = s.length();
        int i = 0, j = 1, k = 0;
        while (j + k < n) {
            int d = s.charAt(i + k) - s.charAt(j + k);
            if (d == 0) {
                ++k;
            } else if (d < 0) {
                i += k + 1;
                k = 0;
                if (i >= j) {
                    j = i + 1;
                }
            } else {
                j += k + 1;
                k = 0;
            }
        }
        return s.substring(i);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string answerString(string word, int numFriends) {
        if (numFriends == 1) {
            return word;
        }
        string s = lastSubstring(word);
        return s.substr(0, min(s.length(), word.length() - numFriends + 1));
    }

    string lastSubstring(string& s) {
        int n = s.size();
        int i = 0, j = 1, k = 0;
        while (j + k < n) {
            if (s[i + k] == s[j + k]) {
                ++k;
            } else if (s[i + k] < s[j + k]) {
                i += k + 1;
                k = 0;
                if (i >= j) {
                    j = i + 1;
                }
            } else {
                j += k + 1;
                k = 0;
            }
        }
        return s.substr(i);
    }
};
```

#### Go

```go
func answerString(word string, numFriends int) string {
    if numFriends == 1 {
        return word
    }
    s := lastSubstring(word)
    return s[:min(len(s), len(word)-numFriends+1)]
}

func lastSubstring(s string) string {
    n := len(s)
    i, j, k := 0, 1, 0
    for j+k < n {
        if s[i+k] == s[j+k] {
            k++
        } else if s[i+k] < s[j+k] {
            i += k + 1
            k = 0
            if i >= j {
                j = i + 1
            }
        } else {
            j += k + 1
            k = 0
        }
    }
    return s[i:]
}
```

#### TypeScript

```ts
function answerString(word: string, numFriends: number): string {
    if (numFriends === 1) {
        return word;
    }
    const s = lastSubstring(word);
    return s.slice(0, word.length - numFriends + 1);
}

function lastSubstring(s: string): string {
    const n = s.length;
    let i = 0;
    for (let j = 1, k = 0; j + k < n;) {
        if (s[i + k] === s[j + k]) {
            ++k;
        } else if (s[i + k] < s[j + k]) {
            i += k + 1;
            k = 0;
            if (i >= j) {
                j = i + 1;
            }
        } else {
            j += k + 1;
            k = 0;
        }
    }
    return s.slice(i);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
