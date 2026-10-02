---
comments: true
difficulty: Hard
tags:
    - Math
    - String
    - Enumeration
---

<!-- problem:start -->

# [906. Super Palindromes](https://leetcode.com/problems/super-palindromes)

[中文文档](/solution/0900-0999/0906.Super%20Palindromes/README.md)

## Mô tả

<!-- description:start -->

<p>Một số nguyên dương được gọi là <strong>siêu palindrome</strong> nếu bản thân nó là palindrome và đồng thời là bình phương của một palindrome.</p>

<p>Cho hai số nguyên dương <code>left</code> và <code>right</code> được biểu diễn dưới dạng chuỗi, hãy trả về <em>số lượng số nguyên là </em><strong>siêu palindrome</strong><em> trong đoạn đóng </em><code>[left, right]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> left = &quot;4&quot;, right = &quot;1000&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích</strong>: 4, 9, 121 và 484 là các số siêu palindrome.
Lưu ý 676 không phải số siêu palindrome: 26 * 26 = 676, nhưng 26 không phải palindrome.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> left = &quot;1&quot;, right = &quot;2&quot;
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= left.length, right.length &lt;= 18</code></li>
	<li><code>left</code> và <code>right</code> chỉ gồm các chữ số.</li>
	<li><code>left</code> và <code>right</code> không có số 0 ở đầu.</li>
	<li><code>left</code> và <code>right</code> biểu diễn các số nguyên trong khoảng <code>[1, 10<sup>18</sup> - 1]</code>.</li>
	<li><code>left</code> nhỏ hơn hoặc bằng <code>right</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Số siêu palindrome có dạng $x=p^2$ trong đoạn $[L,R]\subseteq[1,10^{18})$. Không thể kiểm tra mọi số chính phương trong khoảng này. Bản thân $p$ phải là palindrome nhỏ hơn $10^9$, nên nửa đầu của nó có độ dài tối đa khoảng $10^5$.
>
> Tiền xử lý mọi palindrome tạo bằng cách đối xứng một tiền tố (có hoặc không có chữ số ở giữa), sau đó giữ lại các giá trị $p^2$ nằm trong $[L,R]$ và cũng là palindrome.

<!-- thinking:end -->

Theo đề bài, số siêu palindrome $x = p^2 \in [1, 10^{18})$, trong đó $p$ là một palindrome, nên $p \in [1, 10^9)$. Ta có thể liệt kê nửa đầu của palindrome $p$, đảo ngược phần đó rồi nối lại để tạo ra mọi palindrome; lưu chúng vào mảng $ps$.

Tiếp theo, ta duyệt mảng $ps$. Với mỗi $p$, tính $p^2$, kiểm tra xem giá trị đó có nằm trong đoạn $[L, R]$ hay không và có phải palindrome hay không. Nếu thỏa mãn, tăng đáp án thêm một.

Duyệt xong thì trả về đáp án.

Độ phức tạp thời gian là $O(M^{\frac{1}{4}} \times \log M)$ và độ phức tạp không gian là $O(M^{\frac{1}{4}})$, trong đó $M$ là cận trên của $L$ và $R$; ở bài này, $M \leq 10^{18}$.

Bài toán tương tự:

