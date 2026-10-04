---
comments: true
difficulty: Medium
rating: 1515
source: Weekly Contest 392 Q2
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [3106. Lexicographically Smallest String After Operations With Constraint](https://leetcode.com/problems/lexicographically-smallest-string-after-operations-with-constraint)

[Tài liệu tiếng Trung](/solution/3100-3199/3106.Lexicographically%20Smallest%20String%20After%20Operations%20With%20Constraint/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một số nguyên <code>k</code>.</p>

<p>Định nghĩa hàm <code>distance(s<sub>1</sub>, s<sub>2</sub>)</code> giữa hai chuỗi <code>s<sub>1</sub></code> và <code>s<sub>2</sub></code> có cùng độ dài <code>n</code> như sau:</p>

<ul>
	<li><strong>Tổng</strong> <strong>khoảng cách nhỏ nhất</strong> giữa <code>s<sub>1</sub>[i]</code> và <code>s<sub>2</sub>[i]</code> khi các ký tự từ <code>&#39;a&#39;</code> đến <code>&#39;z&#39;</code> được sắp xếp theo thứ tự <strong>vòng tròn</strong>, với mọi <code>i</code> trong khoảng <code>[0, n - 1]</code>.</li>
</ul>

<p>Ví dụ, <code>distance(&quot;ab&quot;, &quot;cd&quot;) == 4</code>, và <code>distance(&quot;a&quot;, &quot;z&quot;) == 1</code>.</p>

<p>Bạn có thể <strong>thay đổi</strong> bất kỳ chữ cái nào trong <code>s</code> thành <strong>bất kỳ</strong> chữ cái tiếng Anh thường nào khác, với <strong>bất kỳ</strong> số lần nào.</p>

<p>Trả về một chuỗi biểu thị chuỗi <code>t</code> <strong><span data-keyword="lexicographically-smaller-string">nhỏ nhất theo thứ tự từ điển</span></strong> có thể nhận được sau một số lần thay đổi, sao cho <code>distance(s, t) &lt;= k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;zbbz&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;aaaz&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thay đổi <code>s</code> thành <code>&quot;aaaz&quot;</code>. Khoảng cách giữa <code>&quot;zbbz&quot;</code> và <code>&quot;aaaz&quot;</code> bằng <code>k = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;xaxcd&quot;, k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;aawcd&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Khoảng cách giữa &quot;xaxcd&quot; và &quot;aawcd&quot; bằng k = 4.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;lol&quot;, k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;lol&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể thay đổi bất kỳ ký tự nào vì <code>k = 0</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>0 &lt;= k &lt;= 2000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một ký tự có thể di chuyển trên vòng tròn alphabet với tổng chi phí không vượt quá $k$, và chuỗi cần trở thành chuỗi nhỏ nhất theo thứ tự từ điển. Việc tìm kiếm trên toàn bộ ngân sách còn lại tăng theo cả vị trí và $k$.
>
> Thứ tự từ điển được quyết định từ trái sang phải, vì vậy việc thu nhỏ prefix nhiều nhất có thể sẽ không ảnh hưởng xấu đến các vị trí phía sau. Khoảng cách nhỏ nhất trên vòng tròn từ $c_1$ đến một $c_2$ nhỏ hơn là $\min(c_1-c_2,\,26-(c_1-c_2))$.
>
> Duyệt từ trái sang phải, thử các chữ cái nhỏ hơn chữ cái hiện tại và chọn thay đổi khả thi có chi phí nhỏ nhất, sau đó trừ chi phí đó khỏi $k$. Alphabet có kích thước $26$, nên việc liệt kê bên trong có độ phức tạp hằng số.

<!-- thinking:end -->

Ta có thể duyệt qua từng vị trí của chuỗi $s$. Tại mỗi vị trí, ta liệt kê tất cả các ký tự nhỏ hơn ký tự hiện tại và tính chi phí $d$ để thay đổi thành ký tự đó. Nếu $d \leq k$, ta thay đổi ký tự hiện tại thành ký tự đó, trừ $d$ khỏi $k$, kết thúc việc liệt kê và tiếp tục với vị trí tiếp theo.

Sau khi duyệt xong, ta nhận được một chuỗi thỏa mãn các điều kiện.

Độ phức tạp thời gian là $O(n \times |\Sigma|)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài chuỗi $s$, còn $|\Sigma|$ là kích thước của tập ký tự. Trong bài toán này, $|\Sigma| \leq 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getSmallestString(self, s: str, k: int) -> str:
        cs = list(s)
        for i, c1 in enumerate(s):
            for c2 in ascii_lowercase:
                if c2 >= c1:
                    break
                d = min(ord(c1) - ord(c2), 26 - ord(c1) + ord(c2))
                if d <= k:
                    cs[i] = c2
                    k -= d
                    break
        return "".join(cs)
```

#### Java

```java
class Solution {
    public String getSmallestString(String s, int k) {
        char[] cs = s.toCharArray();
        for (int i = 0; i < cs.length; ++i) {
            char c1 = cs[i];
            for (char c2 = 'a'; c2 < c1; ++c2) {
                int d = Math.min(c1 - c2, 26 - c1 + c2);
                if (d <= k) {
                    cs[i] = c2;
                    k -= d;
                    break;
                }
            }
        }
        return new String(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string getSmallestString(string s, int k) {
        for (int i = 0; i < s.size(); ++i) {
            char c1 = s[i];
            for (char c2 = 'a'; c2 < c1; ++c2) {
                int d = min(c1 - c2, 26 - c1 + c2);
                if (d <= k) {
                    s[i] = c2;
                    k -= d;
                    break;
                }
            }
        }
        return s;
    }
};
```

#### Go

```go
func getSmallestString(s string, k int) string {
	cs := []byte(s)
	for i, c1 := range cs {
		for c2 := byte('a'); c2 < c1; c2++ {
			d := int(min(c1-c2, 26-c1+c2))
			if d <= k {
				cs[i] = c2
				k -= d
				break
			}
		}
	}
	return string(cs)
}
```

#### TypeScript

```ts
function getSmallestString(s: string, k: number): string {
    const cs: string[] = s.split('');
    for (let i = 0; i < s.length; ++i) {
        for (let j = 97; j < s[i].charCodeAt(0); ++j) {
            const d = Math.min(s[i].charCodeAt(0) - j, 26 - s[i].charCodeAt(0) + j);
            if (d <= k) {
                cs[i] = String.fromCharCode(j);
                k -= d;
                break;
            }
        }
    }
    return cs.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
