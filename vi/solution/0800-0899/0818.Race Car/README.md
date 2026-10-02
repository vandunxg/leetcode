---
comments: true
difficulty: Hard
tags:
    - Dynamic Programming
    - A* Search
    - Heuristic Search
---

<!-- problem:start -->

# [818. Race Car](https://leetcode.com/problems/race-car)

[中文文档](/solution/0800-0899/0818.Race%20Car/README.md)

## Mô tả

<!-- description:start -->

<p>Xe của bạn bắt đầu ở vị trí <code>0</code> với tốc độ <code>+1</code> trên một trục số vô hạn. Xe có thể đi đến các vị trí âm. Xe tự động di chuyển theo một chuỗi chỉ dẫn <code>&#39;A&#39;</code> (tăng tốc) và <code>&#39;R&#39;</code> (đổi hướng):</p>

<ul>
	<li>Khi nhận chỉ dẫn <code>&#39;A&#39;</code>, xe thực hiện các thao tác sau:

    <ul>
    	<li><code>position += speed</code></li>
    	<li><code>speed *= 2</code></li>
    </ul>
    </li>
	<li>Khi nhận chỉ dẫn <code>&#39;R&#39;</code>, xe thực hiện các thao tác sau:
    <ul>
	<li>Nếu tốc độ dương thì đặt <code>speed = -1</code></li>
	<li>nếu không thì đặt <code>speed = 1</code></li>
    </ul>
	Vị trí của xe giữ nguyên.</li>

</ul>

<p>Ví dụ, sau chuỗi lệnh <code>&quot;AAR&quot;</code>, xe lần lượt đi qua các vị trí <code>0 --&gt; 1 --&gt; 3 --&gt; 3</code>, còn tốc độ lần lượt là <code>1 --&gt; 2 --&gt; 4 --&gt; -1</code>.</p>

<p>Cho vị trí đích <code>target</code>, hãy trả về <em>độ dài của chuỗi chỉ dẫn ngắn nhất để đến đó</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> 
Chuỗi chỉ dẫn ngắn nhất là &quot;AA&quot;.
Vị trí của xe thay đổi như sau: 0 --&gt; 1 --&gt; 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = 6
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> 
Chuỗi chỉ dẫn ngắn nhất là &quot;AAARA&quot;.
Vị trí của xe thay đổi như sau: 0 --&gt; 1 --&gt; 3 --&gt; 7 --&gt; 7 --&gt; 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= target &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các chỉ dẫn giúp xe tăng tốc hoặc đổi hướng trên trục số. Với $target\le 10^4$, BFS trực tiếp trên trạng thái (vị trí, tốc độ) sẽ tốn nhiều tài nguyên. Chạy liên tiếp $k$ lệnh A sẽ đến vị trí $2^k-1$; để đến một đích bất kỳ, ta có thể vượt quá đích rồi đổi hướng, hoặc đổi hướng sớm hơn rồi tăng tốc trở lại.
>
> $dp[i]$ là số lệnh ít nhất để đến $i$. Nếu $i=2^k-1$, chỉ cần đúng $k$ lệnh A; nếu không, ta lấy min giữa hai cách: đi đến $2^k-1$ rồi đổi hướng, hoặc chạy $k-1$ lệnh A rồi đổi hướng, đi thêm $j$ bước rồi quay lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def racecar(self, target: int) -> int:
        dp = [0] * (target + 1)
        for i in range(1, target + 1):
            k = i.bit_length()
            if i == 2**k - 1:
                dp[i] = k
                continue
            dp[i] = dp[2**k - 1 - i] + k + 1
            for j in range(k - 1):
                dp[i] = min(dp[i], dp[i - (2 ** (k - 1) - 2**j)] + k - 1 + j + 2)
        return dp[target]
```

#### Java

```java
class Solution {
    public int racecar(int target) {
        int[] dp = new int[target + 1];
        for (int i = 1; i <= target; ++i) {
            int k = 32 - Integer.numberOfLeadingZeros(i);
            if (i == (1 << k) - 1) {
                dp[i] = k;
                continue;
            }
            dp[i] = dp[(1 << k) - 1 - i] + k + 1;
            for (int j = 0; j < k; ++j) {
                dp[i] = Math.min(dp[i], dp[i - (1 << (k - 1)) + (1 << j)] + k - 1 + j + 2);
            }
        }
        return dp[target];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int racecar(int target) {
        vector<int> dp(target + 1);
        for (int i = 1; i <= target; ++i) {
            int k = 32 - __builtin_clz(i);
            if (i == (1 << k) - 1) {
                dp[i] = k;
                continue;
            }
            dp[i] = dp[(1 << k) - 1 - i] + k + 1;
            for (int j = 0; j < k; ++j) {
                dp[i] = min(dp[i], dp[i - (1 << (k - 1)) + (1 << j)] + k - 1 + j + 2);
            }
        }
        return dp[target];
    }
};
```

#### Go

```go
func racecar(target int) int {
	dp := make([]int, target+1)
	for i := 1; i <= target; i++ {
		k := bits.Len(uint(i))
		if i == (1<<k)-1 {
			dp[i] = k
			continue
		}
		dp[i] = dp[(1<<k)-1-i] + k + 1
		for j := 0; j < k; j++ {
			dp[i] = min(dp[i], dp[i-(1<<(k-1))+(1<<j)]+k-1+j+2)
		}
	}
	return dp[target]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
