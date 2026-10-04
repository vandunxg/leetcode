---
comments: true
difficulty: Easy
rating: 1269
source: Weekly Contest 361 Q1
tags:
    - Math
    - Enumeration
---

<!-- problem:start -->

# [2843. Count Symmetric Integers](https://leetcode.com/problems/count-symmetric-integers)

[中文文档](/solution/2800-2899/2843.Count%20Symmetric%20Integers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên dương <code>low</code> và <code>high</code>.</p>

<p>Một số nguyên <code>x</code> gồm <code>2 * n</code> chữ số được gọi là <strong>đối xứng</strong> nếu tổng của <code>n</code> chữ số đầu tiên của <code>x</code> bằng tổng của <code>n</code> chữ số cuối cùng của <code>x</code>. Các số có số lượng chữ số lẻ không bao giờ đối xứng.</p>

<p>Hãy trả về <em><strong>số lượng số nguyên đối xứng</strong> trong đoạn</em> <code>[low, high]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> low = 1, high = 100
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Có 9 số nguyên đối xứng trong đoạn từ 1 đến 100: 11, 22, 33, 44, 55, 66, 77, 88 và 99.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> low = 1200, high = 1230
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có 4 số nguyên đối xứng trong đoạn từ 1200 đến 1230: 1203, 1212, 1221 và 1230.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= low &lt;= high &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Với $high\le 10^4$, có thể kiểm tra từng số nguyên trong đoạn: số đó phải có số chữ số chẵn và tổng các chữ số ở hai nửa phải bằng nhau. Không cần dùng digit DP.

<!-- thinking:end -->

Ta liệt kê từng số nguyên $x$ trong đoạn $[low, high]$ và kiểm tra xem đó có phải là số đối xứng hay không. Nếu đúng, ta tăng đáp án $ans$ lên $1$.

Độ phức tạp thời gian là $O(n \times \log m)$ và độ phức tạp không gian là $O(\log m)$. Trong đó, $n$ là số lượng số nguyên trong đoạn $[low, high]$, còn $m$ là số nguyên lớn nhất được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSymmetricIntegers(self, low: int, high: int) -> int:
        def f(x: int) -> bool:
            s = str(x)
            if len(s) & 1:
                return False
            n = len(s) // 2
            return sum(map(int, s[:n])) == sum(map(int, s[n:]))

        return sum(f(x) for x in range(low, high + 1))
```

#### Java

```java
class Solution {
    public int countSymmetricIntegers(int low, int high) {
        int ans = 0;
        for (int x = low; x <= high; ++x) {
            ans += f(x);
        }
        return ans;
    }

    private int f(int x) {
        String s = "" + x;
        int n = s.length();
        if (n % 2 == 1) {
            return 0;
        }
        int a = 0, b = 0;
        for (int i = 0; i < n / 2; ++i) {
            a += s.charAt(i) - '0';
        }
        for (int i = n / 2; i < n; ++i) {
            b += s.charAt(i) - '0';
        }
        return a == b ? 1 : 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countSymmetricIntegers(int low, int high) {
        int ans = 0;
        auto f = [](int x) {
            string s = to_string(x);
            int n = s.size();
            if (n & 1) {
                return 0;
            }
            int a = 0, b = 0;
            for (int i = 0; i < n / 2; ++i) {
                a += s[i] - '0';
                b += s[n / 2 + i] - '0';
            }
            return a == b ? 1 : 0;
        };
        for (int x = low; x <= high; ++x) {
            ans += f(x);
        }
        return ans;
    }
};
```

#### Go

```go
func countSymmetricIntegers(low int, high int) (ans int) {
	f := func(x int) int {
		s := strconv.Itoa(x)
		n := len(s)
		if n&1 == 1 {
			return 0
		}
		a, b := 0, 0
		for i := 0; i < n/2; i++ {
			a += int(s[i] - '0')
			b += int(s[n/2+i] - '0')
		}
		if a == b {
			return 1
		}
		return 0
	}
	for x := low; x <= high; x++ {
		ans += f(x)
	}
	return
}
```

#### TypeScript

```ts
function countSymmetricIntegers(low: number, high: number): number {
    let ans = 0;
    const f = (x: number): number => {
        const s = x.toString();
        const n = s.length;
        if (n & 1) {
            return 0;
        }
        let a = 0;
        let b = 0;
        for (let i = 0; i < n >> 1; ++i) {
            a += Number(s[i]);
            b += Number(s[(n >> 1) + i]);
        }
        return a === b ? 1 : 0;
    };
    for (let x = low; x <= high; ++x) {
        ans += f(x);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_symmetric_integers(low: i32, high: i32) -> i32 {
        let mut ans = 0;
        for x in low..=high {
            ans += Self::f(x);
        }
        ans
    }

    fn f(x: i32) -> i32 {
        let s = x.to_string();
        let n = s.len();
        if n % 2 == 1 {
            return 0;
        }
        let bytes = s.as_bytes();
        let mut a = 0;
        let mut b = 0;
        for i in 0..n / 2 {
            a += (bytes[i] - b'0') as i32;
        }
        for i in n / 2..n {
            b += (bytes[i] - b'0') as i32;
        }
        if a == b { 1 } else { 0 }
    }
}
```

#### C#

```cs
public class Solution {
    public int CountSymmetricIntegers(int low, int high) {
        int ans = 0;
        for (int x = low; x <= high; ++x) {
            ans += f(x);
        }
        return ans;
    }

    private int f(int x) {
        string s = x.ToString();
        int n = s.Length;
        if (n % 2 == 1) {
            return 0;
        }
        int a = 0, b = 0;
        for (int i = 0; i < n / 2; ++i) {
            a += s[i] - '0';
        }
        for (int i = n / 2; i < n; ++i) {
            b += s[i] - '0';
        }
        return a == b ? 1 : 0;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
