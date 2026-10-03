---
comments: true
difficulty: Medium
rating: 1893
source: Weekly Contest 300 Q3
tags:
    - Queue
    - Dynamic Programming
    - Simulation
---

<!-- problem:start -->

# [2327. Number of People Aware of a Secret](https://leetcode.com/problems/number-of-people-aware-of-a-secret)

[中文文档](/solution/2300-2399/2327.Number%20of%20People%20Aware%20of%20a%20Secret/README.md)

## Mô tả

<!-- description:start -->

<p>Vào ngày <code>1</code>, một người phát hiện ra một bí mật.</p>

<p>Bạn được cho một số nguyên <code>delay</code>, nghĩa là mỗi người sẽ <strong>chia sẻ</strong> bí mật với một người mới <strong>mỗi ngày</strong>, bắt đầu từ <code>delay</code> ngày sau khi phát hiện ra bí mật. Bạn cũng được cho một số nguyên <code>forget</code>, nghĩa là mỗi người sẽ <strong>quên</strong> bí mật sau <code>forget</code> ngày kể từ khi phát hiện ra nó. Một người <strong>không thể</strong> chia sẻ bí mật vào đúng ngày họ quên, hoặc vào bất kỳ ngày nào sau đó.</p>

<p>Cho một số nguyên <code>n</code>, hãy trả về<em> số người biết bí mật vào cuối ngày </em><code>n</code>. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6, delay = 2, forget = 4
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Ngày 1: Giả sử người đầu tiên tên là A. (1 người)
Ngày 2: A là người duy nhất biết bí mật. (1 người)
Ngày 3: A chia sẻ bí mật với một người mới là B. (2 người)
Ngày 4: A chia sẻ bí mật với một người mới là C. (3 người)
Ngày 5: A quên bí mật, còn B chia sẻ bí mật với một người mới là D. (3 người)
Ngày 6: B chia sẻ bí mật với E, còn C chia sẻ bí mật với F. (5 người)
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, delay = 1, forget = 3
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
Ngày 1: Người đầu tiên tên là A. (1 người)
Ngày 2: A chia sẻ bí mật với B. (2 người)
Ngày 3: A và B chia sẻ bí mật với 2 người mới là C và D. (4 người)
Ngày 4: A quên bí mật. B, C và D chia sẻ bí mật với 3 người mới. (6 người)
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= delay &lt; forget &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu

<!-- thinking:start -->

> **Tư duy**
>
> Một người chia sẻ một lần mỗi ngày trong khoảng $[delay, forget)$ sau khi biết bí mật, rồi quên nó. Vì $n \le 1000$, ta có thể mô phỏng theo từng ngày.
>
> Gọi $cnt[i]$ là số người mới biết bí mật vào ngày $i$; họ sẽ tiếp tục chia sẻ vào mỗi ngày đủ điều kiện sau đó. Một mảng hiệu $d$ theo dõi những người vẫn còn nhớ bí mật; tổng tiền tố đến ngày $n$ là đáp án.

<!-- thinking:end -->

Ta sử dụng một mảng hiệu $d[i]$ để ghi nhận sự thay đổi trong số người biết bí mật vào ngày $i$, và một mảng $cnt[i]$ để ghi nhận số người mới biết bí mật vào ngày $i$.

Với $cnt[i]$ người mới biết bí mật vào ngày $i$, họ có thể chia sẻ bí mật với thêm $cnt[i]$ người mỗi ngày trong khoảng $[i+\text{delay}, i+\text{forget})$.

Đáp án là $\sum_{i=1}^{n} d[i]$.

Độ phức tạp thời gian là $O(n^2)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là số nguyên đã cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def peopleAwareOfSecret(self, n: int, delay: int, forget: int) -> int:
        m = (n << 1) + 10
        d = [0] * m
        cnt = [0] * m
        cnt[1] = 1
        for i in range(1, n + 1):
            if cnt[i]:
                d[i] += cnt[i]
                d[i + forget] -= cnt[i]
                nxt = i + delay
                while nxt < i + forget:
                    cnt[nxt] += cnt[i]
                    nxt += 1
        mod = 10**9 + 7
        return sum(d[: n + 1]) % mod
