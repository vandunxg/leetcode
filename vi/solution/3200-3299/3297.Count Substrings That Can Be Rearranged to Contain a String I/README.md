---
comments: true
difficulty: Medium
rating: 1847
source: Weekly Contest 416 Q3
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [3297. Count Substrings That Can Be Rearranged to Contain a String I](https://leetcode.com/problems/count-substrings-that-can-be-rearranged-to-contain-a-string-i)

[中文文档](/solution/3200-3299/3297.Count%20Substrings%20That%20Can%20Be%20Rearranged%20to%20Contain%20a%20String%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>word1</code> và <code>word2</code>.</p>

<p>Một chuỗi <code>x</code> được gọi là <strong>hợp lệ</strong> nếu có thể sắp xếp lại <code>x</code> để <code>word2</code> trở thành <span data-keyword="string-prefix">tiền tố</span>.</p>

<p>Trả về tổng số <span data-keyword="substring-nonempty">chuỗi con</span> <strong>hợp lệ</strong> của <code>word1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word1 = &quot;bcca&quot;, word2 = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi con hợp lệ duy nhất là <code>&quot;bcca&quot;</code>, có thể được sắp xếp lại thành <code>&quot;abcc&quot;</code> với <code>&quot;abc&quot;</code> là tiền tố.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word1 = &quot;abcabc&quot;, word2 = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả các chuỗi con ngoại trừ các chuỗi con có kích thước 1 và kích thước 2 đều hợp lệ.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word1 = &quot;abcabc&quot;, word2 = &quot;aaabc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word1.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= word2.length &lt;= 10<sup>4</sup></code></li>
	<li><code>word1</code> và <code>word2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi con có thể được sắp xếp lại để chứa $\textit{word2}$ khi và chỉ khi nó là một siêu đa tập của $\textit{word2}$. Việc so sánh số lần xuất hiện trên mọi chuỗi con sẽ quá chậm với chuỗi dài. Tính chất bao phủ có tính đơn điệu: một cửa sổ hợp lệ vẫn hợp lệ khi mở rộng về bên phải, còn việc thu hẹp bên trái sẽ tìm được đoạn bao phủ ngắn nhất.
>
> Ta theo dõi số loại ký tự còn thiếu. Biên phải có thể làm giảm số này; khi số này bằng $0$, ta dịch biên trái. Mọi điểm bắt đầu ở bên trái biên trái hiện tại khi ghép với biên phải hiện tại đều tạo thành chuỗi con hợp lệ, nên ta cộng chỉ số biên trái vào đáp án. Chỉ cần một lần trượt.

<!-- thinking:end -->

Bài toán về cơ bản là đếm số chuỗi con trong $\textit{word1}$ chứa tất cả các ký tự trong $\textit{word2}$. Ta có thể dùng cửa sổ trượt để giải quyết bài toán này.

Trước tiên, nếu độ dài của $\textit{word1}$ nhỏ hơn độ dài của $\textit{word2}$, thì không thể để $\textit{word1}$ chứa tất cả các ký tự của $\textit{word2}$, nên ta trả về trực tiếp $0$.

Tiếp theo, ta dùng một hash table hoặc một mảng có độ dài $26$ tên là $\textit{cnt}$ để đếm số lần xuất hiện của các ký tự trong $\textit{word2}$. Sau đó, ta dùng $\textit{need}$ để ghi nhận số ký tự còn cần thêm để thỏa mãn điều kiện, ban đầu bằng độ dài của $\textit{cnt}$.

Tiếp tục, ta dùng cửa sổ trượt $\textit{win}$ để ghi nhận số lần xuất hiện của các ký tự trong cửa sổ hiện tại. Ta dùng $\textit{ans}$ để ghi nhận số chuỗi con thỏa mãn điều kiện, và $\textit{l}$ để ghi nhận biên trái của cửa sổ.

Ta duyệt qua từng ký tự trong $\textit{word1}$. Với ký tự hiện tại $c$, ta thêm nó vào $\textit{win}$. Nếu giá trị của $\textit{win}[c]$ bằng $\textit{cnt}[c]$, điều đó có nghĩa là cửa sổ hiện tại đã chứa đủ một trong các ký tự của $\textit{word2}$, nên ta giảm $\textit{need}$ đi một. Nếu $\textit{need}$ bằng $0$, điều đó có nghĩa là cửa sổ hiện tại chứa tất cả các ký tự trong $\textit{word2}$. Ta cần thu hẹp biên trái của cửa sổ cho đến khi $\textit{need}$ lớn hơn $0$. Cụ thể, nếu $\textit{win}[\textit{word1}[l]]$ bằng $\textit{cnt}[\textit{word1}[l]]$, điều đó có nghĩa là cửa sổ hiện tại đang chứa đủ một trong các ký tự của $\textit{word2}$. Sau khi thu hẹp biên trái, cửa sổ không còn thỏa mãn điều kiện, nên ta tăng $\textit{need}$ lên một và giảm $\textit{win}[\textit{word1}[l]]$ đi một. Sau đó, ta tăng $\textit{l}$ lên một. Lúc này, cửa sổ là $[l, r]$. Với mọi $0 \leq l' < l$, $[l', r]$ là các chuỗi con thỏa mãn điều kiện, có tất cả $l$ chuỗi con như vậy, và ta cộng số lượng này vào đáp án.

Sau khi duyệt qua tất cả các ký tự trong $\textit{word1}$, ta thu được đáp án.

Độ phức tạp thời gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là độ dài của $\textit{word1}$ và $\textit{word2}$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là tập ký tự. Ở đây, đó là tập các chữ cái viết thường, nên độ phức tạp không gian là hằng số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validSubstringCount(self, word1: str, word2: str) -> int:
        if len(word1) < len(word2):
            return 0
        cnt = Counter(word2)
        need = len(cnt)
        ans = l = 0
        win = Counter()
        for c in word1:
            win[c] += 1
            if win[c] == cnt[c]:
                need -= 1
            while need == 0:
                if win[word1[l]] == cnt[word1[l]]:
                    need += 1
                win[word1[l]] -= 1
                l += 1
            ans += l
        return ans
```

#### Java

```java
class Solution {
    public long validSubstringCount(String word1, String word2) {
        if (word1.length() < word2.length()) {
            return 0;
        }
        int[] cnt = new int[26];
        int need = 0;
        for (int i = 0; i < word2.length(); ++i) {
            if (++cnt[word2.charAt(i) - 'a'] == 1) {
                ++need;
            }
        }
        long ans = 0;
        int[] win = new int[26];
        for (int l = 0, r = 0; r < word1.length(); ++r) {
            int c = word1.charAt(r) - 'a';
            if (++win[c] == cnt[c]) {
                --need;
            }
            while (need == 0) {
                c = word1.charAt(l) - 'a';
                if (win[c] == cnt[c]) {
                    ++need;
                }
                --win[c];
                ++l;
            }
            ans += l;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long validSubstringCount(string word1, string word2) {
        if (word1.size() < word2.size()) {
            return 0;
        }
        int cnt[26]{};
        int need = 0;
        for (char& c : word2) {
            if (++cnt[c - 'a'] == 1) {
                ++need;
            }
        }
        long long ans = 0;
        int win[26]{};
        int l = 0;
        for (char& c : word1) {
            int i = c - 'a';
            if (++win[i] == cnt[i]) {
                --need;
            }
            while (need == 0) {
                i = word1[l] - 'a';
                if (win[i] == cnt[i]) {
                    ++need;
                }
                --win[i];
                ++l;
            }
            ans += l;
        }
        return ans;
    }
};
```

#### Go

```go
func validSubstringCount(word1 string, word2 string) (ans int64) {
	if len(word1) < len(word2) {
		return 0
	}
	cnt := [26]int{}
	need := 0
	for _, c := range word2 {
		cnt[c-'a']++
		if cnt[c-'a'] == 1 {
			need++
		}
	}
	win := [26]int{}
	l := 0
	for _, c := range word1 {
		i := int(c - 'a')
		win[i]++
		if win[i] == cnt[i] {
			need--
		}
		for need == 0 {
			i = int(word1[l] - 'a')
			if win[i] == cnt[i] {
				need++
			}
			win[i]--
			l++
		}
		ans += int64(l)
	}
	return
}
```

#### TypeScript

```ts
function validSubstringCount(word1: string, word2: string): number {
    if (word1.length < word2.length) {
        return 0;
    }
    const cnt: number[] = Array(26).fill(0);
    let need: number = 0;
    for (const c of word2) {
        if (++cnt[c.charCodeAt(0) - 97] === 1) {
            ++need;
        }
    }
    const win: number[] = Array(26).fill(0);
    let [ans, l] = [0, 0];
    for (const c of word1) {
        const i = c.charCodeAt(0) - 97;
        if (++win[i] === cnt[i]) {
            --need;
        }
        while (need === 0) {
            const j = word1[l].charCodeAt(0) - 97;
            if (win[j] === cnt[j]) {
                ++need;
            }
            --win[j];
            ++l;
        }
        ans += l;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
