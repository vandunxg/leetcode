---
comments: true
difficulty: Easy
rating: 1458
source: Weekly Contest 266 Q1
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [2062. Count Vowel Substrings of a String](https://leetcode.com/problems/count-vowel-substrings-of-a-string)

[中文文档](/solution/2000-2099/2062.Count%20Vowel%20Substrings%20of%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp (không rỗng) trong một chuỗi.</p>

<p><strong>Chuỗi con nguyên âm</strong> là một chuỗi con <strong>chỉ</strong> gồm các nguyên âm (<code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> và <code>&#39;u&#39;</code>) và chứa đủ <strong>cả năm</strong> nguyên âm.</p>

<p>Cho một chuỗi <code>word</code>, hãy trả về <em>số lượng <strong>chuỗi con nguyên âm</strong> trong</em> <code>word</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;aeiouu&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các chuỗi con nguyên âm của word là (được gạch chân) như sau:
- &quot;<strong><u>aeiou</u></strong>u&quot;
- &quot;<strong><u>aeiouu</u></strong>&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;unicornarihan&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không chứa đủ cả 5 nguyên âm nên không có chuỗi con nguyên âm nào.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;cuaieuouac&quot;
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Các chuỗi con nguyên âm của word là (được gạch chân) như sau:
- &quot;c<strong><u>uaieuo</u></strong>uac&quot;
- &quot;c<strong><u>uaieuou</u></strong>ac&quot;
- &quot;c<strong><u>uaieuoua</u></strong>c&quot;
- &quot;cu<strong><u>aieuo</u></strong>uac&quot;
- &quot;cu<strong><u>aieuou</u></strong>ac&quot;
- &quot;cu<strong><u>aieuoua</u></strong>c&quot;
- &quot;cua<strong><u>ieuoua</u></strong>c&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 100</code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê vét cạn + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Với $n \le 100$, việc xét tất cả các chuỗi con có độ dài $O(n^2)$ là hoàn toàn phù hợp. Một chuỗi con nguyên âm chỉ gồm các nguyên âm và phải chứa đủ cả năm nguyên âm. Cố định đầu trái rồi mở rộng sang phải; gặp phụ âm thì dừng xét đầu trái đó, còn tập hợp có kích thước $5$ thì tăng đáp án lên một.
>
> Dùng một hash set để lưu các nguyên âm đã xuất hiện trong đoạn hiện tại.

<!-- thinking:end -->

Ta có thể liệt kê chỉ số bắt đầu $i$ của chuỗi con. Với mỗi chỉ số bắt đầu hiện tại, dùng một hash table để lưu các nguyên âm xuất hiện trong chuỗi con hiện tại. Sau đó liệt kê chỉ số kết thúc $j$. Nếu ký tự tại chỉ số kết thúc hiện tại không phải là nguyên âm, ta dừng vòng lặp. Ngược lại, thêm ký tự tại chỉ số kết thúc hiện tại vào hash table. Nếu số phần tử trong hash table bằng $5$, chuỗi con hiện tại là một chuỗi con nguyên âm, nên tăng kết quả lên $1$.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(C)$. Trong đó, $n$ là độ dài của chuỗi $word$, còn $C$ là kích thước của tập ký tự, bằng $5$ trong bài này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countVowelSubstrings(self, word: str) -> int:
        s = set("aeiou")
        ans, n = 0, len(word)
        for i in range(n):
            t = set()
            for c in word[i:]:
                if c not in s:
                    break
                t.add(c)
                ans += len(t) == 5
        return ans
```

#### Java

```java
class Solution {
    public int countVowelSubstrings(String word) {
        int n = word.length();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            Set<Character> t = new HashSet<>();
            for (int j = i; j < n; ++j) {
                char c = word.charAt(j);
                if (!isVowel(c)) {
                    break;
                }
                t.add(c);
                if (t.size() == 5) {
                    ++ans;
                }
            }
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
    int countVowelSubstrings(string word) {
        int ans = 0;
        int n = word.size();
        for (int i = 0; i < n; ++i) {
            unordered_set<char> t;
            for (int j = i; j < n; ++j) {
                char c = word[j];
                if (!isVowel(c)) break;
                t.insert(c);
                ans += t.size() == 5;
            }
        }
        return ans;
    }

    bool isVowel(char c) {
        return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
    }
};
```

#### Go

```go
func countVowelSubstrings(word string) int {
	ans, n := 0, len(word)
	for i := range word {
		t := map[byte]bool{}
		for j := i; j < n; j++ {
			c := word[j]
			if !(c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
				break
			}
			t[c] = true
			if len(t) == 5 {
				ans++
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countVowelSubstrings(word: string): number {
    let ans = 0;
    const n = word.length;
    for (let i = 0; i < n; ++i) {
        const t = new Set<string>();
        for (let j = i; j < n; ++j) {
            const c = word[j];
            if (!(c === 'a' || c === 'e' || c === 'i' || c === 'o' || c === 'u')) {
                break;
            }
            t.add(c);
            if (t.size === 5) {
                ans++;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
