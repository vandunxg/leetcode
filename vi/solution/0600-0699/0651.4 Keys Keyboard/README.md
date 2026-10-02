---
comments: true
difficulty: Medium
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [651. 4 Keys Keyboard 🔒](https://leetcode.com/problems/4-keys-keyboard)

[中文文档](/solution/0600-0699/0651.4%20Keys%20Keyboard/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy tưởng tượng bạn có một bàn phím đặc biệt với các phím sau:</p>

<ul>
	<li>A: In một ký tự <code>&#39;A&#39;</code> lên màn hình.</li>
	<li>Ctrl-A: Chọn toàn bộ nội dung trên màn hình.</li>
	<li>Ctrl-C: Sao chép phần đã chọn vào buffer.</li>
	<li>Ctrl-V: In nội dung trong buffer lên màn hình, nối vào sau phần đã được in.</li>
</ul>

<p>Cho số nguyên n, hãy trả về <em>số ký tự </em><code>&#39;A&#39;</code><em> nhiều nhất có thể in lên màn hình sau khi nhấn các phím tối đa </em><code>n</code><em> lần</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể in tối đa 3 ký tự A lên màn hình bằng cách nhấn các phím theo thứ tự sau:
A, A, A
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 7
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Ta có thể in tối đa 9 ký tự A lên màn hình bằng cách nhấn các phím theo thứ tự sau:
A, A, A, Ctrl A, Ctrl C, Ctrl V, Ctrl V
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khi có thể chọn tất cả rồi sao chép, chuỗi thao tác tối ưu sẽ gõ một số ký tự `A` rồi dán. Không cần liệt kê mọi chuỗi phím có thể nhấn.
>
> $dp[i]$ là số ký tự `A` nhiều nhất có thể tạo ra sau $i$ lần nhấn phím: hoặc gõ trực tiếp $i$ ký tự `A`, hoặc nhấn Ctrl-A tại bước $j$ rồi dán $i-j$ lần, thu được $dp[j-1]\times(i-j)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxA(self, n: int) -> int:
        dp = list(range(n + 1))
        for i in range(3, n + 1):
            for j in range(2, i - 1):
                dp[i] = max(dp[i], dp[j - 1] * (i - j))
        return dp[-1]
```

#### Java

```java
class Solution {
    public int maxA(int n) {
        int[] dp = new int[n + 1];
        for (int i = 0; i < n + 1; ++i) {
            dp[i] = i;
        }
        for (int i = 3; i < n + 1; ++i) {
            for (int j = 2; j < i - 1; ++j) {
                dp[i] = Math.max(dp[i], dp[j - 1] * (i - j));
            }
        }
        return dp[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxA(int n) {
        vector<int> dp(n + 1);
        iota(dp.begin(), dp.end(), 0);
        for (int i = 3; i < n + 1; ++i) {
            for (int j = 2; j < i - 1; ++j) {
                dp[i] = max(dp[i], dp[j - 1] * (i - j));
            }
        }
        return dp[n];
    }
};
```

#### Go

```go
func maxA(n int) int {
	dp := make([]int, n+1)
	for i := range dp {
		dp[i] = i
	}
	for i := 3; i < n+1; i++ {
		for j := 2; j < i-1; j++ {
			dp[i] = max(dp[i], dp[j-1]*(i-j))
		}
	}
	return dp[n]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
