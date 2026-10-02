---
comments: true
difficulty: Easy
rating: 1207
source: Weekly Contest 221 Q1
tags:
    - String
    - Counting
---

<!-- problem:start -->

# [1704. Determine if String Halves Are Alike](https://leetcode.com/problems/determine-if-string-halves-are-alike)

[中文文档](/solution/1700-1799/1704.Determine%20if%20String%20Halves%20Are%20Alike/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> có độ dài chẵn. Chia chuỗi thành hai nửa có độ dài bằng nhau, gọi nửa đầu là <code>a</code> và nửa sau là <code>b</code>.</p>

<p>Hai chuỗi <strong>tương đồng</strong> nếu có cùng số nguyên âm (<code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code>, <code>&#39;u&#39;</code>, <code>&#39;A&#39;</code>, <code>&#39;E&#39;</code>, <code>&#39;I&#39;</code>, <code>&#39;O&#39;</code>, <code>&#39;U&#39;</code>). Lưu ý rằng <code>s</code> chứa cả chữ hoa và chữ thường.</p>

<p>Trả về <code>true</code><em> nếu </em><code>a</code><em> và </em><code>b</code><em> <strong>tương đồng</strong></em>. Ngược lại, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;book&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> a = &quot;b<u>o</u>&quot; và b = &quot;<u>o</u>k&quot;. a có 1 nguyên âm và b cũng có 1 nguyên âm. Vì vậy, chúng tương đồng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;textbook&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> a = &quot;t<u>e</u>xt&quot; và b = &quot;b<u>oo</u>k&quot;. a có 1 nguyên âm còn b có 2. Vì vậy, chúng không tương đồng.
Lưu ý rằng nguyên âm o được đếm hai lần.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s.length</code> là số chẵn.</li>
	<li><code>s</code> gồm các chữ cái <strong>hoa và thường</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ cần biết hai nửa có cùng số nguyên âm hay không. Độ dài tối đa là $1000$, nên chỉ cần duyệt một lần.
>
> Lưu các nguyên âm ở cả hai dạng vào một set và duyệt đồng thời hai nửa: tăng bộ đếm khi gặp nguyên âm ở nửa trái, giảm khi gặp nguyên âm ở nửa phải. Hai nửa tương đồng khi và chỉ khi bộ đếm cuối cùng bằng không.

<!-- thinking:end -->

Duyệt chuỗi. Nếu số nguyên âm trong nửa đầu bằng số nguyên âm trong nửa sau, trả về `true`; ngược lại, trả về `false`.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(C)$, với $C$ là số ký tự nguyên âm.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def halvesAreAlike(self, s: str) -> bool:
        cnt, n = 0, len(s) >> 1
        vowels = set('aeiouAEIOU')
        for i in range(n):
            cnt += s[i] in vowels
            cnt -= s[i + n] in vowels
        return cnt == 0
```

#### Java

```java
class Solution {
    public boolean halvesAreAlike(String s) {
        Set<Character> vowels = Set.of('a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U');
        int n = s.length() >> 1;
        int cnt = 0;
        for (int i = 0; i < n; ++i) {
            cnt += vowels.contains(s.charAt(i)) ? 1 : 0;
            cnt -= vowels.contains(s.charAt(i + n)) ? 1 : 0;
        }
        return cnt == 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool halvesAreAlike(string s) {
        unordered_set<char> vowels = {'a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'};
        int cnt = 0, n = s.size() / 2;
        for (int i = 0; i < n; ++i) {
            cnt += vowels.count(s[i]);
            cnt -= vowels.count(s[i + n]);
        }
        return cnt == 0;
    }
};
```

#### Go

```go
func halvesAreAlike(s string) bool {
	vowels := map[byte]bool{}
	for _, c := range "aeiouAEIOU" {
		vowels[byte(c)] = true
	}
	cnt, n := 0, len(s)>>1
	for i := 0; i < n; i++ {
		if vowels[s[i]] {
			cnt++
		}
		if vowels[s[i+n]] {
			cnt--
		}
	}
	return cnt == 0
}
```

#### TypeScript

```ts
function halvesAreAlike(s: string): boolean {
    const vowels = new Set('aeiouAEIOU'.split(''));
    let cnt = 0;
    const n = s.length >> 1;
    for (let i = 0; i < n; ++i) {
        cnt += vowels.has(s[i]) ? 1 : 0;
        cnt -= vowels.has(s[n + i]) ? 1 : 0;
    }
    return cnt === 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn halves_are_alike(s: String) -> bool {
        let n = s.len() / 2;
        let vowels: std::collections::HashSet<char> = "aeiouAEIOU".chars().collect();
        let mut cnt = 0;

        for i in 0..n {
            if vowels.contains(&s.chars().nth(i).unwrap()) {
                cnt += 1;
            }
            if vowels.contains(&s.chars().nth(i + n).unwrap()) {
                cnt -= 1;
            }
        }

        cnt == 0
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var halvesAreAlike = function (s) {
    const vowels = new Set('aeiouAEIOU'.split(''));
    let cnt = 0;
    const n = s.length >> 1;
    for (let i = 0; i < n; ++i) {
        cnt += vowels.has(s[i]);
        cnt -= vowels.has(s[n + i]);
    }
    return cnt === 0;
};
```

#### PHP

```php
class Solution {
    /**
     * @param String $s
     * @return Boolean
     */
    function halvesAreAlike($s) {
        $n = strlen($s) / 2;
        $vowels = ['a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'];
        $cnt = 0;

        for ($i = 0; $i < $n; $i++) {
            if (in_array($s[$i], $vowels)) {
                $cnt++;
            }
            if (in_array($s[$i + $n], $vowels)) {
                $cnt--;
            }
        }

        return $cnt == 0;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
