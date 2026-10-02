---
comments: true
difficulty: Medium
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [650. 2 Keys Keyboard](https://leetcode.com/problems/2-keys-keyboard)

[中文文档](/solution/0600-0699/0650.2%20Keys%20Keyboard/README.md)

## Mô tả

<!-- description:start -->

<p>Trên màn hình của notepad ban đầu chỉ có một ký tự <code>&#39;A&#39;</code>. Ở mỗi bước, bạn có thể thực hiện một trong hai thao tác:</p>

<ul>
	<li>Sao chép tất cả: Sao chép toàn bộ ký tự đang hiển thị trên màn hình (không được sao chép một phần).</li>
	<li>Dán: Dán các ký tự đã sao chép ở lần gần nhất.</li>
</ul>

<p>Cho số nguyên <code>n</code>, hãy trả về <em>số thao tác ít nhất để trên màn hình có chính xác</em> <code>n</code> <em>ký tự</em> <code>&#39;A&#39;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ban đầu, ta có một ký tự &#39;A&#39;.
Bước 1, ta dùng thao tác Sao chép tất cả.
Bước 2, ta dùng thao tác Dán để được &#39;AA&#39;.
Bước 3, ta dùng thao tác Dán để được &#39;AAA&#39;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Bắt đầu với một `A` rồi dùng sao chép và dán để đạt $n$ ký tự. Cây tìm kiếm các chuỗi thao tác rất rộng.
>
> Nếu ở bước cuối, ta dán một khối độ dài $n/j$ thành $j$ bản, thì $dfs(n)=\min(dfs(n/j)+j)$. Ghi nhớ kết quả theo các ước; với $n=1$, chi phí là $0$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSteps(self, n: int) -> int:
        @cache
        def dfs(n):
            if n == 1:
                return 0
            i, ans = 2, n
            while i * i <= n:
                if n % i == 0:
                    ans = min(ans, dfs(n // i) + i)
                i += 1
            return ans

        return dfs(n)
```

#### Java

```java
class Solution {
    private int[] f;

    public int minSteps(int n) {
        f = new int[n + 1];
        Arrays.fill(f, -1);
        return dfs(n);
    }

    private int dfs(int n) {
        if (n == 1) {
            return 0;
        }
        if (f[n] != -1) {
            return f[n];
        }
        int ans = n;
        for (int i = 2; i * i <= n; ++i) {
            if (n % i == 0) {
                ans = Math.min(ans, dfs(n / i) + i);
            }
        }
        f[n] = ans;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> f;

    int minSteps(int n) {
        f.assign(n + 1, -1);
        return dfs(n);
    }

    int dfs(int n) {
        if (n == 1) return 0;
        if (f[n] != -1) return f[n];
        int ans = n;
        for (int i = 2; i * i <= n; ++i) {
            if (n % i == 0) {
                ans = min(ans, dfs(n / i) + i);
            }
        }
        f[n] = ans;
        return ans;
    }
};
```

#### Go

```go
func minSteps(n int) int {
	f := make([]int, n+1)
	for i := range f {
		f[i] = -1
	}
	var dfs func(int) int
	dfs = func(n int) int {
		if n == 1 {
			return 0
		}
		if f[n] != -1 {
			return f[n]
		}
		ans := n
		for i := 2; i*i <= n; i++ {
			if n%i == 0 {
				ans = min(ans, dfs(n/i)+i)
			}
		}
		return ans
	}
	return dfs(n)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Memoization là cách tiếp cận top-down. Có thể dùng cùng công thức truy hồi để tính $dp[i]$ bottom-up theo các ước, không cần đệ quy.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSteps(self, n: int) -> int:
        dp = list(range(n + 1))
        dp[1] = 0
        for i in range(2, n + 1):
            j = 2
            while j * j <= i:
                if i % j == 0:
                    dp[i] = min(dp[i], dp[i // j] + j)
                j += 1
        return dp[-1]
```

#### Java

```java
class Solution {
    public int minSteps(int n) {
        int[] dp = new int[n + 1];
        for (int i = 0; i < n + 1; ++i) {
            dp[i] = i;
        }
        dp[1] = 0;
        for (int i = 2; i < n + 1; ++i) {
            for (int j = 2; j * j <= i; ++j) {
                if (i % j == 0) {
                    dp[i] = Math.min(dp[i], dp[i / j] + j);
                }
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
    int minSteps(int n) {
        vector<int> dp(n + 1);
        iota(dp.begin(), dp.end(), 0);
        dp[1] = 0;
        for (int i = 2; i < n + 1; ++i) {
            for (int j = 2; j * j <= i; ++j) {
                if (i % j == 0) {
                    dp[i] = min(dp[i], dp[i / j] + j);
                }
            }
        }
        return dp[n];
    }
};
```

#### Go

```go
func minSteps(n int) int {
	dp := make([]int, n+1)
	for i := range dp {
		dp[i] = i
	}
	dp[1] = 0
	for i := 2; i < n+1; i++ {
		for j := 2; j*j <= i; j++ {
			if i%j == 0 {
				dp[i] = min(dp[i], dp[i/j]+j)
			}
		}
	}
	return dp[n]
}
```

#### TypeScript

```ts
function minSteps(n: number): number {
    const dp = Array(n + 1).fill(1000);
    dp[1] = 0;

    for (let i = 2; i <= n; i++) {
        for (let j = 1, half = i / 2; j <= half; j++) {
            if (i % j === 0) {
                dp[i] = Math.min(dp[i], dp[j] + i / j);
            }
        }
    }

    return dp[n];
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */
var minSteps = function (n) {
    const dp = Array(n + 1).fill(1000);
    dp[1] = 0;

    for (let i = 2; i <= n; i++) {
        for (let j = 1, half = i / 2; j <= half; j++) {
            if (i % j === 0) {
                dp[i] = Math.min(dp[i], dp[j] + i / j);
            }
        }
    }

    return dp[n];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Số thao tác tối thiểu bằng tổng các thừa số nguyên tố của $n$: với thừa số $i$, cần một lần sao chép và $i-1$ lần dán. Phân tích $n$ thành thừa số nguyên tố để không cần lập bảng DP.

<!-- thinking:end -->

Phân tích $n$ thành thừa số nguyên tố; mỗi thừa số nguyên tố $i$ tương ứng với $i$ thao tác.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int minSteps(int n) {
        int res = 0;
        for (int i = 2; n > 1; ++i) {
            while (n % i == 0) {
                res += i;
                n /= i;
            }
        }
        return res;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
