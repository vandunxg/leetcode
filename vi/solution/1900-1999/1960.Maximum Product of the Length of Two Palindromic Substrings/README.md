---
comments: true
difficulty: Hard
rating: 2690
source: Biweekly Contest 58 Q4
tags:
    - Two Pointers
    - String
    - Manacher
    - Hash Function
    - Rolling Hash
    - Palindromic Tre
---

<!-- problem:start -->

# [1960. Maximum Product of the Length of Two Palindromic Substrings](https://leetcode.com/problems/maximum-product-of-the-length-of-two-palindromic-substrings)

[Tài liệu tiếng Trung](/solution/1900-1999/1960.Maximum%20Product%20of%20the%20Length%20of%20Two%20Palindromic%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <strong>0-indexed</strong> <code>s</code>, hãy tìm hai <strong>chuỗi con đối xứng không giao nhau</strong> có <strong>độ dài lẻ</strong> sao cho tích độ dài của chúng là lớn nhất.</p>

<p>Cụ thể, cần chọn bốn số nguyên <code>i</code>, <code>j</code>, <code>k</code>, <code>l</code> sao cho <code>0 &lt;= i &lt;= j &lt; k &lt;= l &lt; s.length</code>, đồng thời cả hai chuỗi con <code>s[i...j]</code> và <code>s[k...l]</code> đều là chuỗi đối xứng và có độ dài lẻ. <code>s[i...j]</code> biểu thị chuỗi con từ chỉ số <code>i</code> đến chỉ số <code>j</code>, <strong>bao gồm cả hai đầu mút</strong>.</p>

<p>Trả về <em><strong>tích lớn nhất</strong> có thể đạt được của độ dài hai chuỗi con đối xứng không giao nhau.</em></p>

<p><strong>Chuỗi đối xứng</strong> là chuỗi đọc xuôi và đọc ngược giống nhau. <strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ababbb&quot;
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Các chuỗi con &quot;aba&quot; và &quot;bbb&quot; là những chuỗi đối xứng có độ dài lẻ. Tích = 3 * 3 = 9.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;zaaaxbbby&quot;
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Các chuỗi con &quot;aaa&quot; và &quot;bbb&quot; là những chuỗi đối xứng có độ dài lẻ. Tích = 3 * 3 = 9.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ bao gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mục tiêu là tìm tích lớn nhất của độ dài hai chuỗi con đối xứng lẻ không giao nhau. Việc lặp lại cách tìm chuỗi đối xứng trong thời gian bậc hai cho từng vị trí chia sẽ quá chậm.
>
> Manacher cho biết bán kính của chuỗi đối xứng lẻ tại mọi tâm. Từ các bán kính đó, ta suy ra chuỗi đối xứng dài nhất kết thúc tại hoặc bắt đầu ở mỗi chỉ số, rồi tính giá trị lớn nhất trên prefix/suffix cho từng vị trí chia.
>
> Đáp án là tích lớn nhất giữa giá trị lớn nhất trên prefix bên trái và giá trị lớn nhất trên suffix bên phải của mọi vị trí chia.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProduct(self, s: str) -> int:
        n = len(s)
        hlen = [0] * n
        center = right = 0

        for i in range(n):
            if i < right:
                hlen[i] = min(right - i, hlen[2 * center - i])
            while (
                0 <= i - 1 - hlen[i]
                and i + 1 + hlen[i] < len(s)
                and s[i - 1 - hlen[i]] == s[i + 1 + hlen[i]]
            ):
                hlen[i] += 1
            if right < i + hlen[i]:
                center, right = i, i + hlen[i]

        prefix = [0] * n
        suffix = [0] * n

        for i in range(n):
            prefix[i + hlen[i]] = max(prefix[i + hlen[i]], 2 * hlen[i] + 1)
            suffix[i - hlen[i]] = max(suffix[i - hlen[i]], 2 * hlen[i] + 1)

        for i in range(1, n):
            prefix[~i] = max(prefix[~i], prefix[~i + 1] - 2)
            suffix[i] = max(suffix[i], suffix[i - 1] - 2)

        for i in range(1, n):
            prefix[i] = max(prefix[i - 1], prefix[i])
            suffix[~i] = max(suffix[~i], suffix[~i + 1])

        return max(prefix[i - 1] * suffix[i] for i in range(1, n))
```

#### Java

```java
class Solution {
    public long maxProduct(String s) {
        int n = s.length();
        if (n == 2) return 1;
        int[] len = manachers(s);
        long[] left = new long[n];
        int max = 1;
        left[0] = max;
        for (int i = 1; i <= n - 1; i++) {
            if (len[(i - max - 1 + i) / 2] > max) max += 2;
            left[i] = max;
        }
        max = 1;
        long[] right = new long[n];
        right[n - 1] = max;
        for (int i = n - 2; i >= 0; i--) {
            if (len[(i + max + 1 + i) / 2] > max) max += 2;
            right[i] = max;
        }
        long res = 1;
        for (int i = 1; i < n; i++) {
            res = Math.max(res, left[i - 1] * right[i]);
        }
        return res;
    }
    private int[] manachers(String s) {
        int len = s.length();
        int[] P = new int[len];
        int c = 0;
        int r = 0;
        for (int i = 0; i < len; i++) {
            int mirror = (2 * c) - i;
            if (i < r) {
                P[i] = Math.min(r - i, P[mirror]);
            }
            int a = i + (1 + P[i]);
            int b = i - (1 + P[i]);
            while (a < len && b >= 0 && s.charAt(a) == s.charAt(b)) {
                P[i]++;
                a++;
                b--;
            }
            if (i + P[i] > r) {
                c = i;
                r = i + P[i];
            }
        }
        for (int i = 0; i < len; i++) {
            P[i] = 1 + 2 * P[i];
        }
        return P;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxProduct(string s) {
        long long res = 0, l = 0, n = s.size();
        vector<int> m(n), r(n);

        for (int i = 0, l = 0, r = -1; i < n; ++i) {
            int k = (i > r) ? 1 : min(m[l + r - i], r - i + 1);
            while (0 <= i - k && i + k < n && s[i - k] == s[i + k])
                k++;
            m[i] = k--;
            if (i + k > r) {
                l = i - k;
                r = i + k;
            }
        }

        queue<array<int, 2>> q, q1;

        for (int i = n - 1; i >= 0; --i) {
            while (!q.empty() && q.front()[0] - q.front()[1] > i - 1)
                q.pop();
            r[i] = 1 + (q.empty() ? 0 : (q.front()[0] - i) * 2);
            q.push({i, m[i]});
        }

        for (int i = 0; i < n - 1; i++) {
            while (!q1.empty() && q1.front()[0] + q1.front()[1] < i + 1)
                q1.pop();
            l = max(l, 1ll + (q1.empty() ? 0 : (i - q1.front()[0]) * 2));
            res = max(res, l * r[i + 1]);
            q1.push({i, m[i]});
        }

        return res;
    }
};
```

#### Go

```go
func maxProduct(s string) int64 {
	n := len(s)
	hlen := make([]int, n)
	center, right := 0, 0

	for i := 0; i < n; i++ {
		if i < right {
			mirror := 2*center - i
			if mirror >= 0 && mirror < n {
				hlen[i] = min(right-i, hlen[mirror])
			}
		}
		for i-1-hlen[i] >= 0 && i+1+hlen[i] < n && s[i-1-hlen[i]] == s[i+1+hlen[i]] {
			hlen[i]++
		}
		if i+hlen[i] > right {
			center = i
			right = i + hlen[i]
		}
	}

	prefix := make([]int, n)
	suffix := make([]int, n)

	for i := 0; i < n; i++ {
		r := i + hlen[i]
		if r < n {
			prefix[r] = max(prefix[r], 2*hlen[i]+1)
		}
		l := i - hlen[i]
		if l >= 0 {
			suffix[l] = max(suffix[l], 2*hlen[i]+1)
		}
	}

	for i := 1; i < n; i++ {
		if n-i-1 >= 0 {
			prefix[n-i-1] = max(prefix[n-i-1], prefix[n-i]-2)
		}
		suffix[i] = max(suffix[i], suffix[i-1]-2)
	}

	for i := 1; i < n; i++ {
		prefix[i] = max(prefix[i-1], prefix[i])
		suffix[n-i-1] = max(suffix[n-i], suffix[n-i-1])
	}

	var res int64
	for i := 1; i < n; i++ {
		prod := int64(prefix[i-1]) * int64(suffix[i])
		if prod > res {
			res = prod
		}
	}

	return res
}
```

#### TypeScript

```ts
function maxProduct(s: string): number {
    const n = s.length;
    const hlen = Array(n).fill(0);
    let center = 0;
    let right = 0;
    for (let i = 0; i < n; ++i) {
        if (i < right) {
            hlen[i] = Math.min(right - i, hlen[2 * center - i]);
        }
        while (
            i - 1 - hlen[i] >= 0 &&
            i + 1 + hlen[i] < n &&
            s[i - 1 - hlen[i]] === s[i + 1 + hlen[i]]
        ) {
            ++hlen[i];
        }
        if (right < i + hlen[i]) {
            center = i;
            right = i + hlen[i];
        }
    }
    const prefix = Array(n).fill(0);
    const suffix = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        prefix[i + hlen[i]] = Math.max(prefix[i + hlen[i]], 2 * hlen[i] + 1);
        suffix[i - hlen[i]] = Math.max(suffix[i - hlen[i]], 2 * hlen[i] + 1);
    }
    for (let i = 1; i < n; ++i) {
        prefix[n - i - 1] = Math.max(prefix[n - i - 1], prefix[n - i] - 2);
        suffix[i] = Math.max(suffix[i], suffix[i - 1] - 2);
    }
    for (let i = 1; i < n; ++i) {
        prefix[i] = Math.max(prefix[i - 1], prefix[i]);
        suffix[n - i - 1] = Math.max(suffix[n - i - 1], suffix[n - i]);
    }
    let ans = 0;
    for (let i = 1; i < n; ++i) {
        ans = Math.max(ans, prefix[i - 1] * suffix[i]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
