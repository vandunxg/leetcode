---
comments: true
difficulty: Easy
rating: 1271
source: Biweekly Contest 33 Q1
tags:
    - String
---

<!-- problem:start -->

# [1556. Thousand Separator](https://leetcode.com/problems/thousand-separator)

[中文文档](/solution/1500-1599/1556.Thousand%20Separator/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy thêm dấu chấm (&quot;.&quot;) làm dấu phân cách hàng nghìn và trả về kết quả ở dạng chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 987
<strong>Đầu ra:</strong> &quot;987&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1234
<strong>Đầu ra:</strong> &quot;1.234&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chèn dấu chấm sau mỗi ba chữ số tính từ phải sang trái. Nếu chuyển sang chuỗi ngay từ đầu, cần xử lý thêm trường hợp độ dài là bội của ba; lấy từng chữ số từ hàng thấp lên sẽ đơn giản hơn.
>
> Lặp lại việc lấy $n\bmod 10$ và đếm chữ số; sau mỗi ba chữ số, nếu vẫn còn hàng cao hơn thì thêm dấu chấm. Cuối cùng đảo ngược các ký tự đã thu thập. Vòng lặp chạy một lần trên mỗi chữ số, có độ phức tạp $O(\log n)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def thousandSeparator(self, n: int) -> str:
        cnt = 0
        ans = []
        while 1:
            n, v = divmod(n, 10)
            ans.append(str(v))
            cnt += 1
            if n == 0:
                break
            if cnt == 3:
                ans.append('.')
                cnt = 0
        return ''.join(ans[::-1])
```

#### Java

```java
class Solution {
    public String thousandSeparator(int n) {
        int cnt = 0;
        StringBuilder ans = new StringBuilder();
        while (true) {
            int v = n % 10;
            n /= 10;
            ans.append(v);
            ++cnt;
            if (n == 0) {
                break;
            }
            if (cnt == 3) {
                ans.append('.');
                cnt = 0;
            }
        }
        return ans.reverse().toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string thousandSeparator(int n) {
        int cnt = 0;
        string ans;
        while (1) {
            int v = n % 10;
            n /= 10;
            ans += to_string(v);
            if (n == 0) break;
            if (++cnt == 3) {
                ans += '.';
                cnt = 0;
            }
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func thousandSeparator(n int) string {
	cnt := 0
	ans := []byte{}
	for {
		v := n % 10
		n /= 10
		ans = append(ans, byte('0'+v))
		if n == 0 {
			break
		}
		cnt++
		if cnt == 3 {
			ans = append(ans, '.')
			cnt = 0
		}
	}
	for i, j := 0, len(ans)-1; i < j; i, j = i+1, j-1 {
		ans[i], ans[j] = ans[j], ans[i]
	}
	return string(ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
