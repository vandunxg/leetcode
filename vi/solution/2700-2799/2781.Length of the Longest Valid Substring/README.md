---
comments: true
difficulty: Hard
rating: 2203
source: Weekly Contest 354 Q4
tags:
    - Array
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [2781. Length of the Longest Valid Substring](https://leetcode.com/problems/length-of-the-longest-valid-substring)

[中文文档](/solution/2700-2799/2781.Length%20of%20the%20Longest%20Valid%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>word</code> và một mảng các chuỗi <code>forbidden</code>.</p>

<p>Một chuỗi được gọi là <strong>hợp lệ</strong> nếu không có substring nào của nó xuất hiện trong <code>forbidden</code>.</p>

<p>Trả về <em>độ dài của <strong>substring hợp lệ dài nhất</strong> của chuỗi </em><code>word</code>.</p>

<p><strong>Substring</strong> là một dãy ký tự liên tiếp trong một chuỗi, có thể rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;cbaaaabc&quot;, forbidden = [&quot;aaa&quot;,&quot;cb&quot;]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có 11 substring hợp lệ trong word: &quot;c&quot;, &quot;b&quot;, &quot;a&quot;, &quot;ba&quot;, &quot;aa&quot;, &quot;bc&quot;, &quot;baa&quot;, &quot;aab&quot;, &quot;ab&quot;, &quot;abc&quot; và &quot;aabc&quot;. Độ dài của substring hợp lệ dài nhất là 4.
Có thể chứng minh rằng mọi substring còn lại đều chứa &quot;aaa&quot; hoặc &quot;cb&quot; dưới dạng substring. </pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;leetcode&quot;, forbidden = [&quot;de&quot;,&quot;le&quot;,&quot;e&quot;]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có 11 substring hợp lệ trong word: &quot;l&quot;, &quot;t&quot;, &quot;c&quot;, &quot;o&quot;, &quot;d&quot;, &quot;tc&quot;, &quot;co&quot;, &quot;od&quot;, &quot;tco&quot;, &quot;cod&quot; và &quot;tcod&quot;. Độ dài của substring hợp lệ dài nhất là 4.
Có thể chứng minh rằng mọi substring còn lại đều chứa &quot;de&quot;, &quot;le&quot; hoặc &quot;e&quot; dưới dạng substring.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 10<sup>5</sup></code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= forbidden.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= forbidden[i].length &lt;= 10</code></li>
	<li><code>forbidden[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một substring hợp lệ không chứa chuỗi bị cấm nào dưới dạng một đoạn liên tiếp; ta cần tìm độ dài lớn nhất. Các chuỗi bị cấm có độ dài không quá $10$, nhưng $word$ có thể dài tới $10^5$, nên không thể kiểm tra mọi substring.
>
> Khi đầu phải $j$ tăng dần, ta chỉ cần tra cứu các hậu tố kết thúc tại $j$, có độ dài không quá $10$ và vẫn bắt đầu ở bên phải đầu trái hiện tại. Nếu tìm thấy, ta đưa đầu trái đến ngay sau đoạn bị cấm đó. Cửa sổ dài nhất chính là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestValidSubstring(self, word: str, forbidden: List[str]) -> int:
        s = set(forbidden)
        ans = i = 0
        for j in range(len(word)):
            for k in range(j, max(j - 10, i - 1), -1):
                if word[k : j + 1] in s:
                    i = k + 1
                    break
            ans = max(ans, j - i + 1)
        return ans
```

#### Java

```java
class Solution {
    public int longestValidSubstring(String word, List<String> forbidden) {
        var s = new HashSet<>(forbidden);
        int ans = 0, n = word.length();
        for (int i = 0, j = 0; j < n; ++j) {
            for (int k = j; k > Math.max(j - 10, i - 1); --k) {
                if (s.contains(word.substring(k, j + 1))) {
                    i = k + 1;
                    break;
                }
            }
            ans = Math.max(ans, j - i + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestValidSubstring(string word, vector<string>& forbidden) {
        unordered_set<string> s(forbidden.begin(), forbidden.end());
        int ans = 0, n = word.size();
        for (int i = 0, j = 0; j < n; ++j) {
            for (int k = j; k > max(j - 10, i - 1); --k) {
                if (s.count(word.substr(k, j - k + 1))) {
                    i = k + 1;
                    break;
                }
            }
            ans = max(ans, j - i + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func longestValidSubstring(word string, forbidden []string) (ans int) {
	s := map[string]bool{}
	for _, x := range forbidden {
		s[x] = true
	}
	n := len(word)
	for i, j := 0, 0; j < n; j++ {
		for k := j; k > max(j-10, i-1); k-- {
			if s[word[k:j+1]] {
				i = k + 1
				break
			}
		}
		ans = max(ans, j-i+1)
	}
	return
}
```

#### TypeScript

```ts
function longestValidSubstring(word: string, forbidden: string[]): number {
    const s: Set<string> = new Set(forbidden);
    const n = word.length;
    let ans = 0;
    for (let i = 0, j = 0; j < n; ++j) {
        for (let k = j; k > Math.max(j - 10, i - 1); --k) {
            if (s.has(word.substring(k, j + 1))) {
                i = k + 1;
                break;
            }
        }
        ans = Math.max(ans, j - i + 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
