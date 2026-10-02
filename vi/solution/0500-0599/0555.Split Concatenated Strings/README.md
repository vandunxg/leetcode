---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - String
---

<!-- problem:start -->

# [555. Split Concatenated Strings 🔒](https://leetcode.com/problems/split-concatenated-strings)

[中文文档](/solution/0500-0599/0555.Split%20Concatenated%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>strs</code>. Bạn có thể nối các chuỗi này thành một vòng, trong đó mỗi chuỗi có thể được đảo ngược hoặc giữ nguyên. Trong tất cả các vòng có thể tạo ra</p>

<p>Hãy trả về <em>chuỗi có thứ tự từ điển lớn nhất sau khi cắt vòng để biến chuỗi dạng vòng thành chuỗi thông thường</em>.</p>

<p>Cụ thể, để tìm chuỗi lớn nhất theo thứ tự từ điển, ta thực hiện hai bước:</p>

<ol>
	<li>Nối tất cả chuỗi thành một vòng; có thể đảo ngược tùy ý một số chuỗi, nhưng phải nối chúng theo thứ tự ban đầu.</li>
	<li>Cắt vòng tại một vị trí bất kỳ để tạo thành chuỗi thông thường bắt đầu từ ký tự tại điểm cắt.</li>
</ol>

<p>Nhiệm vụ của bạn là tìm chuỗi thông thường lớn nhất theo thứ tự từ điển trong tất cả các khả năng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;abc&quot;,&quot;xyz&quot;]
<strong>Đầu ra:</strong> &quot;zyxcba&quot;
<strong>Giải thích:</strong> Có thể tạo ra các chuỗi dạng vòng &quot;-abcxyz-&quot;, &quot;-abczyx-&quot;, &quot;-cbaxyz-&quot;, &quot;-cbazyx-&quot;, trong đó &#39;-&#39; biểu thị trạng thái vòng. 
Chuỗi đáp án được tạo từ vòng thứ tư: cắt tại ký tự &#39;a&#39; ở giữa sẽ thu được &quot;zyxcba&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;abc&quot;]
<strong>Đầu ra:</strong> &quot;cba&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= strs.length &lt;= 1000</code></li>
	<li><code>1 &lt;= strs[i].length &lt;= 1000</code></li>
	<li><code>1 &lt;= sum(strs[i].length) &lt;= 1000</code></li>
	<li><code>strs[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Có thể đảo ngược từng chuỗi rồi cắt chuỗi ghép tại một chỉ số bất kỳ. Thử mọi tổ hợp đảo ngược và mọi vị trí cắt sẽ tốn quá nhiều thời gian.
>
> Thay mỗi chuỗi bằng chuỗi lớn hơn theo thứ tự từ điển giữa nó và phiên bản đảo ngược, để phần còn lại của vòng đã được tối ưu. Sau đó, lần lượt chọn mỗi chuỗi làm vị trí cắt, thử cả hai hướng của chuỗi đó và giữ lại kết quả lớn nhất.

<!-- thinking:end -->

Trước tiên, duyệt mảng chuỗi `strs`. Với mỗi chuỗi $s$, nếu chuỗi đảo ngược $t$ lớn hơn $s$ theo thứ tự từ điển thì thay $s$ bằng $t$.

Tiếp theo, lần lượt chọn mỗi vị trí $i$ trong mảng `strs` làm điểm cắt, chia mảng `strs` thành hai phần: $strs[i + 1:]$ và $strs[:i]$. Nối hai phần này thành chuỗi $t$. Sau đó, duyệt từng vị trí $j$ trong chuỗi hiện tại $strs[i]$. Gọi hậu tố là $a = strs[i][j:]$ và tiền tố là $b = strs[i][:j]$. Nối $a$, $t$ và $b$ để tạo chuỗi $cur$. Nếu $cur$ lớn hơn đáp án hiện tại thì cập nhật đáp án. Trường hợp này tương ứng với việc đảo ngược $strs[i]$. Ta cũng cần xét trường hợp không đảo ngược $strs[i]$, tức là nối $a$, $t$ và $b$ theo thứ tự ngược lại để tạo chuỗi $cur$. Nếu $cur$ lớn hơn đáp án hiện tại thì cập nhật đáp án.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số chuỗi trong `strs`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def splitLoopedString(self, strs: List[str]) -> str:
        strs = [s[::-1] if s[::-1] > s else s for s in strs]
        ans = ''.join(strs)
        for i, s in enumerate(strs):
            t = ''.join(strs[i + 1 :]) + ''.join(strs[:i])
            for j in range(len(s)):
                a = s[j:]
                b = s[:j]
                ans = max(ans, a + t + b)
                ans = max(ans, b[::-1] + t + a[::-1])
        return ans
```

#### Java

```java
class Solution {
    public String splitLoopedString(String[] strs) {
        int n = strs.length;
        for (int i = 0; i < n; ++i) {
            String s = strs[i];
            String t = new StringBuilder(s).reverse().toString();
            if (s.compareTo(t) < 0) {
                strs[i] = t;
            }
        }
        String ans = "";
        for (int i = 0; i < n; ++i) {
            String s = strs[i];
            StringBuilder sb = new StringBuilder();
            for (int j = i + 1; j < n; ++j) {
                sb.append(strs[j]);
            }
            for (int j = 0; j < i; ++j) {
                sb.append(strs[j]);
            }
            String t = sb.toString();
            for (int j = 0; j < s.length(); ++j) {
                String a = s.substring(j);
                String b = s.substring(0, j);
                String cur = a + t + b;
                if (ans.compareTo(cur) < 0) {
                    ans = cur;
                }
                cur = new StringBuilder(b)
                          .reverse()
                          .append(t)
                          .append(new StringBuilder(a).reverse().toString())
                          .toString();
                if (ans.compareTo(cur) < 0) {
                    ans = cur;
                }
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
    string splitLoopedString(vector<string>& strs) {
        for (auto& s : strs) {
            string t{s.rbegin(), s.rend()};
            s = max(s, t);
        }
        int n = strs.size();
        string ans = "";
        for (int i = 0; i < strs.size(); ++i) {
            auto& s = strs[i];
            string t;
            for (int j = i + 1; j < n; ++j) {
                t += strs[j];
            }
            for (int j = 0; j < i; ++j) {
                t += strs[j];
            }
            for (int j = 0; j < s.size(); ++j) {
                auto a = s.substr(j);
                auto b = s.substr(0, j);
                auto cur = a + t + b;
                if (ans < cur) {
                    ans = cur;
                }
                reverse(a.begin(), a.end());
                reverse(b.begin(), b.end());
                cur = b + t + a;
                if (ans < cur) {
                    ans = cur;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func splitLoopedString(strs []string) (ans string) {
	for i, s := range strs {
		t := reverse(s)
		if s < t {
			strs[i] = t
		}
	}
	for i, s := range strs {
		sb := &strings.Builder{}
		for _, w := range strs[i+1:] {
			sb.WriteString(w)
		}
		for _, w := range strs[:i] {
			sb.WriteString(w)
		}
		t := sb.String()
		for j := 0; j < len(s); j++ {
			a, b := s[j:], s[0:j]
			cur := a + t + b
			if ans < cur {
				ans = cur
			}
			cur = reverse(b) + t + reverse(a)
			if ans < cur {
				ans = cur
			}
		}
	}
	return ans
}

func reverse(s string) string {
	t := []byte(s)
	for i, j := 0, len(t)-1; i < j; i, j = i+1, j-1 {
		t[i], t[j] = t[j], t[i]
	}
	return string(t)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
