---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [340. Longest Substring with At Most K Distinct Characters 🔒](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters)

[中文文档](/solution/0300-0399/0340.Longest%20Substring%20with%20At%20Most%20K%20Distinct%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và số nguyên <code>k</code>, hãy trả về <em>độ dài của </em><span data-keyword="substring-nonempty"><em>chuỗi con</em></span><em> dài nhất của</em> <code>s</code> <em>chứa không quá</em> <code>k</code> <em>ký tự <strong>phân biệt</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;eceba&quot;, k = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Chuỗi con là &quot;ece&quot; và có độ dài 3.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aa&quot;, k = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Chuỗi con là &quot;aa&quot; và có độ dài 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= k &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding window + Hash table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm chuỗi con dài nhất có không quá $k$ ký tự phân biệt. Xét mọi cặp điểm đầu-cuối tốn $O(n^2)$. Khi mở rộng đầu phải, số ký tự phân biệt chỉ có thể tăng; khi dịch đầu trái, số này có thể giảm, nên có thể dùng sliding window.
>
> Thêm ký tự ở đầu phải vào cửa sổ; khi map có hơn $k$ key, bỏ ký tự ở đầu trái. Cửa sổ luôn là hậu tố hợp lệ dài nhất của prefix đã duyệt, và độ dài của nó là $n-l$.

<!-- thinking:end -->

Ta dùng sliding window và hash table $\textit{cnt}$ để lưu số lần xuất hiện của mỗi ký tự trong cửa sổ; $\textit{l}$ là ranh giới trái của cửa sổ.

Duyệt chuỗi và mỗi lần thêm ký tự ở ranh giới phải vào hash table. Nếu số ký tự phân biệt trong hash table vượt quá $k$, giảm số lần xuất hiện của ký tự ở ranh giới trái, xóa ký tự đó khỏi hash table nếu số lần xuất hiện bằng $0$, rồi dịch ranh giới trái $\textit{l}$.

Cuối cùng, trả về độ dài chuỗi trừ đi vị trí ranh giới trái.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(k)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lengthOfLongestSubstringKDistinct(self, s: str, k: int) -> int:
        l = 0
        cnt = Counter()
        for c in s:
            cnt[c] += 1
            if len(cnt) > k:
                cnt[s[l]] -= 1
                if cnt[s[l]] == 0:
                    del cnt[s[l]]
                l += 1
        return len(s) - l
```

#### Java

```java
class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        Map<Character, Integer> cnt = new HashMap<>();
        int l = 0;
        char[] cs = s.toCharArray();
        for (char c : cs) {
            cnt.merge(c, 1, Integer::sum);
            if (cnt.size() > k) {
                if (cnt.merge(cs[l], -1, Integer::sum) == 0) {
                    cnt.remove(cs[l]);
                }
                ++l;
            }
        }
        return cs.length - l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int lengthOfLongestSubstringKDistinct(string s, int k) {
        unordered_map<char, int> cnt;
        int l = 0;
        for (char& c : s) {
            ++cnt[c];
            if (cnt.size() > k) {
                if (--cnt[s[l]] == 0) {
                    cnt.erase(s[l]);
                }
                ++l;
            }
        }
        return s.size() - l;
    }
};
```

#### Go

```go
func lengthOfLongestSubstringKDistinct(s string, k int) int {
	cnt := map[byte]int{}
	l := 0
	for _, c := range s {
		cnt[byte(c)]++
		if len(cnt) > k {
			cnt[s[l]]--
			if cnt[s[l]] == 0 {
				delete(cnt, s[l])
			}
			l++
		}
	}
	return len(s) - l
}
```

#### TypeScript

```ts
function lengthOfLongestSubstringKDistinct(s: string, k: number): number {
    const cnt: Map<string, number> = new Map();
    let l = 0;
    for (const c of s) {
        cnt.set(c, (cnt.get(c) ?? 0) + 1);
        if (cnt.size > k) {
            cnt.set(s[l], cnt.get(s[l])! - 1);
            if (cnt.get(s[l]) === 0) {
                cnt.delete(s[l]);
            }
            l++;
        }
    }
    return s.length - l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
