---
comments: true
difficulty: Easy
rating: 1294
source: Weekly Contest 293 Q1
tags:
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [2273. Find Resultant Array After Removing Anagrams](https://leetcode.com/problems/find-resultant-array-after-removing-anagrams)

[中文文档](/solution/2200-2299/2273.Find%20Resultant%20Array%20After%20Removing%20Anagrams/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi <code>words</code> được đánh chỉ số từ <strong>0</strong>, trong đó <code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Trong một thao tác, chọn một chỉ số bất kỳ <code>i</code> sao cho <code>0 &lt; i &lt; words.length</code>, đồng thời <code>words[i - 1]</code> và <code>words[i]</code> là <strong>anagram</strong>, rồi <strong>xóa</strong> <code>words[i]</code> khỏi <code>words</code>. Tiếp tục thực hiện thao tác này chừng nào còn chọn được một chỉ số thỏa mãn các điều kiện.</p>

<p>Trả về <code>words</code> <em>sau khi thực hiện tất cả các thao tác</em>. Có thể chứng minh rằng việc chọn các chỉ số theo <strong>bất kỳ thứ tự</strong> nào cũng cho cùng một kết quả.</p>

<p><strong>Anagram</strong> là một từ hoặc cụm từ được tạo thành bằng cách sắp xếp lại các chữ cái của một từ hoặc cụm từ khác, sử dụng đúng một lần tất cả các chữ cái ban đầu. Ví dụ, <code>&quot;dacb&quot;</code> là một anagram của <code>&quot;abdc&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;abba&quot;,&quot;baba&quot;,&quot;bbaa&quot;,&quot;cd&quot;,&quot;cd&quot;]
<strong>Đầu ra:</strong> [&quot;abba&quot;,&quot;cd&quot;]
<strong>Giải thích:</strong>
Một trong các cách thu được mảng kết quả là thực hiện các thao tác sau:
- Vì words[2] = &quot;bbaa&quot; và words[1] = &quot;baba&quot; là anagram, ta chọn chỉ số 2 và xóa words[2].
  Khi đó words = [&quot;abba&quot;,&quot;baba&quot;,&quot;cd&quot;,&quot;cd&quot;].
- Vì words[1] = &quot;baba&quot; và words[0] = &quot;abba&quot; là anagram, ta chọn chỉ số 1 và xóa words[1].
  Khi đó words = [&quot;abba&quot;,&quot;cd&quot;,&quot;cd&quot;].
- Vì words[2] = &quot;cd&quot; và words[1] = &quot;cd&quot; là anagram, ta chọn chỉ số 2 và xóa words[2].
  Khi đó words = [&quot;abba&quot;,&quot;cd&quot;].
Ta không thể thực hiện thêm thao tác nào, nên [&quot;abba&quot;,&quot;cd&quot;] là đáp án cuối cùng.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;,&quot;d&quot;,&quot;e&quot;]
<strong>Đầu ra:</strong> [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;,&quot;d&quot;,&quot;e&quot;]
<strong>Giải thích:</strong>
Không có hai chuỗi kề nhau nào trong words là anagram của nhau, nên không có thao tác nào được thực hiện.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 10</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ta liên tục xóa một từ là anagram của từ đứng trước nó. Các từ đều ngắn và số lượng ít, nên chỉ cần duyệt từ trái sang phải, chỉ giữ lại một từ khi nó không phải là anagram của từ được giữ lại gần nhất, vì các cặp trước đó vẫn hợp lệ.
>
> Bắt đầu với $words[0]$ và thêm $t$ khi $\textit{check}(s,t)$ cho biết chúng khác nhau. Hàm hỗ trợ so sánh số lần xuất hiện và xem các chuỗi có độ dài khác nhau là không phải anagram.

<!-- thinking:end -->

Trước tiên, ta thêm $\textit{words}[0]$ vào mảng kết quả, sau đó duyệt từ $\textit{words}[1]$. Nếu $\textit{words}[i - 1]$ và $\textit{words}[i]$ không phải là anagram, ta thêm $\textit{words}[i]$ vào mảng kết quả.

Bài toán được chuyển thành việc xác định hai chuỗi có phải là anagram hay không. Ta định nghĩa hàm hỗ trợ $\textit{check}(s, t)$ để thực hiện việc này. Nếu $s$ và $t$ không phải là anagram, ta trả về $\text{true}$; ngược lại, ta trả về $\text{false}$.