```

#### Java

```java
class Solution {
    public int peopleAwareOfSecret(int n, int delay, int forget) {
        final int mod = (int) 1e9 + 7;
        int m = (n << 1) + 10;
        long[] d = new long[m];
        long[] cnt = new long[m];
        cnt[1] = 1;
        for (int i = 1; i <= n; ++i) {
            if (cnt[i] > 0) {
                d[i] = (d[i] + cnt[i]) % mod;
                d[i + forget] = (d[i + forget] - cnt[i] + mod) % mod;
                int nxt = i + delay;
                while (nxt < i + forget) {
                    cnt[nxt] = (cnt[nxt] + cnt[i]) % mod;
                    ++nxt;
                }
            }
        }
        long ans = 0;
        for (int i = 1; i <= n; ++i) {
            ans = (ans + d[i]) % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int peopleAwareOfSecret(int n, int delay, int forget) {
        const int mod = 1e9 + 7;
        int m = (n << 1) + 10;
        vector<long long> d(m), cnt(m);
        cnt[1] = 1;
        for (int i = 1; i <= n; i++) {
            if (cnt[i]) {
                d[i] = (d[i] + cnt[i]) % mod;
                d[i + forget] = (d[i + forget] - cnt[i] + mod) % mod;
                int nxt = i + delay;
                while (nxt < i + forget) {
                    cnt[nxt] = (cnt[nxt] + cnt[i]) % mod;
                    nxt++;
                }
            }
        }
        long long ans = 0;
        for (int i = 0; i <= n; i++) {
            ans += d[i];
        }
        return ans % mod;
    }
};
```

#### Go

```go
func peopleAwareOfSecret(n int, delay int, forget int) int {
	m := (n << 1) + 10
	d := make([]int, m)
	cnt := make([]int, m)
	mod := int(1e9) + 7
	cnt[1] = 1
	for i := 1; i <= n; i++ {
		if cnt[i] == 0 {
			continue
		}
		d[i] = (d[i] + cnt[i]) % mod
		d[i+forget] = (d[i+forget] - cnt[i] + mod) % mod
		nxt := i + delay
		for nxt < i+forget {
			cnt[nxt] = (cnt[nxt] + cnt[i]) % mod
			nxt++
		}
	}
	ans := 0
	for i := 1; i <= n; i++ {
		ans = (ans + d[i]) % mod
	}
	return ans
}
```

#### TypeScript

```ts
function peopleAwareOfSecret(n: number, delay: number, forget: number): number {
    const mod = 1e9 + 7;
    const m = (n << 1) + 10;
    const d: number[] = Array(m).fill(0);
    const cnt: number[] = Array(m).fill(0);

    cnt[1] = 1;
    for (let i = 1; i <= n; ++i) {
        if (cnt[i] > 0) {
            d[i] = (d[i] + cnt[i]) % mod;
            d[i + forget] = (d[i + forget] - cnt[i] + mod) % mod;
            let nxt = i + delay;
            while (nxt < i + forget) {
                cnt[nxt] = (cnt[nxt] + cnt[i]) % mod;
                ++nxt;
            }
        }
    }

    let ans = 0;
    for (let i = 1; i <= n; ++i) {
        ans = (ans + d[i]) % mod;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn people_aware_of_secret(n: i32, delay: i32, forget: i32) -> i32 {
        let n = n as usize;
        let delay = delay as usize;
        let forget = forget as usize;
        let m = (n << 1) + 10;
        let modulo: i64 = 1_000_000_007;

        let mut d = vec![0i64; m];
        let mut cnt = vec![0i64; m];

        cnt[1] = 1;
        for i in 1..=n {
            if cnt[i] > 0 {
                d[i] = (d[i] + cnt[i]) % modulo;
                d[i + forget] = (d[i + forget] - cnt[i] + modulo) % modulo;
                let mut nxt = i + delay;
                while nxt < i + forget {
                    cnt[nxt] = (cnt[nxt] + cnt[i]) % modulo;
                    nxt += 1;
                }
            }
        }

        let mut ans: i64 = 0;
        for i in 1..=n {
            ans = (ans + d[i]) % modulo;
        }
        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
