---
comments: true
difficulty: Medium
rating: 1263
source: Weekly Contest 190 Q2
tags:
    - String
    - Sliding Window
---

<!-- problem:start -->

# [1456. Maximum Number of Vowels in a Substring of Given Length](https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length)

[中文文档](/solution/1400-1499/1456.Maximum%20Number%20of%20Vowels%20in%20a%20Substring%20of%20Given%20Length/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một số nguyên <code>k</code>, hãy trả về <em>số lượng chữ cái nguyên âm lớn nhất trong một chuỗi con của </em><code>s</code><em> có độ dài </em><code>k</code>.</p>

<p><strong>Các chữ cái nguyên âm</strong> trong tiếng Anh là <code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> và <code>&#39;u&#39;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abciiidef&quot;, k = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Chuỗi con &quot;iii&quot; chứa 3 chữ cái nguyên âm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aeiou&quot;, k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Mọi chuỗi con có độ dài 2 đều chứa 2 nguyên âm.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;leetcode&quot;, k = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> &quot;lee&quot;, &quot;eet&quot; và &quot;ode&quot; chứa 2 nguyên âm.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= k &lt;= s.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số nguyên âm lớn nhất trong một cửa sổ có độ dài $k$. Vì $n\le 10^5$, ta đếm $k$ ký tự đầu tiên, sau đó thêm hoặc loại bỏ một ký tự khi cửa sổ trượt.

<!-- thinking:end -->

Đầu tiên, ta đếm số lượng nguyên âm trong $k$ ký tự đầu tiên, ký hiệu là $cnt$, và khởi tạo đáp án $ans$ bằng $cnt$.

Sau đó, ta bắt đầu duyệt chuỗi từ vị trí $k$. Ở mỗi vòng lặp, ta thêm ký tự hiện tại vào cửa sổ. Nếu ký tự hiện tại là nguyên âm, ta tăng $cnt$. Ta loại bỏ ký tự đầu tiên khỏi cửa sổ. Nếu ký tự bị loại bỏ là nguyên âm, ta giảm $cnt$. Sau đó, ta cập nhật đáp án $ans = \max(ans, cnt)$.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxVowels(self, s: str, k: int) -> int:
        vowels = set("aeiou")
        ans = cnt = sum(c in vowels for c in s[:k])
        for i in range(k, len(s)):
            cnt += int(s[i] in vowels) - int(s[i - k] in vowels)
            ans = max(ans, cnt)
        return ans
```

#### Java

```java
class Solution {
    public int maxVowels(String s, int k) {
        int cnt = 0;
        for (int i = 0; i < k; ++i) {
            if (isVowel(s.charAt(i))) {
                ++cnt;
            }
        }
        int ans = cnt;
        for (int i = k; i < s.length(); ++i) {
            if (isVowel(s.charAt(i))) {
                ++cnt;
            }
            if (isVowel(s.charAt(i - k))) {
                --cnt;
            }
            ans = Math.max(ans, cnt);
        }
        return ans;
    }

    private boolean isVowel(char c) {
        return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxVowels(string s, int k) {
        auto isVowel = [](char c) {
            return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
        };
        int cnt = count_if(s.begin(), s.begin() + k, isVowel);
        int ans = cnt;
        for (int i = k; i < s.size(); ++i) {
            cnt += isVowel(s[i]) - isVowel(s[i - k]);
            ans = max(ans, cnt);
        }
        return ans;
    }
};
```

#### Go

```go
func maxVowels(s string, k int) int {
	isVowel := func(c byte) bool {
		return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u'
	}
	cnt := 0
	for i := 0; i < k; i++ {
		if isVowel(s[i]) {
			cnt++
		}
	}
	ans := cnt
	for i := k; i < len(s); i++ {
		if isVowel(s[i-k]) {
			cnt--
		}
		if isVowel(s[i]) {
			cnt++
		}
		ans = max(ans, cnt)
	}
	return ans
}
```

#### TypeScript

```ts
function maxVowels(s: string, k: number): number {
    const vowels = new Set(['a', 'e', 'i', 'o', 'u']);
    let cnt = 0;
    for (let i = 0; i < k; i++) {
        if (vowels.has(s[i])) {
            cnt++;
        }
    }
    let ans = cnt;
    for (let i = k; i < s.length; i++) {
        if (vowels.has(s[i])) {
            cnt++;
        }
        if (vowels.has(s[i - k])) {
            cnt--;
        }
        ans = Math.max(ans, cnt);
    }
    return ans;
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $s
     * @param Integer $k
     * @return Integer
     */
    function isVowel($c) {
        return $c === 'a' || $c === 'e' || $c === 'i' || $c === 'o' || $c === 'u';
    }
    function maxVowels($s, $k) {
        $cnt = 0;
        for ($i = 0; $i < $k; $i++) {
            if ($this->isVowel($s[$i])) {
                $cnt++;
            }
        }
        $ans = $cnt;
        for ($j = $k; $j < strlen($s); $j++) {
            if ($this->isVowel($s[$j - $k])) {
                $cnt--;
            }
            if ($this->isVowel($s[$j])) {
                $cnt++;
            }
            $ans = max($ans, $cnt);
        }
        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
