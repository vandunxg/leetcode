---
comments: true
difficulty: Medium
rating: 1423
source: Biweekly Contest 124 Q2
tags:
    - Array
    - Hash Table
    - Counting
    - Sorting
---

<!-- problem:start -->

# [3039. Apply Operations to Make String Empty](https://leetcode.com/problems/apply-operations-to-make-string-empty)

[中文文档](/solution/3000-3099/3039.Apply%20Operations%20to%20Make%20String%20Empty/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code>.</p>

<p>Hãy thực hiện thao tác sau cho đến khi <code>s</code> trở thành <strong>rỗng</strong>:</p>

<ul>
	<li>Với <strong>mọi</strong> ký tự trong bảng chữ cái từ <code>&#39;a&#39;</code> đến <code>&#39;z&#39;</code>, xóa lần xuất hiện <strong>đầu tiên</strong> của ký tự đó trong <code>s</code> (nếu tồn tại).</li>
</ul>

<p>Ví dụ, ban đầu <code>s = &quot;aabcbbca&quot;</code>. Ta thực hiện các thao tác sau:</p>

<ul>
	<li>Xóa các ký tự được gạch chân <code>s = &quot;<u><strong>a</strong></u>a<strong><u>bc</u></strong>bbca&quot;</code>. Chuỗi kết quả là <code>s = &quot;abbca&quot;</code>.</li>
	<li>Xóa các ký tự được gạch chân <code>s = &quot;<u><strong>ab</strong></u>b<u><strong>c</strong></u>a&quot;</code>. Chuỗi kết quả là <code>s = &quot;ba&quot;</code>.</li>
	<li>Xóa các ký tự được gạch chân <code>s = &quot;<u><strong>ba</strong></u>&quot;</code>. Chuỗi kết quả là <code>s = &quot;&quot;</code>.</li>
</ul>

<p>Trả về <em>giá trị của chuỗi </em><code>s</code><em> ngay <strong>trước</strong> khi thực hiện thao tác <strong>cuối cùng</strong></em>. Trong ví dụ trên, đáp án là <code>&quot;ba&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aabcbbca&quot;
<strong>Đầu ra:</strong> &quot;ba&quot;
<strong>Giải thích:</strong> Được giải thích trong phần mô tả.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcd&quot;
<strong>Đầu ra:</strong> &quot;abcd&quot;
<strong>Giải thích:</strong> Ta thực hiện thao tác sau:
- Xóa các ký tự được gạch chân s = &quot;<u><strong>abcd</strong></u>&quot;. Chuỗi kết quả là s = &quot;&quot;.
Chuỗi ngay trước thao tác cuối cùng là &quot;abcd&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm hoặc mảng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi vòng xóa lần xuất hiện còn lại đầu tiên của từng ký tự. Ta cần tìm các ký tự bị xóa trong vòng cuối cùng, theo thứ tự ban đầu của chúng. $n \le 5 \times 10^5$.
>
> Vòng cuối cùng xóa chính xác những ký tự có số lần xuất hiện bằng tần suất lớn nhất trong chuỗi, tại lần xuất hiện cuối cùng của chúng.
>
> Sau khi đếm số lần xuất hiện và vị trí cuối cùng, ta giữ lại những ký tự đạt tần suất lớn nhất và đang ở chỉ số cuối cùng của chúng.

<!-- thinking:end -->

Ta sử dụng một bảng băm hoặc mảng $cnt$ để ghi lại số lần xuất hiện của mỗi ký tự trong chuỗi $s$, đồng thời sử dụng một bảng băm hoặc mảng $last$ khác để ghi lại vị trí xuất hiện cuối cùng của mỗi ký tự trong chuỗi $s$. Số lần xuất hiện lớn nhất của các ký tự trong chuỗi $s$ được ký hiệu là $mx$.

Sau đó, ta duyệt chuỗi $s$. Nếu số lần xuất hiện của ký tự hiện tại bằng $mx$ và vị trí của ký tự hiện tại bằng vị trí xuất hiện cuối cùng của ký tự đó, ta thêm ký tự hiện tại vào đáp án.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài chuỗi $s$, còn $\Sigma$ là tập ký tự. Trong bài này, $\Sigma$ là tập các chữ cái tiếng Anh viết thường.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lastNonEmptyString(self, s: str) -> str:
        cnt = Counter(s)
        mx = cnt.most_common(1)[0][1]
        last = {c: i for i, c in enumerate(s)}
        return "".join(c for i, c in enumerate(s) if cnt[c] == mx and last[c] == i)
```

#### Java

```java
class Solution {
    public String lastNonEmptyString(String s) {
        int[] cnt = new int[26];
        int[] last = new int[26];
        int n = s.length();
        int mx = 0;
        for (int i = 0; i < n; ++i) {
            int c = s.charAt(i) - 'a';
            mx = Math.max(mx, ++cnt[c]);
            last[c] = i;
        }
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < n; ++i) {
            int c = s.charAt(i) - 'a';
            if (cnt[c] == mx && last[c] == i) {
                ans.append(s.charAt(i));
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
    string lastNonEmptyString(string s) {
        int cnt[26]{};
        int last[26]{};
        int n = s.size();
        int mx = 0;
        for (int i = 0; i < n; ++i) {
            int c = s[i] - 'a';
            mx = max(mx, ++cnt[c]);
            last[c] = i;
        }
        string ans;
        for (int i = 0; i < n; ++i) {
            int c = s[i] - 'a';
            if (cnt[c] == mx && last[c] == i) {
                ans.push_back(s[i]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func lastNonEmptyString(s string) string {
	cnt := [26]int{}
	last := [26]int{}
	mx := 0
	for i, c := range s {
		c -= 'a'
		cnt[c]++
		last[c] = i
		mx = max(mx, cnt[c])
	}
	ans := []rune{}
	for i, c := range s {
		if cnt[c-'a'] == mx && last[c-'a'] == i {
			ans = append(ans, c)
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function lastNonEmptyString(s: string): string {
    const cnt: number[] = Array(26).fill(0);
    const last: number[] = Array(26).fill(0);
    const n = s.length;
    let mx = 0;
    for (let i = 0; i < n; ++i) {
        const c = s.charCodeAt(i) - 97;
        mx = Math.max(mx, ++cnt[c]);
        last[c] = i;
    }
    const ans: string[] = [];
    for (let i = 0; i < n; ++i) {
        const c = s.charCodeAt(i) - 97;
        if (cnt[c] === mx && last[c] === i) {
            ans.push(String.fromCharCode(c + 97));
        }
    }
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
