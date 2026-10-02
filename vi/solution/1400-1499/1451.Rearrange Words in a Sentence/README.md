---
comments: true
difficulty: Medium
rating: 1309
source: Weekly Contest 189 Q2
tags:
    - String
    - Sorting
---

<!-- problem:start -->

# [1451. Rearrange Words in a Sentence](https://leetcode.com/problems/rearrange-words-in-a-sentence)

[中文文档](/solution/1400-1499/1451.Rearrange%20Words%20in%20a%20Sentence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một câu <code>text</code> (một <em>câu</em> là một chuỗi gồm các từ được phân tách bằng dấu cách) có định dạng như sau:</p>

<ul>
	<li>Ký tự đầu tiên là chữ hoa.</li>
	<li>Mỗi từ trong <code>text</code> được phân tách bằng một dấu cách.</li>
</ul>

<p>Nhiệm vụ của bạn là sắp xếp lại các từ trong text sao cho&nbsp;tất cả các từ được sắp xếp theo thứ tự tăng dần của độ dài. Nếu hai từ có cùng độ dài, hãy giữ chúng theo thứ tự ban đầu.</p>

<p>Trả về text mới&nbsp;theo định dạng như trên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;Leetcode is cool&quot;
<strong>Đầu ra:</strong> &quot;Is cool leetcode&quot;
<strong>Giải thích: </strong>Có 3 từ, &quot;Leetcode&quot; có độ dài 8, &quot;is&quot; có độ dài 2 và &quot;cool&quot; có độ dài 4.
Đầu ra được sắp xếp theo độ dài và từ đầu tiên mới bắt đầu bằng chữ hoa.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;Keep calm and code on&quot;
<strong>Đầu ra:</strong> &quot;On and keep calm code&quot;
<strong>Giải thích: </strong>Đầu ra được sắp xếp như sau:
&quot;On&quot; 2 chữ cái.
&quot;and&quot; 3 chữ cái.
&quot;keep&quot; 4 chữ cái, khi bằng nhau thì sắp xếp theo vị trí trong text ban đầu.
&quot;calm&quot; 4 chữ cái.
&quot;code&quot; 4 chữ cái.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;To be or not to be&quot;
<strong>Đầu ra:</strong> &quot;To be or to be not&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>text</code> bắt đầu bằng một chữ hoa, sau đó là các chữ thường và một dấu cách giữa các từ.</li>
	<li><code>1 &lt;= text.length &lt;= 10^5</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp các từ theo độ dài, giữ nguyên thứ tự ban đầu của các từ có cùng độ dài, rồi viết hoa lại câu. Tách chuỗi, chuyển từ đầu tiên thành chữ thường, stable-sort theo `len`, sau đó viết hoa chữ cái đầu của từ đầu tiên mới.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def arrangeWords(self, text: str) -> str:
        words = text.split()
        words[0] = words[0].lower()
        words.sort(key=len)
        words[0] = words[0].title()
        return " ".join(words)
```

#### Java

```java
class Solution {
    public String arrangeWords(String text) {
        String[] words = text.split(" ");
        words[0] = words[0].toLowerCase();
        Arrays.sort(words, Comparator.comparingInt(String::length));
        words[0] = words[0].substring(0, 1).toUpperCase() + words[0].substring(1);
        return String.join(" ", words);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string arrangeWords(string text) {
        vector<string> words;
        stringstream ss(text);
        string t;
        while (ss >> t) {
            words.push_back(t);
        }
        words[0][0] = tolower(words[0][0]);
        stable_sort(words.begin(), words.end(), [](const string& a, const string& b) {
            return a.size() < b.size();
        });
        string ans = "";
        for (auto& s : words) {
            ans += s + " ";
        }
        ans.pop_back();
        ans[0] = toupper(ans[0]);
        return ans;
    }
};
```

#### Go

```go
func arrangeWords(text string) string {
	words := strings.Split(text, " ")
	words[0] = strings.ToLower(words[0])
	sort.SliceStable(words, func(i, j int) bool { return len(words[i]) < len(words[j]) })
	words[0] = strings.Title(words[0])
	return strings.Join(words, " ")
}
```

#### TypeScript

```ts
function arrangeWords(text: string): string {
    let words: string[] = text.split(' ');
    words[0] = words[0].toLowerCase();
    words.sort((a, b) => a.length - b.length);
    words[0] = words[0].charAt(0).toUpperCase() + words[0].slice(1);
    return words.join(' ');
}
```

#### JavaScript

```js
/**
 * @param {string} text
 * @return {string}
 */
var arrangeWords = function (text) {
    let arr = text.split(' ');
    arr[0] = arr[0].toLocaleLowerCase();
    arr.sort((a, b) => a.length - b.length);
    arr[0] = arr[0][0].toLocaleUpperCase() + arr[0].substr(1);
    return arr.join(' ');
};
```

#### PHP

```php
class Solution {
    /**
     * @param String $text
     * @return String
     */
    function arrangeWords($text) {
        $text = lcfirst($text);
        $arr = explode(' ', $text);
        for ($i = 0; $i < count($arr); $i++) {
            $hashtable[$i] = strlen($arr[$i]);
        }
        asort($hashtable);
        $key = array_keys($hashtable);
        $rs = [];
        for ($j = 0; $j < count($key); $j++) {
            array_push($rs, $arr[$key[$j]]);
        }
        return ucfirst(implode(' ', $rs));
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
