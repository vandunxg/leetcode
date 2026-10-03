---
comments: true
difficulty: Hard
rating: 2090
source: Biweekly Contest 73 Q4
tags:
    - Greedy
    - Binary Indexed Tree
    - Two Pointers
    - String
---

<!-- problem:start -->

# [2193. Minimum Number of Moves to Make Palindrome](https://leetcode.com/problems/minimum-number-of-moves-to-make-palindrome)

[中文文档](/solution/2100-2199/2193.Minimum%20Number%20of%20Moves%20to%20Make%20Palindrome/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Trong một <strong>lần di chuyển</strong>, bạn có thể chọn hai ký tự <strong>liền kề</strong> bất kỳ của <code>s</code> và đổi chỗ chúng.</p>

<p>Trả về <em><strong>số lần di chuyển tối thiểu</strong> cần thiết để biến</em> <code>s</code> <em>thành một palindrome</em>.</p>

<p><strong>Lưu ý</strong> rằng dữ liệu đầu vào được tạo sao cho <code>s</code> luôn có thể được biến đổi thành một palindrome.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aabb&quot;
<strong>Output:</strong> 2
<strong>Giải thích:</strong>
Ta có thể tạo hai palindrome từ s là &quot;abba&quot; và &quot;baab&quot;.
- Có thể biến s thành &quot;abba&quot; sau 2 lần di chuyển: &quot;a<u><strong>ab</strong></u>b&quot; -&gt; &quot;ab<u><strong>ab</strong></u>&quot; -&gt; &quot;abba&quot;.
- Có thể biến s thành &quot;baab&quot; sau 2 lần di chuyển: &quot;a<u><strong>ab</strong></u>b&quot; -&gt; &quot;<u><strong>ab</strong></u>ab&quot; -&gt; &quot;baab&quot;.
Do đó, số lần di chuyển tối thiểu cần thiết để biến s thành một palindrome là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;letelt&quot;
<strong>Output:</strong> 2
<strong>Giải thích:</strong>
Một trong các palindrome có thể tạo ra sau 2 lần di chuyển là &quot;lettel&quot;.
Một cách để tạo ra palindrome này là &quot;lete<u><strong>lt</strong></u>&quot; -&gt; &quot;let<u><strong>et</strong></u>l&quot; -&gt; &quot;lettel&quot;.
Các palindrome khác như &quot;tleelt&quot; cũng có thể được tạo ra sau 2 lần di chuyển.
Có thể chứng minh rằng không thể tạo ra một palindrome với ít hơn 2 lần di chuyển.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 2000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>s</code> có thể được biến đổi thành một palindrome sau một số hữu hạn lần di chuyển.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các lần đổi chỗ ký tự liền kề phải biến chuỗi thành một palindrome; đề bài đảm bảo điều này luôn thực hiện được. Chi phí là tổng khoảng cách mà các cặp chữ cái phải di chuyển đến những vị trí đối xứng. Việc tìm kiếm tất cả các chuỗi thao tác đổi chỗ là quá lớn; với $n\le 2000$, ta có thể dùng một thuật toán tham lam $O(n^2)$.
>
> Cố định chữ cái ngoài cùng bên trái $a$, tìm chữ cái $a$ gần nhất tính từ bên phải, đổi chỗ nó đến cuối chuỗi, rồi tiếp tục xử lý chuỗi con bên trong. Nếu $a$ không có ký tự tương ứng bên phải (tức là chữ cái lẻ duy nhất), đưa nó đến giữa với chi phí bằng khoảng cách đến vị trí trung tâm.
>
> Ghép cặp chữ cái ngoài cùng không bao giờ tệ hơn việc ghép một chữ cái bên trong trước, nên chiến lược tham lam là tối ưu.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMovesToMakePalindrome(self, s: str) -> int:
        cs = list(s)
        ans, n = 0, len(s)
        i, j = 0, n - 1
        while i < j:
            even = False
            for k in range(j, i, -1):
                if cs[i] == cs[k]:
                    even = True
                    while k < j:
                        cs[k], cs[k + 1] = cs[k + 1], cs[k]
                        k += 1
                        ans += 1
                    j -= 1
                    break
            if not even:
                ans += n // 2 - i
            i += 1
        return ans
```

#### Java

```java
class Solution {
    public int minMovesToMakePalindrome(String s) {
        int n = s.length();
        int ans = 0;
        char[] cs = s.toCharArray();
        for (int i = 0, j = n - 1; i < j; ++i) {
            boolean even = false;
            for (int k = j; k != i; --k) {
                if (cs[i] == cs[k]) {
                    even = true;
                    for (; k < j; ++k) {
                        char t = cs[k];
                        cs[k] = cs[k + 1];
                        cs[k + 1] = t;
                        ++ans;
                    }
                    --j;
                    break;
                }
            }
            if (!even) {
                ans += n / 2 - i;
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
    int minMovesToMakePalindrome(string s) {
        int n = s.size();
        int ans = 0;
        for (int i = 0, j = n - 1; i < j; ++i) {
            bool even = false;
            for (int k = j; k != i; --k) {
                if (s[i] == s[k]) {
                    even = true;
                    for (; k < j; ++k) {
                        swap(s[k], s[k + 1]);
                        ++ans;
                    }
                    --j;
                    break;
                }
            }
            if (!even) ans += n / 2 - i;
        }
        return ans;
    }
};
```

#### Go

```go
func minMovesToMakePalindrome(s string) int {
	cs := []byte(s)
	ans, n := 0, len(s)
	for i, j := 0, n-1; i < j; i++ {
		even := false
		for k := j; k != i; k-- {
			if cs[i] == cs[k] {
				even = true
				for ; k < j; k++ {
					cs[k], cs[k+1] = cs[k+1], cs[k]
					ans++
				}
				j--
				break
			}
		}
		if !even {
			ans += n/2 - i
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
