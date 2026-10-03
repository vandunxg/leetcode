---
comments: true
difficulty: Easy
rating: 1172
source: Weekly Contest 313 Q1
tags:
    - Math
    - Enumeration
    - Greatest Common Divisor
    - Number Theory
    - Euclidean Algorithm
---

<!-- problem:start -->

# [2427. Number of Common Factors](https://leetcode.com/problems/number-of-common-factors)

[中文文档](/solution/2400-2499/2427.Number%20of%20Common%20Factors/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên dương <code>a</code> và <code>b</code>, hãy trả về <em>số lượng <strong>ước chung</strong> của </em><code>a</code><em> và </em><code>b</code>.</p>

<p>Một số nguyên <code>x</code> là <strong>ước chung</strong> của <code>a</code> và <code>b</code> nếu <code>x</code> chia hết cả <code>a</code> và <code>b</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 12, b = 6
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các ước chung của 12 và 6 là 1, 2, 3, 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 25, b = 30
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các ước chung của 25 và 30 là 1, 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= a, b &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Mọi ước chung đều là ước của $g=\gcd(a,b)$. Với $a,b\le 1000$, ta kiểm tra từng số nguyên trong đoạn $[1,g]$ với $g$; không cần kiểm tra riêng $a$ và $b$.

<!-- thinking:end -->

Trước tiên, ta có thể tính ước chung lớn nhất $g$ của $a$ và $b$, sau đó duyệt từng số trong đoạn $[1,..g]$, kiểm tra xem nó có phải là ước của $g$ hay không; nếu đúng thì tăng đáp án lên một.

Độ phức tạp thời gian là $O(\min(a, b))$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def commonFactors(self, a: int, b: int) -> int:
        g = gcd(a, b)
        return sum(g % x == 0 for x in range(1, g + 1))
```

#### Java

```java
class Solution {
    public int commonFactors(int a, int b) {
        int g = gcd(a, b);
        int ans = 0;
        for (int x = 1; x <= g; ++x) {
            if (g % x == 0) {
                ++ans;
            }
        }
        return ans;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int commonFactors(int a, int b) {
        int g = gcd(a, b);
        int ans = 0;
        for (int x = 1; x <= g; ++x) {
            ans += g % x == 0;
        }
        return ans;
    }
};
```

#### Go

```go
func commonFactors(a int, b int) (ans int) {
	g := gcd(a, b)
	for x := 1; x <= g; x++ {
		if g%x == 0 {
			ans++
		}
	}
	return
}

func gcd(a int, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

#### TypeScript

```ts
function commonFactors(a: number, b: number): number {
    const g = gcd(a, b);
    let ans = 0;
    for (let x = 1; x <= g; ++x) {
        if (g % x === 0) {
            ++ans;
        }
    }
    return ans;
}

function gcd(a: number, b: number): number {
    return b === 0 ? a : gcd(b, a % b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Duyệt tối ưu

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 đã duyệt đến $g$. Các ước xuất hiện theo cặp, vì vậy ta chỉ cần duyệt đến $\sqrt{g}$ và đếm cả $x$ và $g/x$ (chỉ đếm một lần khi chúng trùng nhau), với độ phức tạp $O(\sqrt{g})$.

<!-- thinking:end -->

Tương tự Lời giải 1, trước tiên ta có thể tính ước chung lớn nhất $g$ của $a$ và $b$, sau đó duyệt tất cả các ước của ước chung lớn nhất $g$ và cộng dồn đáp án.

Độ phức tạp thời gian là $O(\sqrt{\min(a, b)})$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def commonFactors(self, a: int, b: int) -> int:
        g = gcd(a, b)
        ans, x = 0, 1
        while x * x <= g:
            if g % x == 0:
                ans += 1
                ans += x * x < g
            x += 1
        return ans
```

#### Java

```java
class Solution {
    public int commonFactors(int a, int b) {
        int g = gcd(a, b);
        int ans = 0;
        for (int x = 1; x * x <= g; ++x) {
            if (g % x == 0) {
                ++ans;
                if (x * x < g) {
                    ++ans;
                }
            }
        }
        return ans;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int commonFactors(int a, int b) {
        int g = gcd(a, b);
        int ans = 0;
        for (int x = 1; x * x <= g; ++x) {
            if (g % x == 0) {
                ans++;
                ans += x * x < g;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func commonFactors(a int, b int) (ans int) {
	g := gcd(a, b)
	for x := 1; x*x <= g; x++ {
		if g%x == 0 {
			ans++
			if x*x < g {
				ans++
			}
		}
	}
	return
}

func gcd(a int, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

#### TypeScript

```ts
function commonFactors(a: number, b: number): number {
    const g = gcd(a, b);
    let ans = 0;
    for (let x = 1; x * x <= g; ++x) {
        if (g % x === 0) {
            ++ans;
            if (x * x < g) {
                ++ans;
            }
        }
    }
    return ans;
}

function gcd(a: number, b: number): number {
    return b === 0 ? a : gcd(b, a % b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
