---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [1554. Strings Differ by One Character 🔒](https://leetcode.com/problems/strings-differ-by-one-character)

[中文文档](/solution/1500-1599/1554.Strings%20Differ%20by%20One%20Character/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách chuỗi <code>dict</code>, trong đó mọi chuỗi có cùng độ dài.</p>

<p>Trả về <code>true</code> nếu có hai chuỗi chỉ khác nhau một ký tự tại cùng một chỉ số, nếu không thì trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> dict = [&quot;abcd&quot;,&quot;acbd&quot;, &quot;aacd&quot;]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Hai chuỗi &quot;a<strong>b</strong>cd&quot; và &quot;a<strong>a</strong>cd&quot; chỉ khác nhau một ký tự tại chỉ số 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> dict = [&quot;ab&quot;,&quot;cd&quot;,&quot;yz&quot;]
<strong>Đầu ra:</strong> false
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> dict = [&quot;abcd&quot;,&quot;cccc&quot;,&quot;abyd&quot;,&quot;abab&quot;]
<strong>Đầu ra:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số ký tự trong <code>dict &lt;= 10<sup>5</sup></code></li>
	<li><code>dict[i].length == dict[j].length</code></li>
	<li><code>dict[i]</code> là duy nhất.</li>
	<li><code>dict[i]</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xác định xem hai từ có khác nhau đúng một vị trí hay không. Tổng số ký tự nhiều nhất là $10^5$, nên việc so sánh từng cặp sẽ có độ phức tạp bậc hai.
>
> Thay ký tự tại từng chỉ số của một từ bằng wildcard để tạo mẫu bỏ qua vị trí đó. Nếu mẫu đã có trong một set, một từ khác chỉ khác từ hiện tại tại vị trí ấy. Hash set giúp tra cứu với thời gian kỳ vọng hằng số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def differByOne(self, dict: List[str]) -> bool:
        s = set()
        for word in dict:
            for i in range(len(word)):
                t = word[:i] + "*" + word[i + 1 :]
                if t in s:
                    return True
                s.add(t)
        return False
```

#### Java

```java
class Solution {
    public boolean differByOne(String[] dict) {
        Set<String> s = new HashSet<>();
        for (String word : dict) {
            for (int i = 0; i < word.length(); ++i) {
                String t = word.substring(0, i) + "*" + word.substring(i + 1);
                if (s.contains(t)) {
                    return true;
                }
                s.add(t);
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool differByOne(vector<string>& dict) {
        unordered_set<string> s;
        for (auto word : dict) {
            for (int i = 0; i < word.size(); ++i) {
                auto t = word;
                t[i] = '*';
                if (s.count(t)) return true;
                s.insert(t);
            }
        }
        return false;
    }
};
```

#### Go

```go
func differByOne(dict []string) bool {
	s := make(map[string]bool)
	for _, word := range dict {
		for i := range word {
			t := word[:i] + "*" + word[i+1:]
			if s[t] {
				return true
			}
			s[t] = true
		}
	}
	return false
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
