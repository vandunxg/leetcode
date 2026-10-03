---
comments: true
difficulty: Easy
rating: 1199
source: Weekly Contest 258 Q1
tags:
    - Stack
    - Two Pointers
    - String
---

<!-- problem:start -->

# [2000. Reverse Prefix of Word](https://leetcode.com/problems/reverse-prefix-of-word)

[中文文档](/solution/2000-2099/2000.Reverse%20Prefix%20of%20Word/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <strong>đánh chỉ số từ 0</strong> <code>word</code> và một ký tự <code>ch</code>, hãy <strong>đảo ngược</strong> đoạn của <code>word</code> bắt đầu từ chỉ số <code>0</code> và kết thúc tại chỉ số xuất hiện <strong>đầu tiên</strong> của <code>ch</code> (<strong>bao gồm</strong> chỉ số đó). Nếu ký tự <code>ch</code> không tồn tại trong <code>word</code>, không làm gì cả.</p>

<ul>
	<li>Ví dụ, nếu <code>word = &quot;abcdefd&quot;</code> và <code>ch = &quot;d&quot;</code>, ta cần <strong>đảo ngược</strong> đoạn bắt đầu từ <code>0</code> và kết thúc tại <code>3</code> (<strong>bao gồm</strong> chỉ số đó). Chuỗi kết quả sẽ là <code>&quot;<u>dcba</u>efd&quot;</code>.</li>
</ul>

<p>Trả về <em>chuỗi kết quả</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;<u>abcd</u>efd&quot;, ch = &quot;d&quot;
<strong>Đầu ra:</strong> &quot;<u>dcba</u>efd&quot;
<strong>Giải thích:</strong>&nbsp;Ký tự &quot;d&quot; xuất hiện đầu tiên tại chỉ số 3.
Đảo ngược phần của word từ 0 đến 3 (bao gồm cả 3), chuỗi kết quả là &quot;dcbaefd&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;<u>xyxz</u>xe&quot;, ch = &quot;z&quot;
<strong>Đầu ra:</strong> &quot;<u>zxyx</u>xe&quot;
<strong>Giải thích:</strong>&nbsp;Ký tự &quot;z&quot; xuất hiện duy nhất tại chỉ số 3.
Đảo ngược phần của word từ 0 đến 3 (bao gồm cả 3), chuỗi kết quả là &quot;zxyxxe&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;abcd&quot;, ch = &quot;z&quot;
<strong>Đầu ra:</strong> &quot;abcd&quot;
<strong>Giải thích:</strong>&nbsp;&quot;z&quot; không tồn tại trong word.
Không thực hiện thao tác đảo ngược nào, chuỗi kết quả là &quot;abcd&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 250</code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>ch</code> là một chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Độ dài chuỗi thỏa mãn $n \le 250$, nên việc tìm $ch$ và đảo ngược tiền tố đều có thể thực hiện trong thời gian tuyến tính. Nếu không tìm thấy $ch$, chuỗi ban đầu đã là đáp án.
>
> Chỉ đoạn đến lần xuất hiện đầu tiên mới được đảo ngược; phần hậu tố giữ nguyên. Một lần gọi `find` sẽ cho chỉ số $i$, hoặc $-1$ nếu không tìm thấy ký tự.
>
> Vì vậy, ta lấy lát cắt $word[i::-1]$ cho tiền tố đã đảo ngược rồi nối với $word[i+1:]$.

<!-- thinking:end -->

Đầu tiên, ta tìm chỉ số $i$ nơi ký tự $ch$ xuất hiện lần đầu tiên. Sau đó, ta đảo ngược các ký tự từ chỉ số $0$ đến chỉ số $i$ (bao gồm cả $i$). Cuối cùng, ta nối chuỗi đã đảo ngược với chuỗi bắt đầu từ chỉ số $i + 1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi $word$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reversePrefix(self, word: str, ch: str) -> str:
        i = word.find(ch)
        return word if i == -1 else word[i::-1] + word[i + 1 :]
```

#### Java

```java
class Solution {
    public String reversePrefix(String word, char ch) {
        int j = word.indexOf(ch);
        if (j == -1) {
            return word;
        }
        char[] cs = word.toCharArray();
        for (int i = 0; i < j; ++i, --j) {
            char t = cs[i];
            cs[i] = cs[j];
            cs[j] = t;
        }
        return String.valueOf(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string reversePrefix(string word, char ch) {
        int i = word.find(ch);
        if (i != string::npos) {
            reverse(word.begin(), word.begin() + i + 1);
        }
        return word;
    }
};
```

#### Go

```go
func reversePrefix(word string, ch byte) string {
	j := strings.IndexByte(word, ch)
	if j < 0 {
		return word
	}
	s := []byte(word)
	for i := 0; i < j; i++ {
		s[i], s[j] = s[j], s[i]
		j--
	}
	return string(s)
}
```

#### TypeScript

```ts
function reversePrefix(word: string, ch: string): string {
    const i = word.indexOf(ch) + 1;
    if (!i) {
        return word;
    }
    return [...word.slice(0, i)].reverse().join('') + word.slice(i);
}
```

#### Rust

```rust
impl Solution {
    pub fn reverse_prefix(word: String, ch: char) -> String {
        match word.find(ch) {
            Some(i) => word[..=i].chars().rev().collect::<String>() + &word[i + 1..],
            None => word,
        }
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $word
     * @param String $ch
     * @return String
     */
    function reversePrefix($word, $ch) {
        $len = strlen($word);
        $rs = '';
        for ($i = 0; $i < $len; $i++) {
            $rs = $rs . $word[$i];
            if ($word[$i] == $ch) {
                break;
            }
        }
        if (strlen($rs) == $len && $rs[$len - 1] != $ch) {
            return $word;
        }
        $rs = strrev($rs);
        $rs = $rs . substr($word, strlen($rs));
        return $rs;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
