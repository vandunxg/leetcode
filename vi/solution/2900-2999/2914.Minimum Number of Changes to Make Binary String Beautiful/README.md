---
comments: true
difficulty: Medium
rating: 1479
source: Biweekly Contest 116 Q2
tags:
    - String
---

<!-- problem:start -->

# [2914. Minimum Number of Changes to Make Binary String Beautiful](https://leetcode.com/problems/minimum-number-of-changes-to-make-binary-string-beautiful)

[中文文档](/solution/2900-2999/2914.Minimum%20Number%20of%20Changes%20to%20Make%20Binary%20String%20Beautiful/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi nhị phân <code>s</code> được <strong>đánh chỉ số từ 0</strong> và có độ dài chẵn.</p>

<p>Một chuỗi được gọi là <strong>đẹp</strong> nếu có thể phân chia chuỗi đó thành một hoặc nhiều chuỗi con sao cho:</p>

<ul>
	<li>Mỗi chuỗi con có <strong>độ dài chẵn</strong>.</li>
	<li>Mỗi chuỗi con <strong>chỉ</strong> chứa các ký tự <code>1</code> hoặc <strong>chỉ</strong> chứa các ký tự <code>0</code>.</li>
</ul>

<p>Bạn có thể thay đổi bất kỳ ký tự nào trong <code>s</code> thành <code>0</code> hoặc <code>1</code>.</p>

<p>Trả về <em>số lượng thay đổi <strong>nhỏ nhất</strong> cần thực hiện để biến chuỗi </em><code>s</code><em> thành chuỗi đẹp.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1001&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Đổi s[1] thành 1 và s[3] thành 0 để nhận được chuỗi &quot;1100&quot;.
Có thể thấy chuỗi &quot;1100&quot; là chuỗi đẹp vì ta có thể phân chia nó thành &quot;11|00&quot;.
Có thể chứng minh rằng 2 là số lượng thay đổi nhỏ nhất cần thực hiện để biến chuỗi thành chuỗi đẹp.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;10&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Đổi s[1] thành 1 để nhận được chuỗi &quot;11&quot;.
Có thể thấy chuỗi &quot;11&quot; là chuỗi đẹp vì ta có thể phân chia nó thành &quot;11&quot;.
Có thể chứng minh rằng 1 là số lượng thay đổi nhỏ nhất cần thực hiện để biến chuỗi thành chuỗi đẹp.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0000&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không cần thay đổi vì chuỗi &quot;0000&quot; vốn đã là chuỗi đẹp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> có độ dài chẵn.</li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi đẹp được chia thành các cặp bằng nhau, và cách ghép cặp là cố định: $(s[0],s[1])$, $(s[2],s[3])$, v.v. Các cặp không ảnh hưởng lẫn nhau. Việc thay đổi ký tự giữa các cặp không thể giảm chi phí bên trong một cặp.
>
> Do đó, chỉ cần kiểm tra mỗi chỉ số lẻ với ký tự ngay trước nó và đếm số vị trí khác nhau. Với $n \le 10^5$, một lần duyệt với bước nhảy $2$ là đủ.

<!-- thinking:end -->

Chúng ta chỉ cần duyệt qua tất cả các chỉ số lẻ $1, 3, 5, \cdots$ của chuỗi $s$. Nếu chỉ số lẻ hiện tại khác chỉ số liền trước, tức là $s[i] \ne s[i - 1]$, ta cần thay đổi ký tự hiện tại để $s[i] = s[i - 1]$. Vì vậy, tăng đáp án lên $1$.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minChanges(self, s: str) -> int:
        return sum(s[i] != s[i - 1] for i in range(1, len(s), 2))
```

#### Java

```java
class Solution {
    public int minChanges(String s) {
        int ans = 0;
        for (int i = 1; i < s.length(); i += 2) {
            if (s.charAt(i) != s.charAt(i - 1)) {
                ++ans;
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
    int minChanges(string s) {
        int ans = 0;
        int n = s.size();
        for (int i = 1; i < n; i += 2) {
            ans += s[i] != s[i - 1];
        }
        return ans;
    }
};
```

#### Go

```go
func minChanges(s string) (ans int) {
	for i := 1; i < len(s); i += 2 {
		if s[i] != s[i-1] {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function minChanges(s: string): number {
    let ans = 0;
    for (let i = 1; i < s.length; i += 2) {
        if (s[i] !== s[i - 1]) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
