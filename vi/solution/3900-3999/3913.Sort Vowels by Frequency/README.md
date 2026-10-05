---
comments: true
difficulty: Medium
rating: 1524
source: Weekly Contest 499 Q2
---

<!-- problem:start -->

# [3913. Sort Vowels by Frequency](https://leetcode.com/problems/sort-vowels-by-frequency)

[中文文档](/solution/3900-3999/3913.Sort%20Vowels%20by%20Frequency/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> gồm các ký tự tiếng Anh viết thường.</p>

<p>Chỉ sắp xếp lại <strong>các nguyên âm</strong> trong chuỗi sao cho chúng xuất hiện theo thứ tự <strong>không tăng dần</strong> của tần suất.</p>

<p>Nếu nhiều nguyên âm có cùng <strong>tần suất</strong>, hãy sắp xếp chúng theo vị trí <strong>xuất hiện đầu tiên</strong> trong <code>s</code>.</p>

<p>Trả về chuỗi sau khi đã sửa đổi.</p>

<p>Các nguyên âm là <code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> và <code>&#39;u&#39;</code>.</p>

<p><strong>Tần suất</strong> của một chữ cái là số lần chữ cái đó xuất hiện trong chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;leetcode&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;leetcedo&quot;</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Các nguyên âm trong chuỗi là <code>[&#39;e&#39;, &#39;e&#39;, &#39;o&#39;, &#39;e&#39;]</code> với tần suất: <code>e = 3</code>, <code>o = 1</code>.</li>
	<li>Sắp xếp theo thứ tự không tăng dần của tần suất rồi đặt chúng trở lại các vị trí nguyên âm, ta được <code>&quot;leetcedo&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aeiaaioooa&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;aaaaoooiie&quot;</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Các nguyên âm trong chuỗi là <code>[&#39;a&#39;, &#39;e&#39;, &#39;i&#39;, &#39;a&#39;, &#39;a&#39;, &#39;i&#39;, &#39;o&#39;, &#39;o&#39;, &#39;o&#39;, &#39;a&#39;]</code> với tần suất: <code>a = 4</code>, <code>o = 3</code>, <code>i = 2</code>, <code>e = 1</code>.</li>
	<li>Sắp xếp chúng theo thứ tự không tăng dần của tần suất rồi đặt chúng trở lại các vị trí nguyên âm, ta được <code>&quot;aaaaoooiie&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;baeiou&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;baeiou&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mỗi nguyên âm xuất hiện đúng một lần, nên tất cả có cùng tần suất.</li>
	<li>Do đó, chúng giữ nguyên thứ tự tương đối dựa trên lần xuất hiện đầu tiên, và chuỗi không thay đổi.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các ký tự tiếng Anh viết thường</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Sắp xếp tùy chỉnh

<!-- thinking:start -->

> **Tư duy**
>
> Nếu sắp xếp toàn bộ chuỗi, các phụ âm cũng sẽ bị di chuyển. Ta chỉ sắp xếp lại các nguyên âm, đồng thời khi tần suất bằng nhau phải giữ thứ tự theo lần xuất hiện đầu tiên.
>
> Lần duyệt đầu tiên đếm tần suất các nguyên âm và ghi nhận mỗi nguyên âm tại lần xuất hiện đầu tiên; sau đó sắp xếp danh sách này theo tần suất giảm dần. Lần duyệt thứ hai dùng một con trỏ để lần lượt lấy các phần tử trong danh sách đã sắp xếp và ghi nguyên âm hiện tại vào các vị trí nguyên âm ban đầu.
>
> Các phụ âm giữ nguyên vị trí; khi số lần còn lại của một nguyên âm bằng 0, con trỏ được tăng lên, nên các nguyên âm có tần suất cao hơn sẽ được ghi trước.

<!-- thinking:end -->

Ta có thể dùng một bảng băm $\textit{cnt}$ để ghi nhận tần suất của mỗi nguyên âm. Đồng thời, ta cần một danh sách $\textit{vowels}$ để lưu các nguyên âm xuất hiện trong chuỗi, theo thứ tự xuất hiện đầu tiên của chúng.

Sau đó, ta sắp xếp danh sách $\textit{vowels}$ bằng một comparator tùy chỉnh: các nguyên âm được sắp xếp theo thứ tự không tăng dần của tần suất.

Cuối cùng, ta duyệt qua chuỗi, thay mỗi nguyên âm bằng chữ cái tương ứng trong danh sách $\textit{vowels}$ và cập nhật tần suất trong bảng băm. Khi tần suất của một nguyên âm bằng 0, ta di chuyển con trỏ trong danh sách $\textit{vowels}$ tiến lên một vị trí.

Độ phức tạp thời gian là $O(n + |\Sigma| \log |\Sigma|)$ và độ phức tạp không gian là $O(n + |\Sigma|)$, trong đó $n$ là độ dài chuỗi và $\Sigma$ là tập các nguyên âm xuất hiện trong chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortVowels(self, s: str) -> str:
        st = set("aeiou")
        vowels = []
        cnt = Counter()
        for c in s:
            if c not in st:
                continue
            if c not in cnt:
                vowels.append(c)
            cnt[c] += 1
        vowels.sort(key=lambda c: -cnt[c])
        ans = list(s)
        i = 0
        for k, c in enumerate(s):
            if c not in st:
                continue
            ans[k] = c = vowels[i]
            cnt[c] -= 1
            if cnt[c] == 0:
                i += 1
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String sortVowels(String s) {
        Set<Character> st = Set.of('a', 'e', 'i', 'o', 'u');
        List<Character> vowels = new ArrayList<>();
        Map<Character, Integer> cnt = new HashMap<>();
        for (char c : s.toCharArray()) {
            if (!st.contains(c)) {
                continue;
            }
            if (!cnt.containsKey(c)) {
                vowels.add(c);
            }
            cnt.merge(c, 1, Integer::sum);
        }
        vowels.sort((a, b) -> cnt.get(b) - cnt.get(a));
        char[] ans = s.toCharArray();
        int i = 0;
        for (int k = 0; k < s.length(); k++) {
            char c = s.charAt(k);
            if (!st.contains(c)) {
                continue;
            }
            ans[k] = c = vowels.get(i);
            cnt.merge(c, -1, Integer::sum);
            if (cnt.get(c) == 0) {
                i++;
            }
        }
        return new String(ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string sortVowels(string s) {
        unordered_set<char> st = {'a', 'e', 'i', 'o', 'u'};
        vector<char> vowels;
        unordered_map<char, int> cnt;
        for (char c : s) {
            if (!st.count(c)) {
                continue;
            }
            if (!cnt.count(c)) {
                vowels.push_back(c);
            }
            cnt[c]++;
        }
        sort(vowels.begin(), vowels.end(), [&](char a, char b) {
            return cnt[a] > cnt[b];
        });
        string ans = s;
        int i = 0;
        for (int k = 0; k < s.size(); k++) {
            if (!st.count(s[k])) {
                continue;
            }
            char c = vowels[i];
            ans[k] = c;
            if (--cnt[c] == 0) {
                i++;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func sortVowels(s string) string {
	st := map[rune]bool{'a': true, 'e': true, 'i': true, 'o': true, 'u': true}
	var vowels []rune
	cnt := make(map[rune]int)
	for _, c := range s {
		if !st[c] {
			continue
		}
		if _, ok := cnt[c]; !ok {
			vowels = append(vowels, c)
		}
		cnt[c]++
	}
	sort.Slice(vowels, func(i, j int) bool {
		return cnt[vowels[i]] > cnt[vowels[j]]
	})
	ans := []rune(s)
	i := 0
	for k, c := range s {
		if !st[c] {
			continue
		}
		char := vowels[i]
		ans[k] = char
		cnt[char]--
		if cnt[char] == 0 {
			i++
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function sortVowels(s: string): string {
    const st = new Set('aeiou');
    const vowels: string[] = [];
    const cnt: Map<string, number> = new Map();
    for (const c of s) {
        if (!st.has(c)) {
            continue;
        }
        if (!cnt.has(c)) {
            vowels.push(c);
        }
        cnt.set(c, (cnt.get(c) || 0) + 1);
    }
    vowels.sort((a, b) => (cnt.get(b) || 0) - (cnt.get(a) || 0));
    const ans = s.split('');
    let i = 0;
    for (let k = 0; k < s.length; k++) {
        let c = s[k];
        if (!st.has(c)) {
            continue;
        }
        c = vowels[i];
        ans[k] = c;
        cnt.set(c, (cnt.get(c) || 0) - 1);
        if (cnt.get(c) === 0) {
            i++;
        }
    }
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
