---
comments: true
difficulty: Easy
rating: 1205
source: Weekly Contest 150 Q1
tags:
    - Array
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [1160. Find Words That Can Be Formed by Characters](https://leetcode.com/problems/find-words-that-can-be-formed-by-characters)

[中文文档](/solution/1100-1199/1160.Find%20Words%20That%20Can%20Be%20Formed%20by%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>words</code> và chuỗi <code>chars</code>.</p>

<p>Một chuỗi được gọi là <strong>hợp lệ</strong> nếu có thể tạo thành từ các ký tự trong <code>chars</code> (mỗi ký tự chỉ được dùng một lần cho <strong>từng</strong> từ trong <code>words</code>).</p>

<p>Trả về <em>tổng độ dài của tất cả chuỗi hợp lệ trong words</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;cat&quot;,&quot;bt&quot;,&quot;hat&quot;,&quot;tree&quot;], chars = &quot;atach&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Có thể tạo thành các chuỗi &quot;cat&quot; và &quot;hat&quot;, nên đáp án là 3 + 3 = 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;hello&quot;,&quot;world&quot;,&quot;leetcode&quot;], chars = &quot;welldonehoneyr&quot;
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Có thể tạo thành các chuỗi &quot;hello&quot; và &quot;world&quot;, nên đáp án là 5 + 5 = 10.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 1000</code></li>
	<li><code>1 &lt;= words[i].length, chars.length &lt;= 100</code></li>
	<li><code>words[i]</code> và <code>chars</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm tần suất

<!-- thinking:start -->

> **Tư duy**
>
> Một từ tạo được khi số lần xuất hiện của mỗi chữ cái không vượt quá số lượng trong $chars$. Đếm lại $chars$ cho từng từ sẽ lặp công việc. Hãy đếm $chars$ một lần, rồi so sánh tần suất của từng từ: nếu một chữ cái vượt quá số lượng cho phép thì bỏ qua từ đó, nếu không thì cộng độ dài của từ vào đáp án. Bảng chữ cái có kích thước cố định, nên mỗi lần so sánh có độ phức tạp tuyến tính theo độ dài từ.

<!-- thinking:end -->

Ta có thể dùng mảng $cnt$ độ dài $26$ để đếm số lần xuất hiện của từng chữ cái trong chuỗi $chars$.

Sau đó, ta duyệt mảng chuỗi $words$. Với mỗi chuỗi $w$, dùng mảng $wc$ độ dài $26$ để đếm số lần xuất hiện của từng chữ cái trong $w$. Nếu với mọi chữ cái $c$, $wc[c] \leq cnt[c]$, ta có thể tạo chuỗi $w$ từ các chữ cái trong $chars$; nếu không thì không thể. Nếu tạo được $w$, ta cộng độ dài của chuỗi này vào đáp án.

Sau khi duyệt xong, ta thu được đáp án.

Độ phức tạp thời gian là $O(L)$, độ phức tạp không gian là $O(C)$. Trong đó, $L$ là tổng độ dài của tất cả các chuỗi trong bài toán, còn $C$ là kích thước của tập ký tự. Ở bài này, $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countCharacters(self, words: List[str], chars: str) -> int:
        cnt = Counter(chars)
        ans = 0
        for w in words:
            wc = Counter(w)
            if all(cnt[c] >= v for c, v in wc.items()):
                ans += len(w)
        return ans
```

#### Java

```java
class Solution {
    public int countCharacters(String[] words, String chars) {
        int[] cnt = new int[26];
        for (int i = 0; i < chars.length(); ++i) {
            ++cnt[chars.charAt(i) - 'a'];
        }
        int ans = 0;
        for (String w : words) {
            int[] wc = new int[26];
            boolean ok = true;
            for (int i = 0; i < w.length(); ++i) {
                int j = w.charAt(i) - 'a';
                if (++wc[j] > cnt[j]) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                ans += w.length();
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
    int countCharacters(vector<string>& words, string chars) {
        int cnt[26]{};
        for (char& c : chars) {
            ++cnt[c - 'a'];
        }
        int ans = 0;
        for (auto& w : words) {
            int wc[26]{};
            bool ok = true;
            for (auto& c : w) {
                int i = c - 'a';
                if (++wc[i] > cnt[i]) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                ans += w.size();
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countCharacters(words []string, chars string) (ans int) {
	cnt := [26]int{}
	for _, c := range chars {
		cnt[c-'a']++
	}
	for _, w := range words {
		wc := [26]int{}
		ok := true
		for _, c := range w {
			c -= 'a'
			wc[c]++
			if wc[c] > cnt[c] {
				ok = false
				break
			}
		}
		if ok {
			ans += len(w)
		}
	}
	return
}
```

#### TypeScript

```ts
function countCharacters(words: string[], chars: string): number {
    const idx = (c: string) => c.charCodeAt(0) - 'a'.charCodeAt(0);
    const cnt = new Array(26).fill(0);
    for (const c of chars) {
        cnt[idx(c)]++;
    }
    let ans = 0;
    for (const w of words) {
        const wc = new Array(26).fill(0);
        let ok = true;
        for (const c of w) {
            if (++wc[idx(c)] > cnt[idx(c)]) {
                ok = false;
                break;
            }
        }
        if (ok) {
            ans += w.length;
        }
    }
    return ans;
}
```

#### PHP

```php
class Solution {
    /**
     * @param String[] $words
     * @param String $chars
     * @return Integer
     */
    function countCharacters($words, $chars) {
        $sum = 0;
        for ($i = 0; $i < strlen($chars); $i++) {
            $hashtable[$chars[$i]] += 1;
        }
        for ($j = 0; $j < count($words); $j++) {
            $tmp = $hashtable;
            $sum += strlen($words[$j]);
            for ($k = 0; $k < strlen($words[$j]); $k++) {
                if (!isset($tmp[$words[$j][$k]]) || $tmp[$words[$j][$k]] === 0) {
                    $sum -= strlen($words[$j]);
                    break;
                }
                $tmp[$words[$j][$k]] -= 1;
            }
        }
        return $sum;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
