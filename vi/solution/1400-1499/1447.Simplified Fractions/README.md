---
comments: true
difficulty: Medium
rating: 1268
source: Biweekly Contest 26 Q2
tags:
    - Math
    - String
    - Greatest Common Divisor
    - Number Theory
    - Euclidean Algorithm
---

<!-- problem:start -->

# [1447. Simplified Fractions](https://leetcode.com/problems/simplified-fractions)

[中文文档](/solution/1400-1499/1447.Simplified%20Fractions/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>, hãy trả về <em>danh sách tất cả các phân số <strong>tối giản</strong> nằm giữa </em><code>0</code><em> và </em><code>1</code><em> (không bao gồm hai đầu mút), trong đó mẫu số nhỏ hơn hoặc bằng </em><code>n</code>. Bạn có thể trả về đáp án theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> [&quot;1/2&quot;]
<strong>Giải thích:</strong> &quot;1/2&quot; là phân số duy nhất có mẫu số nhỏ hơn hoặc bằng 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> [&quot;1/2&quot;,&quot;1/3&quot;,&quot;2/3&quot;]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> [&quot;1/2&quot;,&quot;1/3&quot;,&quot;1/4&quot;,&quot;2/3&quot;,&quot;3/4&quot;]
<strong>Giải thích:</strong> &quot;2/4&quot; không phải là phân số tối giản vì có thể rút gọn thành &quot;1/2&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 100$. Liệt kê $1\le i<j\le n$ và giữ lại cặp khi $\gcd(i,j)=1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def simplifiedFractions(self, n: int) -> List[str]:
        return [
            f'{i}/{j}'
            for i in range(1, n)
            for j in range(i + 1, n + 1)
            if gcd(i, j) == 1
        ]
```

#### Java

```java
class Solution {
    public List<String> simplifiedFractions(int n) {
        List<String> ans = new ArrayList<>();
        for (int i = 1; i < n; ++i) {
            for (int j = i + 1; j < n + 1; ++j) {
                if (gcd(i, j) == 1) {
                    ans.add(i + "/" + j);
                }
            }
        }
        return ans;
    }

    private int gcd(int a, int b) {
        return b > 0 ? gcd(b, a % b) : a;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> simplifiedFractions(int n) {
        vector<string> ans;
        for (int i = 1; i < n; ++i) {
            for (int j = i + 1; j < n + 1; ++j) {
                if (__gcd(i, j) == 1) {
                    ans.push_back(to_string(i) + "/" + to_string(j));
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func simplifiedFractions(n int) (ans []string) {
	for i := 1; i < n; i++ {
		for j := i + 1; j < n+1; j++ {
			if gcd(i, j) == 1 {
				ans = append(ans, strconv.Itoa(i)+"/"+strconv.Itoa(j))
			}
		}
	}
	return ans
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

#### TypeScript

```ts
function simplifiedFractions(n: number): string[] {
    const ans: string[] = [];
    for (let i = 1; i < n; ++i) {
        for (let j = i + 1; j < n + 1; ++j) {
            if (gcd(i, j) === 1) {
                ans.push(`${i}/${j}`);
            }
        }
    }
    return ans;
}

function gcd(a: number, b: number): number {
    return b === 0 ? a : gcd(b, a % b);
}
```

#### Rust

```rust
impl Solution {
    fn gcd(a: i32, b: i32) -> i32 {
        match b {
            0 => a,
            _ => Solution::gcd(b, a % b),
        }
    }

    pub fn simplified_fractions(n: i32) -> Vec<String> {
        let mut res = vec![];
        for i in 1..n {
            for j in i + 1..=n {
                if Solution::gcd(i, j) == 1 {
                    res.push(format!("{}/{}", i, j));
                }
            }
        }
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
