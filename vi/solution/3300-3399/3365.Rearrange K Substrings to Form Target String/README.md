---
comments: true
difficulty: Medium
rating: 1513
source: Weekly Contest 425 Q2
tags:
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [3365. Rearrange K Substrings to Form Target String](https://leetcode.com/problems/rearrange-k-substrings-to-form-target-string)

[中文文档](/solution/3300-3399/3365.Rearrange%20K%20Substrings%20to%20Form%20Target%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi <code>s</code> và <code>t</code>, cả hai đều là anagram của nhau, cùng với một số nguyên <code>k</code>.</p>

<p>Nhiệm vụ của bạn là xác định xem có thể chia chuỗi <code>s</code> thành <code>k</code> chuỗi con có cùng độ dài, sắp xếp lại các chuỗi con đó và nối chúng theo <em>bất kỳ thứ tự nào</em> để tạo thành một chuỗi mới giống với chuỗi <code>t</code> hay không.</p>

<p>Trả về <code>true</code> nếu có thể, ngược lại trả về <code>false</code>.</p>

<p><strong>Anagram</strong> là một từ hoặc cụm từ được tạo thành bằng cách sắp xếp lại các chữ cái của một từ hoặc cụm từ khác, sử dụng đúng một lần tất cả các chữ cái ban đầu.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp <b>không rỗng</b> trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcd&quot;, t = &quot;cdab&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chia <code>s</code> thành 2 chuỗi con có độ dài 2: <code>[&quot;ab&quot;, &quot;cd&quot;]</code>.</li>
	<li>Sắp xếp lại các chuỗi con này thành <code>[&quot;cd&quot;, &quot;ab&quot;]</code>, sau đó nối chúng lại sẽ thu được <code>&quot;cdab&quot;</code>, trùng với <code>t</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aabbcc&quot;, t = &quot;bbaacc&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chia <code>s</code> thành 3 chuỗi con có độ dài 2: <code>[&quot;aa&quot;, &quot;bb&quot;, &quot;cc&quot;]</code>.</li>
	<li>Sắp xếp lại các chuỗi con này thành <code>[&quot;bb&quot;, &quot;aa&quot;, &quot;cc&quot;]</code>, sau đó nối chúng lại sẽ thu được <code>&quot;bbaacc&quot;</code>, trùng với <code>t</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aabbcc&quot;, t = &quot;bbaacc&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chia <code>s</code> thành 2 chuỗi con có độ dài 3: <code>[&quot;aab&quot;, &quot;bcc&quot;]</code>.</li>
	<li>Không thể sắp xếp lại các chuỗi con này để tạo thành <code>t = &quot;bbaacc&quot;</code>, vì vậy kết quả là <code>false</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length == t.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= s.length</code></li>
	<li><code>s.length</code> chia hết cho <code>k</code>.</li>
	<li><code>s</code> và <code>t</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Dữ liệu đầu vào được tạo sao cho<!-- notionvc: 53e485fc-71ce-4032-aed1-f712dd3822ba --> <code>s</code> và <code>t</code> là anagram của nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Chia $s$ và $t$ thành $k$ block có cùng độ dài, rồi kiểm tra xem hai multiset có giống nhau hay không. Với $n \le 2 \times 10^5$, chỉ cần dùng một bộ đếm.
>
> Tăng bộ đếm với mỗi block của $s$ và giảm bộ đếm với mỗi block của $t$; tất cả các giá trị đếm phải bằng $0$ khi kết thúc.
>
> Đề bài đã đảm bảo $s$ và $t$ là anagram, nên chỉ cần quan tâm đến multiset các block.

<!-- thinking:end -->

Gọi độ dài của chuỗi $s$ là $n$, khi đó độ dài của mỗi chuỗi con là $m = n / k$.

Ta sử dụng một bảng băm $\textit{cnt}$ để ghi lại hiệu giữa số lần xuất hiện của mỗi chuỗi con có độ dài $m$ trong chuỗi $s$ và trong chuỗi $t$.

Ta duyệt chuỗi $s$, mỗi lần lấy ra một chuỗi con có độ dài $m$, rồi cập nhật bảng băm $\textit{cnt}$.

Cuối cùng, ta kiểm tra xem tất cả các giá trị trong bảng băm $\textit{cnt}$ có bằng $0$ hay không.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPossibleToRearrange(self, s: str, t: str, k: int) -> bool:
        cnt = Counter()
        n = len(s)
        m = n // k
        for i in range(0, n, m):
            cnt[s[i : i + m]] += 1
            cnt[t[i : i + m]] -= 1
        return all(v == 0 for v in cnt.values())
```

#### Java

```java
class Solution {
    public boolean isPossibleToRearrange(String s, String t, int k) {
        Map<String, Integer> cnt = new HashMap<>(k);
        int n = s.length();
        int m = n / k;
        for (int i = 0; i < n; i += m) {
            cnt.merge(s.substring(i, i + m), 1, Integer::sum);
            cnt.merge(t.substring(i, i + m), -1, Integer::sum);
        }
        for (int v : cnt.values()) {
            if (v != 0) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isPossibleToRearrange(string s, string t, int k) {
        unordered_map<string, int> cnt;
        int n = s.size();
        int m = n / k;
        for (int i = 0; i < n; i += m) {
            cnt[s.substr(i, m)]++;
            cnt[t.substr(i, m)]--;
        }
        for (auto& [_, v] : cnt) {
            if (v) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func isPossibleToRearrange(s string, t string, k int) bool {
	n := len(s)
	m := n / k
	cnt := map[string]int{}
	for i := 0; i < n; i += m {
		cnt[s[i:i+m]]++
		cnt[t[i:i+m]]--
	}
	for _, v := range cnt {
		if v != 0 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function isPossibleToRearrange(s: string, t: string, k: number): boolean {
    const cnt: Record<string, number> = {};
    const n = s.length;
    const m = Math.floor(n / k);
    for (let i = 0; i < n; i += m) {
        const a = s.slice(i, i + m);
        cnt[a] = (cnt[a] || 0) + 1;
        const b = t.slice(i, i + m);
        cnt[b] = (cnt[b] || 0) - 1;
    }
    return Object.values(cnt).every(x => x === 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
