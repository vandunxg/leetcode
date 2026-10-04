---
comments: true
difficulty: Medium
rating: 1761
source: Weekly Contest 430 Q2
tags:
    - Two Pointers
    - String
    - Enumeration
---

<!-- problem:start -->

# [3403. Find the Lexicographically Largest String From the Box I](https://leetcode.com/problems/find-the-lexicographically-largest-string-from-the-box-i)

[中文文档](/solution/3400-3499/3403.Find%20the%20Lexicographically%20Largest%20String%20From%20the%20Box%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>word</code> và một số nguyên <code>numFriends</code>.</p>

<p>Alice đang tổ chức một trò chơi cho <code>numFriends</code> người bạn của mình. Trò chơi có nhiều vòng, trong mỗi vòng:</p>

<ul>
    <li><code>word</code> được tách thành <code>numFriends</code> chuỗi <strong>không rỗng</strong>, sao cho không có vòng nào trước đó có cùng cách tách <strong>chính xác</strong>.</li>
    <li>Tất cả các chuỗi sau khi tách được cho vào một chiếc hộp.</li>
</ul>

<p>Hãy tìm chuỗi <span data-keyword="lexicographically-smaller-string">lớn nhất theo thứ tự từ điển</span> trong hộp sau khi hoàn tất tất cả các vòng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;dbca&quot;, numFriends = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;dbc&quot;</span></p>

<p><strong>Giải thích:</strong>&nbsp;</p>

<p>Mọi cách tách có thể là:</p>

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

<p><strong>Giải thích:</strong>&nbsp;</p>

<p>Cách tách duy nhất có thể là: <code>&quot;g&quot;</code>, <code>&quot;g&quot;</code>, <code>&quot;g&quot;</code> và <code>&quot;g&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= word.length &lt;= 5&nbsp;* 10<sup>3</sup></code></li>
    <li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
    <li><code>1 &lt;= numFriends &lt;= word.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê điểm bắt đầu của chuỗi con

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta chia $word$ thành $\textit{numFriends}$ mảnh không rỗng và giữ lại mảnh lớn nhất theo thứ tự từ điển. Khi $\textit{numFriends}=1$, toàn bộ chuỗi là mảnh duy nhất.
>
> $n$ không lớn, nhưng việc liệt kê mọi cách phân hoạch sẽ khiến cùng một chuỗi con bị so sánh nhiều lần. Với một điểm bắt đầu cố định, chuỗi dài hơn không bao giờ nhỏ hơn theo thứ tự từ điển.
>
> Các người bạn còn lại phải nhận ít nhất một ký tự, nên mảnh bắt đầu tại $i$ có độ dài tối đa là $n-(\textit{numFriends}-1)$. Chúng ta liệt kê các điểm bắt đầu, lấy đoạn tương ứng và trả về giá trị lớn nhất trong số đó.

<!-- thinking:end -->

Nếu cố định điểm bắt đầu của chuỗi con, chuỗi con càng dài thì thứ tự từ điển càng lớn. Giả sử điểm bắt đầu của chuỗi con là $i$, và độ dài tối thiểu của các chuỗi con còn lại là $\text{numFriends} - 1$, thì điểm kết thúc bên phải của chuỗi con có thể lên tới $\min(n, i + n - (\text{numFriends} - 1))$, trong đó $n$ là độ dài của chuỗi. Lưu ý rằng các khoảng đang xét là đóng bên trái, mở bên phải.

Chúng ta liệt kê tất cả các điểm bắt đầu có thể có, trích xuất chuỗi con tương ứng, so sánh thứ tự từ điển của chúng, rồi tìm chuỗi con lớn nhất theo thứ tự từ điển.

Độ phức tạp thời gian là $O(n^2)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def answerString(self, word: str, numFriends: int) -> str:
        if numFriends == 1:
            return word
        n = len(word)
        return max(word[i : i + n - (numFriends - 1)] for i in range(n))
```

#### Java

```java
class Solution {
    public String answerString(String word, int numFriends) {
        if (numFriends == 1) {
            return word;
        }
        int n = word.length();
        String ans = "";
        for (int i = 0; i < n; ++i) {
            String t = word.substring(i, Math.min(n, i + n - (numFriends - 1)));
            if (ans.compareTo(t) < 0) {
                ans = t;
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
    string answerString(string word, int numFriends) {
        if (numFriends == 1) {
            return word;
        }
        int n = word.length();
        string ans = "";
        for (int i = 0; i < n; ++i) {
            string t = word.substr(i, min(n - i, n - (numFriends - 1)));
            if (ans < t) {
                ans = t;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func answerString(word string, numFriends int) (ans string) {
    if numFriends == 1 {
        return word
    }
    n := len(word)
    for i := 0; i < n; i++ {
        t := word[i:min(n, i+n-(numFriends-1))]
        ans = max(ans, t)
    }
    return
}
```

#### TypeScript

```ts
function answerString(word: string, numFriends: number): string {
    if (numFriends === 1) {
        return word;
    }
    const n = word.length;
    let ans = '';
    for (let i = 0; i < n; i++) {
        const t = word.slice(i, Math.min(n, i + n - (numFriends - 1)));
        ans = t > ans ? t : ans;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