- [2967. Minimum Cost to Make Array Equalindromic](https://github.com/doocs/leetcode/blob/main/solution/2900-2999/2967.Minimum%20Cost%20to%20Make%20Array%20Equalindromic/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
ps = []
for i in range(1, 10**5 + 1):
    s = str(i)
    t1 = s[::-1]
    t2 = s[:-1][::-1]
    ps.append(int(s + t1))
    ps.append(int(s + t2))


class Solution:
    def superpalindromesInRange(self, left: str, right: str) -> int:
        def is_palindrome(x: int) -> bool:
            y, t = 0, x
            while t:
                y = y * 10 + t % 10
                t //= 10
            return x == y

        l, r = int(left), int(right)
        return sum(l <= x <= r and is_palindrome(x) for x in map(lambda x: x * x, ps))
```

#### Java

```java
class Solution {
    private static long[] ps;

    static {
        ps = new long[2 * (int) 1e5];
        for (int i = 1; i <= 1e5; i++) {
            String s = Integer.toString(i);
            String t1 = new StringBuilder(s).reverse().toString();
            String t2 = new StringBuilder(s.substring(0, s.length() - 1)).reverse().toString();
            ps[2 * i - 2] = Long.parseLong(s + t1);
            ps[2 * i - 1] = Long.parseLong(s + t2);
        }
    }

    public int superpalindromesInRange(String left, String right) {
        long l = Long.parseLong(left);
        long r = Long.parseLong(right);
        int ans = 0;
        for (long p : ps) {
            long x = p * p;
            if (x >= l && x <= r && isPalindrome(x)) {
                ++ans;
            }
        }
        return ans;
    }

    private boolean isPalindrome(long x) {
        long y = 0;
        for (long t = x; t > 0; t /= 10) {
            y = y * 10 + t % 10;
        }
        return x == y;
    }
}
```

#### C++

```cpp
using ll = unsigned long long;

ll ps[2 * 100000];

int init = [] {
    for (int i = 1; i <= 100000; i++) {
        string s = to_string(i);
        string t1 = s;
        reverse(t1.begin(), t1.end());
        string t2 = s.substr(0, s.length() - 1);
        reverse(t2.begin(), t2.end());
        ps[2 * i - 2] = stoll(s + t1);
        ps[2 * i - 1] = stoll(s + t2);
    }
    return 0;
}();

class Solution {
public:
    int superpalindromesInRange(string left, string right) {
        ll l = stoll(left), r = stoll(right);
        int ans = 0;
        for (ll p : ps) {
            ll x = p * p;
            if (x >= l && x <= r && is_palindrome(x)) {
                ++ans;
            }
        }
        return ans;
    }

    bool is_palindrome(ll x) {
        ll y = 0;
        for (ll t = x; t; t /= 10) {
            y = y * 10 + t % 10;
        }
        return x == y;
    }
};
```

#### Go

```go
var ps [2 * 100000]int64

func init() {
	for i := 1; i <= 100000; i++ {
		s := strconv.Itoa(i)
		t1 := reverseString(s)
		t2 := reverseString(s[:len(s)-1])
		ps[2*i-2], _ = strconv.ParseInt(s+t1, 10, 64)
		ps[2*i-1], _ = strconv.ParseInt(s+t2, 10, 64)
	}
}

func reverseString(s string) string {
	cs := []rune(s)
	for i, j := 0, len(cs)-1; i < j; i, j = i+1, j-1 {
		cs[i], cs[j] = cs[j], cs[i]
	}
	return string(cs)
}

func superpalindromesInRange(left string, right string) (ans int) {
	l, _ := strconv.ParseInt(left, 10, 64)
	r, _ := strconv.ParseInt(right, 10, 64)
	isPalindrome := func(x int64) bool {
		var y int64
		for t := x; t > 0; t /= 10 {
			y = y*10 + int64(t%10)
		}
		return x == y
	}
	for _, p := range ps {
		x := p * p
		if x >= l && x <= r && isPalindrome(x) {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
const ps = Array(2e5).fill(0);

const init = (() => {
    for (let i = 1; i <= 1e5; ++i) {
        const s: string = i.toString();
        const t1: string = s.split('').reverse().join('');
        const t2: string = s.slice(0, -1).split('').reverse().join('');
        ps[2 * i - 2] = parseInt(s + t1, 10);
        ps[2 * i - 1] = parseInt(s + t2, 10);
    }
})();

function superpalindromesInRange(left: string, right: string): number {
    const l = BigInt(left);
    const r = BigInt(right);
    const isPalindrome = (x: bigint): boolean => {
        const s: string = x.toString();
        return s === s.split('').reverse().join('');
    };
    let ans = 0;
    for (const p of ps) {
        const x = BigInt(p) * BigInt(p);
        if (x >= l && x <= r && isPalindrome(x)) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
