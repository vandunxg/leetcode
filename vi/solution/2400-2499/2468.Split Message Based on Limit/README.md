---
comments: true
difficulty: Hard
rating: 2381
source: Biweekly Contest 91 Q4
tags:
    - String
    - Enumeration
---

<!-- problem:start -->

# [2468. Split Message Based on Limit](https://leetcode.com/problems/split-message-based-on-limit)

[中文文档](/solution/2400-2499/2468.Split%20Message%20Based%20on%20Limit/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi, <code>message</code>, và một số nguyên dương, <code>limit</code>.</p>

<p>Bạn phải <strong>chia</strong> <code>message</code> thành một hoặc nhiều <strong>phần</strong> dựa trên <code>limit</code>. Mỗi phần tạo thành phải có hậu tố <code>&quot;&lt;a/b&gt;&quot;</code>, trong đó <code>&quot;b&quot;</code> được <strong>thay thế</strong> bằng tổng số phần và <code>&quot;a&quot;</code> được <strong>thay thế</strong> bằng chỉ số của phần, bắt đầu từ <code>1</code> và tăng đến <code>b</code>. Ngoài ra, độ dài của mỗi phần tạo thành (bao gồm cả hậu tố) phải <strong>bằng</strong> <code>limit</code>, ngoại trừ phần cuối cùng có thể có độ dài <strong>không vượt quá</strong> <code>limit</code>.</p>

<p>Các phần tạo thành phải được xây dựng sao cho khi bỏ hậu tố và nối tất cả chúng lại <strong>theo thứ tự</strong>, kết quả bằng <code>message</code>. Đồng thời, kết quả phải có ít phần nhất có thể.</p>

<p>Trả về <em>các phần mà</em> <code>message</code><em> được chia thành dưới dạng một mảng chuỗi</em>. Nếu không thể chia <code>message</code> theo yêu cầu, hãy trả về <em>mảng rỗng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> message = &quot;this is really a very awesome message&quot;, limit = 9
<strong>Đầu ra:</strong> [&quot;thi&lt;1/14&gt;&quot;,&quot;s i&lt;2/14&gt;&quot;,&quot;s r&lt;3/14&gt;&quot;,&quot;eal&lt;4/14&gt;&quot;,&quot;ly &lt;5/14&gt;&quot;,&quot;a v&lt;6/14&gt;&quot;,&quot;ery&lt;7/14&gt;&quot;,&quot; aw&lt;8/14&gt;&quot;,&quot;eso&lt;9/14&gt;&quot;,&quot;me&lt;10/14&gt;&quot;,&quot; m&lt;11/14&gt;&quot;,&quot;es&lt;12/14&gt;&quot;,&quot;sa&lt;13/14&gt;&quot;,&quot;ge&lt;14/14&gt;&quot;]
<strong>Giải thích:</strong>
9 phần đầu tiên mỗi phần lấy 3 ký tự từ đầu của message.
5 phần tiếp theo mỗi phần lấy 2 ký tự để hoàn tất việc chia message.
Trong ví dụ này, mỗi phần, kể cả phần cuối, đều có độ dài 9.
Có thể chứng minh rằng không thể chia message thành ít hơn 14 phần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> message = &quot;short message&quot;, limit = 15
<strong>Đầu ra:</strong> [&quot;short mess&lt;1/2&gt;&quot;,&quot;age&lt;2/2&gt;&quot;]
<strong>Giải thích:</strong>
Với các ràng buộc đã cho, chuỗi có thể được chia thành hai phần:
- Phần đầu tiên gồm 10 ký tự đầu tiên và có độ dài 15.
- Phần tiếp theo gồm 3 ký tự cuối cùng và có độ dài 8.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= message.length &lt;= 10<sup>4</sup></code></li>
	<li><code>message</code> chỉ gồm các chữ cái tiếng Anh viết thường và <code>&#39; &#39;</code>.</li>
	<li><code>1 &lt;= limit &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê số đoạn + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi phần chứa một hậu tố $<j/k>$ trong giới hạn độ dài $limit$. Vì $n\le 10^4$, ta thử lần lượt $k=1,2,\ldots$: tổng độ dài chữ số của các chỉ số và của $k$, cộng với ba ký hiệu trong mỗi phần, có thể được tính trong $O(1)$. Khi dung lượng còn lại ít nhất là $n$, ta cắt message và thêm các hậu tố.

<!-- thinking:end -->

Ta ký hiệu độ dài của chuỗi `message` là $n$, và số đoạn là $k$.

Theo đề bài, nếu $k > n$, nghĩa là ta có thể chia chuỗi thành nhiều hơn $n$ đoạn. Vì độ dài chuỗi chỉ là $n$, việc chia thành nhiều hơn $n$ đoạn chắc chắn sẽ tạo ra một số đoạn có độ dài $0$, và có thể xóa chúng. Do đó, ta chỉ cần giới hạn phạm vi của $k$ trong $[1,.. n]$.

Ta liệt kê số đoạn $k$ từ nhỏ đến lớn. Gọi tổng độ dài của phần $a$ trong tất cả các đoạn là $sa$, tổng độ dài của phần $b$ trong tất cả các đoạn là $sb$, và tổng độ dài của tất cả các ký hiệu (bao gồm ngoặc nhọn và dấu gạch chéo) trong tất cả các đoạn là $sc$.

Khi đó, giá trị của $sa$ là ${\textstyle \sum_{j=1}^{k}} len(s_j)$, có thể lấy trực tiếp thông qua tổng tiền tố; giá trị của $sb$ là $len(str(k)) \times k$; và giá trị của $sc$ là $3 \times k$.

Do đó, số ký tự có thể điền vào tất cả các đoạn là $limit\times k - (sa + sb + sc)$. Nếu giá trị này lớn hơn hoặc bằng $n$, nghĩa là chuỗi có thể được chia thành $k$ đoạn, và ta có thể trực tiếp xây dựng đáp án rồi trả về.

Độ phức tạp thời gian là $O(n\times \log n)$, trong đó $n$ là độ dài của chuỗi `message`. Bỏ qua phần bộ nhớ dùng cho đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def splitMessage(self, message: str, limit: int) -> List[str]:
        n = len(message)
        sa = 0
        for k in range(1, n + 1):
            sa += len(str(k))
            sb = len(str(k)) * k
            sc = 3 * k
            if limit * k - (sa + sb + sc) >= n:
                ans = []
                i = 0
                for j in range(1, k + 1):
                    tail = f'<{j}/{k}>'
                    t = message[i : i + limit - len(tail)] + tail
                    ans.append(t)
                    i += limit - len(tail)
                return ans
        return []
```

#### Java

```java
class Solution {
    public String[] splitMessage(String message, int limit) {
        int n = message.length();
        int sa = 0;
        String[] ans = new String[0];
        for (int k = 1; k <= n; ++k) {
            int lk = (k + "").length();
            sa += lk;
            int sb = lk * k;
            int sc = 3 * k;
            if (limit * k - (sa + sb + sc) >= n) {
                int i = 0;
                ans = new String[k];
                for (int j = 1; j <= k; ++j) {
                    String tail = String.format("<%d/%d>", j, k);
                    String t = message.substring(i, Math.min(n, i + limit - tail.length())) + tail;
                    ans[j - 1] = t;
                    i += limit - tail.length();
                }
                break;
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
    vector<string> splitMessage(string message, int limit) {
        int n = message.size();
        int sa = 0;
        vector<string> ans;
        for (int k = 1; k <= n; ++k) {
            int lk = to_string(k).size();
            sa += lk;
            int sb = lk * k;
            int sc = 3 * k;
            if (k * limit - (sa + sb + sc) >= n) {
                int i = 0;
                for (int j = 1; j <= k; ++j) {
                    string tail = "<" + to_string(j) + "/" + to_string(k) + ">";
                    string t = message.substr(i, limit - tail.size()) + tail;
                    ans.emplace_back(t);
                    i += limit - tail.size();
                }
                break;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func splitMessage(message string, limit int) (ans []string) {
	n := len(message)
	sa := 0
	for k := 1; k <= n; k++ {
		lk := len(strconv.Itoa(k))
		sa += lk
		sb := lk * k
		sc := 3 * k
		if limit*k-(sa+sb+sc) >= n {
			i := 0
			for j := 1; j <= k; j++ {
				tail := "<" + strconv.Itoa(j) + "/" + strconv.Itoa(k) + ">"
				t := message[i:min(i+limit-len(tail), n)] + tail
				ans = append(ans, t)
				i += limit - len(tail)
			}
			break
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
