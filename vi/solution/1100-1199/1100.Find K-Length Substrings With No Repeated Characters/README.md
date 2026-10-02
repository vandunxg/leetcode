---
comments: true
difficulty: Medium
rating: 1348
source: Biweekly Contest 3 Q2
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [1100. Find K-Length Substrings With No Repeated Characters 🔒](https://leetcode.com/problems/find-k-length-substrings-with-no-repeated-characters)

[中文文档](/solution/1100-1199/1100.Find%20K-Length%20Substrings%20With%20No%20Repeated%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và số nguyên <code>k</code>, hãy trả về <em>số lượng chuỗi con trong </em><code>s</code><em> có độ dài </em><code>k</code><em> và không chứa ký tự lặp lại</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;havefunonleetcode&quot;, k = 5
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Có 6 chuỗi con: &#39;havef&#39;,&#39;avefu&#39;,&#39;vefun&#39;,&#39;efuno&#39;,&#39;etcod&#39;,&#39;tcode&#39;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;home&quot;, k = 5
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Lưu ý, k có thể lớn hơn độ dài của s. Khi đó không thể tìm được chuỗi con nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding window và hash table

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra từng chuỗi con độ dài $k$ có ký tự duy nhất sẽ tốn $O(nk)$. Với $n,k\le 10^4$, cách này vẫn có thể chạy được, nhưng hai cửa sổ liền nhau chỉ khác một ký tự đi vào và một ký tự đi ra.
>
> Duy trì cửa sổ độ dài $k$ cùng map tần suất: thêm $s[i]$, bỏ $s[i-k]$ và xóa key khi số lần xuất hiện về 0. Map có đúng $k$ key khi và chỉ khi mỗi ký tự trong cửa sổ xuất hiện một lần; khi đó tăng đáp án lên 1.

<!-- thinking:end -->

Ta duy trì sliding window có độ dài $k$ và dùng hash table $cnt$ để đếm số lần xuất hiện của mỗi ký tự trong cửa sổ.

Trước tiên, thêm $k$ ký tự đầu tiên của chuỗi $s$ vào hash table $cnt$, rồi kiểm tra kích thước của $cnt$ có bằng $k$ hay không. Nếu bằng, mọi ký tự trong cửa sổ đều khác nhau, nên tăng đáp án $ans$ lên 1.

Tiếp theo, bắt đầu duyệt chuỗi $s$ từ chỉ số $k$. Ở mỗi bước, thêm $s[i]$ vào hash table $cnt$ đồng thời giảm số lần xuất hiện của $s[i-k]$ đi 1. Nếu sau khi giảm, $cnt[s[i-k]]$ bằng $0$, xóa $s[i-k]$ khỏi hash table $cnt$. Nếu lúc này kích thước hash table $cnt$ bằng $k$, mọi ký tự trong cửa sổ đều khác nhau, nên tăng đáp án $ans$ lên 1.

Cuối cùng, trả về đáp án $ans$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(\min(k, |\Sigma|))$, trong đó $n$ là độ dài chuỗi $s$, còn $\Sigma$ là tập ký tự. Trong bài này, tập ký tự gồm các chữ cái tiếng Anh viết thường nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numKLenSubstrNoRepeats(self, s: str, k: int) -> int:
        cnt = Counter(s[:k])
        ans = int(len(cnt) == k)
        for i in range(k, len(s)):
            cnt[s[i]] += 1
            cnt[s[i - k]] -= 1
            if cnt[s[i - k]] == 0:
                cnt.pop(s[i - k])
            ans += int(len(cnt) == k)
        return ans
```

#### Java

```java
class Solution {
    public int numKLenSubstrNoRepeats(String s, int k) {
        int n = s.length();
        if (n < k) {
            return 0;
        }
        Map<Character, Integer> cnt = new HashMap<>(k);
        for (int i = 0; i < k; ++i) {
            cnt.merge(s.charAt(i), 1, Integer::sum);
        }
        int ans = cnt.size() == k ? 1 : 0;
        for (int i = k; i < n; ++i) {
            cnt.merge(s.charAt(i), 1, Integer::sum);
            if (cnt.merge(s.charAt(i - k), -1, Integer::sum) == 0) {
                cnt.remove(s.charAt(i - k));
            }
            ans += cnt.size() == k ? 1 : 0;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numKLenSubstrNoRepeats(string s, int k) {
        int n = s.size();
        if (n < k) {
            return 0;
        }
        unordered_map<char, int> cnt;
        for (int i = 0; i < k; ++i) {
            ++cnt[s[i]];
        }
        int ans = cnt.size() == k;
        for (int i = k; i < n; ++i) {
            ++cnt[s[i]];
            if (--cnt[s[i - k]] == 0) {
                cnt.erase(s[i - k]);
            }
            ans += cnt.size() == k;
        }
        return ans;
    }
};
```

#### Go

```go
func numKLenSubstrNoRepeats(s string, k int) (ans int) {
	n := len(s)
	if n < k {
		return
	}
	cnt := map[byte]int{}
	for i := 0; i < k; i++ {
		cnt[s[i]]++
	}
	if len(cnt) == k {
		ans++
	}
	for i := k; i < n; i++ {
		cnt[s[i]]++
		cnt[s[i-k]]--
		if cnt[s[i-k]] == 0 {
			delete(cnt, s[i-k])
		}
		if len(cnt) == k {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function numKLenSubstrNoRepeats(s: string, k: number): number {
    const n = s.length;
    if (n < k) {
        return 0;
    }
    const cnt: Map<string, number> = new Map();
    for (let i = 0; i < k; ++i) {
        cnt.set(s[i], (cnt.get(s[i]) ?? 0) + 1);
    }
    let ans = cnt.size === k ? 1 : 0;
    for (let i = k; i < n; ++i) {
        cnt.set(s[i], (cnt.get(s[i]) ?? 0) + 1);
        cnt.set(s[i - k], (cnt.get(s[i - k]) ?? 0) - 1);
        if (cnt.get(s[i - k]) === 0) {
            cnt.delete(s[i - k]);
        }
        ans += cnt.size === k ? 1 : 0;
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
    function numKLenSubstrNoRepeats($s, $k) {
        $n = strlen($s);
        if ($n < $k) {
            return 0;
        }
        $cnt = [];
        for ($i = 0; $i < $k; ++$i) {
            if (!isset($cnt[$s[$i]])) {
                $cnt[$s[$i]] = 1;
            } else {
                $cnt[$s[$i]]++;
            }
        }
        $ans = count($cnt) == $k ? 1 : 0;
        for ($i = $k; $i < $n; ++$i) {
            if (!isset($cnt[$s[$i]])) {
                $cnt[$s[$i]] = 1;
            } else {
                $cnt[$s[$i]]++;
            }
            if ($cnt[$s[$i - $k]] - 1 == 0) {
                unset($cnt[$s[$i - $k]]);
            } else {
                $cnt[$s[$i - $k]]--;
            }
            $ans += count($cnt) == $k ? 1 : 0;
        }
        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