Trong hàm $\textit{check}(s, t)$, trước tiên ta kiểm tra độ dài của $s$ và $t$ có bằng nhau hay không. Nếu không bằng nhau, ta trả về $\text{true}$. Nếu bằng nhau, ta sử dụng một mảng $\textit{cnt}$ có độ dài $26$ để đếm số lần xuất hiện của mỗi ký tự trong $s$, sau đó duyệt từng ký tự trong $t$ và giảm $\textit{cnt}[c]$ đi $1$. Nếu $\textit{cnt}[c]$ nhỏ hơn $0$, ta trả về $\text{true}$. Nếu duyệt hết các ký tự trong $t$ mà không gặp vấn đề gì, nghĩa là $s$ và $t$ là anagram, và ta trả về $\text{false}$.

Độ phức tạp thời gian là $O(L)$, và độ phức tạp không gian là $O(|\Sigma|)$. Trong đó, $L$ là độ dài của mảng $\textit{words}$, còn $\Sigma$ là tập ký tự, ở đây là các chữ cái tiếng Anh viết thường, nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeAnagrams(self, words: List[str]) -> List[str]:
        def check(s: str, t: str) -> bool:
            if len(s) != len(t):
                return True
            cnt = Counter(s)
            for c in t:
                cnt[c] -= 1
                if cnt[c] < 0:
                    return True
            return False

        return [words[0]] + [t for s, t in pairwise(words) if check(s, t)]
```

#### Java

```java
class Solution {
    public List<String> removeAnagrams(String[] words) {
        List<String> ans = new ArrayList<>();
        ans.add(words[0]);
        for (int i = 1; i < words.length; ++i) {
            if (check(words[i - 1], words[i])) {
                ans.add(words[i]);
            }
        }
        return ans;
    }

    private boolean check(String s, String t) {
        if (s.length() != t.length()) {
            return true;
        }
        int[] cnt = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            ++cnt[s.charAt(i) - 'a'];
        }
        for (int i = 0; i < t.length(); ++i) {
            if (--cnt[t.charAt(i) - 'a'] < 0) {
                return true;
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
    vector<string> removeAnagrams(vector<string>& words) {
        auto check = [](string& s, string& t) -> bool {
            if (s.size() != t.size()) {
                return true;
            }
            int cnt[26]{};
            for (char& c : s) {
                ++cnt[c - 'a'];
            }
            for (char& c : t) {
                if (--cnt[c - 'a'] < 0) {
                    return true;
                }
            }
            return false;
        };

        vector<string> ans = {words[0]};
        for (int i = 1; i < words.size(); ++i) {
            if (check(words[i - 1], words[i])) {
                ans.emplace_back(words[i]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func removeAnagrams(words []string) []string {
	ans := []string{words[0]}
	check := func(s, t string) bool {
		if len(s) != len(t) {
			return true
		}
		cnt := [26]int{}
		for _, c := range s {
			cnt[c-'a']++
		}
		for _, c := range t {
			cnt[c-'a']--
			if cnt[c-'a'] < 0 {
				return true
			}
		}
		return false
	}
	for i, t := range words[1:] {
		if check(words[i], t) {
			ans = append(ans, t)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function removeAnagrams(words: string[]): string[] {
    const ans: string[] = [words[0]];
    const check = (s: string, t: string): boolean => {
        if (s.length !== t.length) {
            return true;
        }
        const cnt: number[] = Array(26).fill(0);
        for (const c of s) {
            ++cnt[c.charCodeAt(0) - 97];
        }
        for (const c of t) {
            if (--cnt[c.charCodeAt(0) - 97] < 0) {
                return true;
            }
        }
        return false;
    };
    for (let i = 1; i < words.length; ++i) {
        if (check(words[i - 1], words[i])) {
            ans.push(words[i]);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn remove_anagrams(words: Vec<String>) -> Vec<String> {
        fn check(s: &str, t: &str) -> bool {
            if s.len() != t.len() {
                return true;
            }
            let mut cnt = [0; 26];
            for c in s.bytes() {
                cnt[(c - b'a') as usize] += 1;
            }
            for c in t.bytes() {
                let idx = (c - b'a') as usize;
                cnt[idx] -= 1;
                if cnt[idx] < 0 {
                    return true;
                }
            }
            false
        }

        let mut ans = vec![words[0].clone()];
        for i in 1..words.len() {
            if check(&words[i - 1], &words[i]) {
                ans.push(words[i].clone());
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {string[]} words
 * @return {string[]}
 */
var removeAnagrams = function (words) {
    const ans = [words[0]];
    const check = (s, t) => {
        if (s.length !== t.length) {
            return true;
        }
        const cnt = Array(26).fill(0);
        for (const c of s) {
            ++cnt[c.charCodeAt() - 97];
        }
        for (const c of t) {
            if (--cnt[c.charCodeAt() - 97] < 0) {
                return true;
            }
        }
        return false;
    };
    for (let i = 1; i < words.length; ++i) {
        if (check(words[i - 1], words[i])) {
            ans.push(words[i]);
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
