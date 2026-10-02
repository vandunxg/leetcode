---
comments: true
difficulty: Medium
rating: 1877
source: Weekly Contest 159 Q3
tags:
    - String
    - Sliding Window
---

<!-- problem:start -->

# [1234. Replace the Substring for Balanced String](https://leetcode.com/problems/replace-the-substring-for-balanced-string)

[中文文档](/solution/1200-1299/1234.Replace%20the%20Substring%20for%20Balanced%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi s có độ dài <code>n</code>, chỉ gồm bốn loại ký tự: <code>&#39;Q&#39;</code>, <code>&#39;W&#39;</code>, <code>&#39;E&#39;</code> và <code>&#39;R&#39;</code>.</p>

<p>Một chuỗi được gọi là <strong>cân bằng</strong><em> </em>nếu mỗi ký tự xuất hiện <code>n / 4</code> lần, trong đó <code>n</code> là độ dài chuỗi.</p>

<p>Trả về <em>độ dài nhỏ nhất của chuỗi con có thể được thay bằng một chuỗi khác <strong>bất kỳ</strong> có cùng độ dài để khiến </em><code>s</code><em> trở nên <strong>cân bằng</strong></em>. Nếu s đã <strong>cân bằng</strong>, trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;QWER&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> s đã cân bằng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;QQWE&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta cần thay một &#39;Q&#39; bằng &#39;R&#39; để &quot;RQWE&quot; (hoặc &quot;QRWE&quot;) trở nên cân bằng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;QQQW&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể thay hai ký tự &quot;QQ&quot; đầu tiên bằng &quot;ER&quot;. 
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == s.length</code></li>
	<li><code>4 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>n</code> là bội số của <code>4</code>.</li>
	<li><code>s</code> chỉ chứa <code>&#39;Q&#39;</code>, <code>&#39;W&#39;</code>, <code>&#39;E&#39;</code> và <code>&#39;R&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi cân bằng khi mỗi trong bốn ký tự xuất hiện $n/4$ lần. Vì $n \le 10^5$, không thể thử mọi chuỗi con cần thay. Phần nằm ngoài cửa sổ thỏa điều kiện khi cửa sổ chứa đủ các ký tự đang dư; cửa sổ càng dài thì càng dễ thỏa điều kiện, nên tính khả thi có tính đơn điệu.
>
> Ta đếm ký tự trong cả chuỗi; nếu chuỗi đã cân bằng thì đáp án là $0$. Nếu chưa, đầu phải lần lượt đưa ký tự vào cửa sổ (giảm số lượng ký tự ở ngoài cửa sổ). Khi phần ngoài cửa sổ vẫn thỏa điều kiện, ta thu hẹp đầu trái và ghi nhận cửa sổ ngắn nhất. Hai con trỏ duy trì đoạn ngắn nhất bao phủ đủ các ký tự dư để phần bên ngoài cân bằng.

<!-- thinking:end -->

Trước tiên, dùng hash table hoặc mảng `cnt` để đếm số lần xuất hiện của từng ký tự trong chuỗi $s$. Nếu số lượng của mọi ký tự đều không vượt quá $n/4$, thì chuỗi $s$ đã cân bằng và ta trả về ngay $0$.

Nếu không, dùng hai con trỏ $j$ và $i$ để xác định biên trái và biên phải của cửa sổ; ban đầu $j = 0$.

Tiếp theo, duyệt chuỗi $s$ từ trái sang phải. Mỗi khi gặp một ký tự, giảm số lượng ký tự đó đi $1$, rồi kiểm tra cửa sổ hiện tại có thỏa điều kiện hay không; tức là số lượng từng ký tự nằm ngoài cửa sổ không vượt quá $n/4$. Nếu điều kiện được thỏa, cập nhật đáp án rồi dịch biên trái sang phải cho đến khi điều kiện không còn đúng.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(C)$, trong đó $n$ là độ dài chuỗi $s$, còn $C$ là kích thước bảng chữ cái; trong bài này, $C = 4$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def balancedString(self, s: str) -> int:
        cnt = Counter(s)
        n = len(s)
        if all(v <= n // 4 for v in cnt.values()):
            return 0
        ans, j = n, 0
        for i, c in enumerate(s):
            cnt[c] -= 1
            while j <= i and all(v <= n // 4 for v in cnt.values()):
                ans = min(ans, i - j + 1)
                cnt[s[j]] += 1
                j += 1
        return ans
```

#### Java

```java
class Solution {
    public int balancedString(String s) {
        int[] cnt = new int[4];
        String t = "QWER";
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            cnt[t.indexOf(s.charAt(i))]++;
        }
        int m = n / 4;
        if (cnt[0] == m && cnt[1] == m && cnt[2] == m && cnt[3] == m) {
            return 0;
        }
        int ans = n;
        for (int i = 0, j = 0; i < n; ++i) {
            cnt[t.indexOf(s.charAt(i))]--;
            while (j <= i && cnt[0] <= m && cnt[1] <= m && cnt[2] <= m && cnt[3] <= m) {
                ans = Math.min(ans, i - j + 1);
                cnt[t.indexOf(s.charAt(j++))]++;
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
    int balancedString(string s) {
        int cnt[4]{};
        string t = "QWER";
        int n = s.size();
        for (char& c : s) {
            cnt[t.find(c)]++;
        }
        int m = n / 4;
        if (cnt[0] == m && cnt[1] == m && cnt[2] == m && cnt[3] == m) {
            return 0;
        }
        int ans = n;
        for (int i = 0, j = 0; i < n; ++i) {
            cnt[t.find(s[i])]--;
            while (j <= i && cnt[0] <= m && cnt[1] <= m && cnt[2] <= m && cnt[3] <= m) {
                ans = min(ans, i - j + 1);
                cnt[t.find(s[j++])]++;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func balancedString(s string) int {
	cnt := [4]int{}
	t := "QWER"
	n := len(s)
	for i := range s {
		cnt[strings.IndexByte(t, s[i])]++
	}
	m := n / 4
	if cnt[0] == m && cnt[1] == m && cnt[2] == m && cnt[3] == m {
		return 0
	}
	ans := n
	for i, j := 0, 0; i < n; i++ {
		cnt[strings.IndexByte(t, s[i])]--
		for j <= i && cnt[0] <= m && cnt[1] <= m && cnt[2] <= m && cnt[3] <= m {
			ans = min(ans, i-j+1)
			cnt[strings.IndexByte(t, s[j])]++
			j++
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
