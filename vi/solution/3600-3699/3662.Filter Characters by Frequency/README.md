---
comments: true
difficulty: Easy
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [3662. Filter Characters by Frequency 🔒](https://leetcode.com/problems/filter-characters-by-frequency)

[中文文档](/solution/3600-3699/3662.Filter%20Characters%20by%20Frequency/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và một số nguyên <code>k</code>.</p>

<p>Nhiệm vụ của bạn là tạo một chuỗi mới chỉ chứa những ký tự trong <code>s</code> xuất hiện <strong>ít hơn</strong> <code>k</code> lần trong toàn bộ chuỗi. Thứ tự các ký tự trong chuỗi mới phải <strong>giống</strong> với <strong>thứ tự</strong> của chúng trong <code>s</code>.</p>

<p>Trả về chuỗi thu được. Nếu không có ký tự nào thỏa mãn, trả về một chuỗi rỗng.</p>

<p>Lưu ý: Giữ lại <strong>mọi lần xuất hiện</strong> của một ký tự xuất hiện ít hơn <code>k</code> lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aadbbcccca&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;dbb&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tần suất của các ký tự trong <code>s</code>:</p>

<ul>
	<li><code>&#39;a&#39;</code> xuất hiện 3 lần</li>
	<li><code>&#39;d&#39;</code> xuất hiện 1 lần</li>
	<li><code>&#39;b&#39;</code> xuất hiện 2 lần</li>
	<li><code>&#39;c&#39;</code> xuất hiện 4 lần</li>
</ul>

<p>Chỉ <code>&#39;d&#39;</code> và <code>&#39;b&#39;</code> xuất hiện ít hơn 3 lần. Giữ nguyên thứ tự của chúng, kết quả là <code>&quot;dbb&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;xyz&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;xyz&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả các ký tự (<code>&#39;x&#39;</code>, <code>&#39;y&#39;</code>, <code>&#39;z&#39;</code>) đều xuất hiện đúng một lần, ít hơn 2. Vì vậy, toàn bộ chuỗi được trả về.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= k &lt;= s.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Giữ lại các ký tự có tần suất xuất hiện trong toàn bộ chuỗi nhỏ hơn $k$, theo đúng thứ tự ban đầu. Trước tiên đếm tần suất, sau đó lọc, để việc xóa ký tự không làm thay đổi tần suất trong quá trình duyệt.
>
> Dùng một $\textit{Counter}$ để lưu tổng số lần xuất hiện; ở lượt duyệt thứ hai, thêm các ký tự có số lần xuất hiện nhỏ hơn $k$.
>
> Vì $n\le 100$, hai lượt duyệt tuyến tính là đủ.

<!-- thinking:end -->

Trước tiên, ta duyệt qua chuỗi $s$ và đếm tần suất của từng ký tự, lưu kết quả trong một hash table hoặc mảng $\textit{cnt}$.

Sau đó, ta duyệt lại chuỗi $s$, thêm vào chuỗi kết quả những ký tự có tần suất nhỏ hơn $k$. Cuối cùng, trả về chuỗi kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là kích thước của tập ký tự.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def filterCharacters(self, s: str, k: int) -> str:
        cnt = Counter(s)
        ans = []
        for c in s:
            if cnt[c] < k:
                ans.append(c)
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String filterCharacters(String s, int k) {
        int[] cnt = new int[26];
        for (char c : s.toCharArray()) {
            ++cnt[c - 'a'];
        }
        StringBuilder ans = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (cnt[c - 'a'] < k) {
                ans.append(c);
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string filterCharacters(string s, int k) {
        int cnt[26]{};
        for (char c : s) {
            ++cnt[c - 'a'];
        }
        string ans;
        for (char c : s) {
            if (cnt[c - 'a'] < k) {
                ans.push_back(c);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func filterCharacters(s string, k int) string {
	cnt := [26]int{}
	for _, c := range s {
		cnt[c-'a']++
	}
	ans := []rune{}
	for _, c := range s {
		if cnt[c-'a'] < k {
			ans = append(ans, c)
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function filterCharacters(s: string, k: number): string {
    const cnt: Record<string, number> = {};
    for (const c of s) {
        cnt[c] = (cnt[c] || 0) + 1;
    }
    const ans: string[] = [];
    for (const c of s) {
        if (cnt[c] < k) {
            ans.push(c);
        }
    }
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
