---
comments: true
difficulty: Hard
rating: 2376
source: Weekly Contest 485 Q4
tags:
    - Stack
    - Greedy
    - Hash Table
    - String
    - Monotonic Stack
---

<!-- problem:start -->

# [3816. Lexicographically Smallest String After Deleting Duplicate Characters](https://leetcode.com/problems/lexicographically-smallest-string-after-deleting-duplicate-characters)

[中文文档](/solution/3800-3899/3816.Lexicographically%20Smallest%20String%20After%20Deleting%20Duplicate%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Bạn có thể thực hiện thao tác sau bao nhiêu lần tùy ý (có thể không lần nào):</p>

<ul>
	<li>Chọn một chữ cái xuất hiện <strong>ít nhất hai lần</strong> trong chuỗi hiện tại <code>s</code> và xóa <strong>một</strong> lần xuất hiện của chữ cái đó.</li>
</ul>

<p>Hãy trả về chuỗi kết quả <strong><span data-keyword="lexicographically-smaller-string">nhỏ nhất theo thứ tự từ điển</span></strong> có thể tạo ra bằng cách này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aaccb&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;aacb&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể tạo ra các chuỗi <code>&quot;acb&quot;</code>, <code>&quot;aacb&quot;</code>, <code>&quot;accb&quot;</code> và <code>&quot;aaccb&quot;</code>. Trong số đó, <code>&quot;aacb&quot;</code> là chuỗi nhỏ nhất theo thứ tự từ điển.</p>

<p>Chẳng hạn, ta có thể thu được <code>&quot;aacb&quot;</code> bằng cách chọn <code>&#39;c&#39;</code> và xóa lần xuất hiện đầu tiên của nó.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;z&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;z&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta không thể thực hiện thao tác nào. Chuỗi duy nhất có thể tạo ra là <code>&quot;z&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ngăn xếp đơn điệu

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể xóa một lần xuất hiện của bất kỳ chữ cái nào vẫn còn xuất hiện ít nhất hai lần, nhằm tìm chuỗi nhỏ nhất theo thứ tự từ điển. Với $|s| \le 10^5$, không thể liệt kê các cách xóa.
>
> Mỗi chữ cái phải còn lại ít nhất một lần. Khi một chữ cái vẫn còn dư, ta nên pop phần tử lớn hơn ở đỉnh stack để thay bằng ký tự hiện tại.
>
> Đếm tần suất, sau đó duyệt từ trái sang phải bằng monotonic stack: pop đỉnh lớn hơn nếu chữ cái đó vẫn còn xuất hiện về sau. Sau khi duyệt xong, xóa các phần tử trùng lặp còn lại ở cuối stack.
>
> Stack thu được là dãy nhỏ nhất vẫn giữ lại ít nhất một lần xuất hiện của mỗi chữ cái.

<!-- thinking:end -->

Ta có thể dùng một stack $\textit{stk}$ để lưu các ký tự của chuỗi kết quả, và một hash table $\textit{cnt}$ để ghi nhận số lần xuất hiện của mỗi ký tự trong chuỗi $s$.

Đầu tiên, ta khởi tạo $\textit{cnt}$ để đếm số lần xuất hiện của mỗi ký tự trong chuỗi $s$. Sau đó, ta duyệt qua từng ký tự $c$ trong chuỗi $s$:

- Nếu stack không rỗng, ký tự ở đỉnh stack lớn hơn $c$, và ký tự ở đỉnh sẽ còn xuất hiện trong chuỗi $s$, ta pop ký tự ở đỉnh và giảm số lần xuất hiện của nó trong $\textit{cnt}$.
- Đẩy ký tự $c$ vào stack.

Cuối cùng, nếu stack còn các ký tự trùng lặp, ta tiếp tục pop ký tự ở đỉnh cho đến khi số lần xuất hiện của ký tự ở đỉnh trong $\textit{cnt}$ chỉ còn là 1.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lexSmallestAfterDeletion(self, s: str) -> str:
        cnt = Counter(s)
        stk = []
        for c in s:
            while stk and stk[-1] > c and cnt[stk[-1]] > 1:
                cnt[stk.pop()] -= 1
            stk.append(c)
        while stk and cnt[stk[-1]] > 1:
            cnt[stk.pop()] -= 1
        return "".join(stk)
```

#### Java

```java
class Solution {
    public String lexSmallestAfterDeletion(String s) {
        int[] cnt = new int[26];
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            ++cnt[s.charAt(i) - 'a'];
        }
        StringBuilder stk = new StringBuilder();
        for (int i = 0; i < n; ++i) {
            char c = s.charAt(i);
            while (stk.length() > 0 && stk.charAt(stk.length() - 1) > c
                && cnt[stk.charAt(stk.length() - 1) - 'a'] > 1) {
                --cnt[stk.charAt(stk.length() - 1) - 'a'];
                stk.setLength(stk.length() - 1);
            }
            stk.append(c);
        }
        while (cnt[stk.charAt(stk.length() - 1) - 'a'] > 1) {
            --cnt[stk.charAt(stk.length() - 1) - 'a'];
            stk.setLength(stk.length() - 1);
        }
        return stk.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string lexSmallestAfterDeletion(string s) {
        int cnt[26]{};
        for (char c : s) {
            ++cnt[c - 'a'];
        }
        string stk;
        for (char c : s) {
            while (stk.size() && stk.back() > c && cnt[stk.back() - 'a'] > 1) {
                --cnt[stk.back() - 'a'];
                stk.pop_back();
            }
            stk.push_back(c);
        }
        while (cnt[stk.back() - 'a'] > 1) {
            --cnt[stk.back() - 'a'];
            stk.pop_back();
        }
        return stk;
    }
};
```

#### Go

```go
func lexSmallestAfterDeletion(s string) string {
	cnt := [26]int{}
	for _, c := range s {
		cnt[c-'a']++
	}
	stk := []byte{}
	for _, c := range s {
		for len(stk) > 0 && stk[len(stk)-1] > byte(c) && cnt[stk[len(stk)-1]-'a'] > 1 {
			cnt[stk[len(stk)-1]-'a']--
			stk = stk[:len(stk)-1]
		}
		stk = append(stk, byte(c))
	}
	for cnt[stk[len(stk)-1]-'a'] > 1 {
		cnt[stk[len(stk)-1]-'a']--
		stk = stk[:len(stk)-1]
	}
	return string(stk)
}
```

#### TypeScript

```ts
function lexSmallestAfterDeletion(s: string): string {
    const cnt: number[] = new Array(26).fill(0);
    const n = s.length;
    const a = 'a'.charCodeAt(0);
    for (let i = 0; i < n; ++i) {
        ++cnt[s.charCodeAt(i) - a];
    }
    const stk: string[] = [];
    for (let i = 0; i < n; ++i) {
        const c = s[i];
        while (
            stk.length > 0 &&
            stk[stk.length - 1] > c &&
            cnt[stk[stk.length - 1].charCodeAt(0) - a] > 1
        ) {
            --cnt[stk.pop()!.charCodeAt(0) - a];
        }
        stk.push(c);
    }
    while (cnt[stk[stk.length - 1].charCodeAt(0) - a] > 1) {
        --cnt[stk.pop()!.charCodeAt(0) - a];
    }
    return stk.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
