---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [748. Shortest Completing Word](https://leetcode.com/problems/shortest-completing-word)

[中文文档](/solution/0700-0799/0748.Shortest%20Completing%20Word/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>licensePlate</code> và mảng chuỗi <code>words</code>, hãy tìm từ <strong>hoàn chỉnh ngắn nhất</strong> trong <code>words</code>.</p>

<p>Từ <strong>hoàn chỉnh</strong> là từ <strong>chứa tất cả chữ cái</strong> trong <code>licensePlate</code>. <strong>Bỏ qua chữ số và dấu cách</strong> trong <code>licensePlate</code>, đồng thời không phân biệt <strong>chữ hoa và chữ thường</strong>. Nếu một chữ cái xuất hiện nhiều lần trong <code>licensePlate</code>, từ đó phải chứa chữ cái ấy ít nhất với số lần tương ứng.</p>

<p>Ví dụ, nếu <code>licensePlate</code><code> = &quot;aBc 12c&quot;</code>, thì chuỗi có các chữ cái <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code> (không phân biệt hoa thường) và hai chữ <code>&#39;c&#39;</code>. Các từ <strong>hoàn chỉnh</strong> có thể là <code>&quot;abccdef&quot;</code>, <code>&quot;caaacab&quot;</code> và <code>&quot;cbca&quot;</code>.</p>

<p>Hãy trả về <em>từ <strong>hoàn chỉnh</strong> ngắn nhất trong </em><code>words</code><em>.</em> Đảm bảo luôn có đáp án. Nếu có nhiều từ <strong>hoàn chỉnh</strong> cùng ngắn nhất, hãy trả về từ xuất hiện <strong>đầu tiên</strong> trong <code>words</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> licensePlate = &quot;1s3 PSt&quot;, words = [&quot;step&quot;,&quot;steps&quot;,&quot;stripe&quot;,&quot;stepple&quot;]
<strong>Đầu ra:</strong> &quot;steps&quot;
<strong>Giải thích:</strong> licensePlate có các chữ cái &#39;s&#39;, &#39;p&#39;, &#39;s&#39; (không phân biệt hoa thường) và &#39;t&#39;.
&quot;step&quot; có &#39;t&#39; và &#39;p&#39;, nhưng chỉ có một &#39;s&#39;.
&quot;steps&quot; có &#39;t&#39;, &#39;p&#39; và cả hai chữ &#39;s&#39;.
&quot;stripe&quot; thiếu một chữ &#39;s&#39;.
&quot;stepple&quot; thiếu một chữ &#39;s&#39;.
Vì &quot;steps&quot; là từ duy nhất chứa đủ các chữ cái, đó là đáp án.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> licensePlate = &quot;1s3 456&quot;, words = [&quot;looks&quot;,&quot;pest&quot;,&quot;stew&quot;,&quot;show&quot;]
<strong>Đầu ra:</strong> &quot;pest&quot;
<strong>Giải thích:</strong> licensePlate chỉ có chữ &#39;s&#39;. Tất cả các từ đều có &#39;s&#39;, nhưng trong số đó &quot;pest&quot;, &quot;stew&quot; và &quot;show&quot; là các từ ngắn nhất. Đáp án là &quot;pest&quot; vì từ này xuất hiện trước hai từ còn lại.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= licensePlate.length &lt;= 7</code></li>
	<li><code>licensePlate</code> gồm chữ số, chữ cái (viết hoa hoặc viết thường) hoặc dấu cách <code>&#39; &#39;</code>.</li>
	<li><code>1 &lt;= words.length &lt;= 1000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 15</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm tần suất

<!-- thinking:start -->

> **Tư duy**
>
> Từ hoàn chỉnh phải chứa đủ chữ cái trên biển số (không phân biệt hoa thường và bỏ qua chữ số); ta cần chọn từ ngắn nhất, xuất hiện sớm nhất. Chỉ cần đếm tần suất.
>
> Đếm tần suất chữ cái trên biển số, rồi duyệt các từ: bỏ qua từ không ngắn hơn đáp án hiện tại và chọn từ đầu tiên có đủ số lượng chữ cái yêu cầu.
>
> Bảng chữ cái có $26$ ký tự; mỗi lần kiểm tra cần thời gian tuyến tính theo độ dài từ.

<!-- thinking:end -->

Đầu tiên, ta dùng hash table hoặc mảng $cnt$ độ dài $26$ để đếm tần suất mỗi chữ cái trong chuỗi `licensePlate`. Lưu ý chuyển tất cả chữ cái thành chữ thường trước khi đếm.

Sau đó, ta duyệt từng từ $w$ trong mảng `words`. Nếu $w$ dài hơn đáp án $ans$, ta bỏ qua từ này. Ngược lại, dùng hash table khác hoặc mảng $t$ độ dài $26$ để đếm tần suất từng chữ cái trong $w$. Nếu tần suất của bất kỳ chữ cái nào trong $t$ thấp hơn tần suất tương ứng trong $cnt$, ta cũng bỏ qua từ đó. Nếu không, ta đã tìm được từ thỏa mãn và cập nhật đáp án $ans$ thành $w$.

Độ phức tạp thời gian là $O(n \times |\Sigma|)$ và độ phức tạp không gian là $O(|\Sigma|)$. Trong đó, $n$ là số từ trong mảng `words`, còn $\Sigma$ là tập ký tự. Ở đây, tập ký tự gồm toàn bộ chữ cái viết thường nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestCompletingWord(self, licensePlate: str, words: List[str]) -> str:
        cnt = Counter(c.lower() for c in licensePlate if c.isalpha())
        ans = None
        for w in words:
            if ans and len(w) >= len(ans):
                continue
            t = Counter(w)
            if all(v <= t[c] for c, v in cnt.items()):
                ans = w
        return ans
```

#### Java

```java
class Solution {
    public String shortestCompletingWord(String licensePlate, String[] words) {
        int[] cnt = new int[26];
        for (int i = 0; i < licensePlate.length(); ++i) {
            char c = licensePlate.charAt(i);
            if (Character.isLetter(c)) {
                cnt[Character.toLowerCase(c) - 'a']++;
            }
        }
        String ans = "";
        for (String w : words) {
            if (!ans.isEmpty() && w.length() >= ans.length()) {
                continue;
            }
            int[] t = new int[26];
            for (int i = 0; i < w.length(); ++i) {
                t[w.charAt(i) - 'a']++;
            }
            boolean ok = true;
            for (int i = 0; i < 26; ++i) {
                if (t[i] < cnt[i]) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                ans = w;
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
    string shortestCompletingWord(string licensePlate, vector<string>& words) {
        int cnt[26]{};
        for (char& c : licensePlate) {
            if (isalpha(c)) {
                ++cnt[tolower(c) - 'a'];
            }
        }
        string ans;
        for (auto& w : words) {
            if (ans.size() && ans.size() <= w.size()) {
                continue;
            }
            int t[26]{};
            for (char& c : w) {
                ++t[c - 'a'];
            }
            bool ok = true;
            for (int i = 0; i < 26; ++i) {
                if (cnt[i] > t[i]) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                ans = w;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func shortestCompletingWord(licensePlate string, words []string) (ans string) {
	cnt := [26]int{}
	for _, c := range licensePlate {
		if unicode.IsLetter(c) {
			cnt[unicode.ToLower(c)-'a']++
		}
	}
	for _, w := range words {
		if len(ans) > 0 && len(ans) <= len(w) {
			continue
		}
		t := [26]int{}
		for _, c := range w {
			t[c-'a']++
		}
		ok := true
		for i, v := range cnt {
			if t[i] < v {
				ok = false
				break
			}
		}
		if ok {
			ans = w
		}
	}
	return
}
```

#### TypeScript

```ts
function shortestCompletingWord(licensePlate: string, words: string[]): string {
    const cnt: number[] = Array(26).fill(0);
    for (const c of licensePlate) {
        const i = c.toLowerCase().charCodeAt(0) - 97;
        if (0 <= i && i < 26) {
            ++cnt[i];
        }
    }
    let ans = '';
    for (const w of words) {
        if (ans.length && ans.length <= w.length) {
            continue;
        }
        const t = Array(26).fill(0);
        for (const c of w) {
            ++t[c.charCodeAt(0) - 97];
        }
        let ok = true;
        for (let i = 0; i < 26; ++i) {
            if (t[i] < cnt[i]) {
                ok = false;
                break;
            }
        }
        if (ok) {
            ans = w;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn shortest_completing_word(license_plate: String, words: Vec<String>) -> String {
        let mut cnt = vec![0; 26];
        for c in license_plate.chars() {
            if c.is_ascii_alphabetic() {
                cnt[((c.to_ascii_lowercase() as u8) - b'a') as usize] += 1;
            }
        }
        let mut ans = String::new();
        for w in words {
            if !ans.is_empty() && w.len() >= ans.len() {
                continue;
            }
            let mut t = vec![0; 26];
            for c in w.chars() {
                t[((c as u8) - b'a') as usize] += 1;
            }
            let mut ok = true;
            for i in 0..26 {
                if t[i] < cnt[i] {
                    ok = false;
                    break;
                }
            }
            if ok {
                ans = w;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
