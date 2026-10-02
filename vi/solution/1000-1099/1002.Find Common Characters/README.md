---
comments: true
difficulty: Easy
rating: 1279
source: Weekly Contest 126 Q1
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [1002. Find Common Characters](https://leetcode.com/problems/find-common-characters)

[中文文档](/solution/1000-1099/1002.Find%20Common%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>words</code>, trả về <em>mảng chứa mọi ký tự xuất hiện trong tất cả chuỗi thuộc </em><code>words</code><em> (kể cả các ký tự lặp)</em>. Có thể trả kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> words = ["bella","label","roller"]
<strong>Đầu ra:</strong> ["e","l","l"]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> words = ["cool","lock","cook"]
<strong>Đầu ra:</strong> ["c","o"]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 100</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Có thể duyệt từng chuỗi để đếm ký tự vì số chuỗi và độ dài mỗi chuỗi đều không quá $100$. Tuy nhiên, lặp lại việc quét cùng bảng chữ cái sẽ gây ra nhiều phép so sánh không cần thiết.
>
> Một ký tự xuất hiện trong đáp án số lần bằng tần suất nhỏ nhất của nó trong tất cả chuỗi — tức giao của các multiset.
>
> Vì vậy, ta đếm tần suất trong chuỗi đầu tiên, lấy giá trị $\min$ theo từng ký tự với các chuỗi còn lại, rồi tạo đáp án từ các tần suất giao nhau. Bảng chữ cái có kích thước $26$, nên bộ nhớ phụ là hằng số.

<!-- thinking:end -->

Ta dùng mảng $cnt$ có độ dài $26$ để lưu số lần xuất hiện nhỏ nhất của mỗi ký tự trong tất cả chuỗi. Cuối cùng, duyệt mảng $cnt$ và thêm vào đáp án các ký tự có tần suất lớn hơn $0$.

Độ phức tạp thời gian là $O(n \sum w_i)$ và độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là số chuỗi trong mảng $words$, $w_i$ là độ dài chuỗi thứ $i$ và $|\Sigma|$ là kích thước bảng ký tự, bằng $26$ trong bài này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def commonChars(self, words: List[str]) -> List[str]:
        cnt = Counter(words[0])
        for w in words:
            t = Counter(w)
            for c in cnt:
                cnt[c] = min(cnt[c], t[c])
        return list(cnt.elements())
```

#### Java

```java
class Solution {
    public List<String> commonChars(String[] words) {
        int[] cnt = new int[26];
        Arrays.fill(cnt, 20000);
        for (var w : words) {
            int[] t = new int[26];
            for (int i = 0; i < w.length(); ++i) {
                ++t[w.charAt(i) - 'a'];
            }
            for (int i = 0; i < 26; ++i) {
                cnt[i] = Math.min(cnt[i], t[i]);
            }
        }
        List<String> ans = new ArrayList<>();
        for (int i = 0; i < 26; ++i) {
            ans.addAll(Collections.nCopies(cnt[i], String.valueOf((char) ('a' + i))));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> commonChars(vector<string>& words) {
        vector<int> cnt(26, 20000);
        for (const auto& w : words) {
            vector<int> t(26, 0);
            for (char c : w) {
                ++t[c - 'a'];
            }
            for (int i = 0; i < 26; ++i) {
                cnt[i] = min(cnt[i], t[i]);
            }
        }
        vector<string> ans;
        for (int i = 0; i < 26; ++i) {
            for (int j = 0; j < cnt[i]; ++j) {
                ans.push_back(string(1, 'a' + i));
            }
        }
        return ans;
    }
};
```

#### Go

```go
func commonChars(words []string) (ans []string) {
	cnt := make([]int, 26)
	for i := range cnt {
		cnt[i] = 20000
	}
	for _, w := range words {
		t := make([]int, 26)
		for _, c := range w {
			t[c-'a']++
		}
		for i := 0; i < 26; i++ {
			cnt[i] = min(cnt[i], t[i])
		}
	}
	for i := 0; i < 26; i++ {
		for j := 0; j < cnt[i]; j++ {
			ans = append(ans, string('a'+rune(i)))
		}
	}
	return ans
}
```

#### TypeScript

```ts
function commonChars(words: string[]): string[] {
    const cnt = Array(26).fill(20000);
    const aCode = 'a'.charCodeAt(0);
    for (const w of words) {
        const t = Array(26).fill(0);
        for (const c of w) {
            t[c.charCodeAt(0) - aCode]++;
        }
        for (let i = 0; i < 26; i++) {
            cnt[i] = Math.min(cnt[i], t[i]);
        }
    }
    const ans: string[] = [];
    for (let i = 0; i < 26; i++) {
        cnt[i] && ans.push(...String.fromCharCode(i + aCode).repeat(cnt[i]));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
