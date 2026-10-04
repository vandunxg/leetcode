---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Array
    - Math
    - Dynamic Programming
    - Backtracking
    - Bitmask
    - Number Theory
---

<!-- problem:start -->

# [2992. Number of Self-Divisible Permutations 🔒](https://leetcode.com/problems/number-of-self-divisible-permutations)

[中文文档](/solution/2900-2999/2992.Number%20of%20Self-Divisible%20Permutations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>, hãy trả về <em>số <strong>hoán vị</strong> của <strong>mảng được đánh chỉ số từ 1</strong></em> <code>nums = [1, 2, ..., n]</code><em> sao cho mảng đó <strong>tự chia hết</strong></em>.</p>

<p>Một mảng <code>a</code> có độ dài <code>n</code>, được <strong>đánh chỉ số từ 1</strong>, được gọi là <strong>tự chia hết</strong> nếu với mọi <code>1 &lt;= i &lt;= n</code>, <code><span data-keyword="gcd-function">gcd</span>(a[i], i) == 1</code>.</p>

<p><strong>Hoán vị</strong> của một mảng là sự sắp xếp lại các phần tử của mảng đó. Ví dụ, dưới đây là tất cả các hoán vị của mảng <code>[1, 2, 3]</code>:</p>

<ul>
	<li><code>[1, 2, 3]</code></li>
	<li><code>[1, 3, 2]</code></li>
	<li><code>[2, 1, 3]</code></li>
	<li><code>[2, 3, 1]</code></li>
	<li><code>[3, 1, 2]</code></li>
	<li><code>[3, 2, 1]</code></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Mảng [1] chỉ có 1 hoán vị tự chia hết.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Mảng [1,2] có 2 hoán vị và chỉ một trong số đó tự chia hết:
nums = [1,2]: Hoán vị này không tự chia hết vì gcd(nums[2], 2) != 1.
nums = [2,1]: Hoán vị này tự chia hết vì gcd(nums[1], 1) == 1 và gcd(nums[2], 2) == 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Mảng [1,2,3] có 3 hoán vị tự chia hết: [1,3,2], [3,1,2], [2,3,1].
Có thể chứng minh rằng 3 hoán vị còn lại không tự chia hết. Vì vậy, đáp án là 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 12</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nén trạng thái + Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Một hoán vị phải thỏa mãn $\gcd(i, perm_i)=1$. Vì $n$ nhỏ, ta dùng bit mask để ghi lại các giá trị đã sử dụng. $dfs(mask)$ điền vị trí $bit\_count+1$ bằng một $j$ chưa được sử dụng và nguyên tố cùng nhau với vị trí đó.
>
> Với $2^n$ trạng thái và tối đa $n$ lựa chọn ở mỗi trạng thái, giới hạn này phù hợp.

<!-- thinking:end -->

Ta có thể dùng một số nhị phân $mask$ để biểu diễn trạng thái hoán vị hiện tại. Bit thứ $i$ bằng $1$ cho biết số $i$ đã được sử dụng, còn bằng $0$ cho biết số $i$ chưa được sử dụng.

Sau đó, ta xây dựng hàm $dfs(mask)$ biểu diễn số hoán vị có thể tạo ra từ trạng thái hoán vị hiện tại $mask$ và thỏa mãn yêu cầu của đề bài. Đáp án là $dfs(0)$.

Ta có thể dùng phương pháp tìm kiếm có ghi nhớ để tính giá trị của $dfs(mask)$.

Trong quá trình tính $dfs(mask)$, ta dùng $i$ để biểu diễn số sẽ được thêm vào hoán vị. Nếu $i \gt n$, điều đó có nghĩa là hoán vị đã được tạo xong, khi đó ta trả về $1$.

Nếu không, ta liệt kê các số $j$ chưa được sử dụng trong hoán vị hiện tại. Nếu $i$ và $j$ thỏa mãn yêu cầu của đề bài, ta có thể thêm $j$ vào hoán vị. Khi đó, trạng thái trở thành $mask \mid 2^j$, trong đó $|$ biểu diễn phép OR theo bit. Vì $j$ đã được sử dụng, ta cần đệ quy tính giá trị của $dfs(mask \mid 2^j)$ rồi cộng giá trị đó vào $dfs(mask)$.

Cuối cùng, ta nhận được giá trị của $dfs(0)$, đây chính là đáp án.

Độ phức tạp thời gian là $O(n \times 2^n)$, và độ phức tạp không gian là $O(2^n)$. Trong đó, $n$ là độ dài của hoán vị.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def selfDivisiblePermutationCount(self, n: int) -> int:
        @cache
        def dfs(mask: int) -> int:
            i = mask.bit_count() + 1
            if i > n:
                return 1
            ans = 0
            for j in range(1, n + 1):
                if (mask >> j & 1) == 0 and gcd(i, j) == 1:
                    ans += dfs(mask | 1 << j)
            return ans

        return dfs(0)
```

#### Java

```java
class Solution {
    private int n;
    private Integer[] f;

    public int selfDivisiblePermutationCount(int n) {
        this.n = n;
        f = new Integer[1 << (n + 1)];
        return dfs(0);
    }

    private int dfs(int mask) {
        if (f[mask] != null) {
            return f[mask];
        }
        int i = Integer.bitCount(mask) + 1;
        if (i > n) {
            return 1;
        }
        f[mask] = 0;
        for (int j = 1; j <= n; ++j) {
            if ((mask >> j & 1) == 0 && gcd(i, j) == 1) {
                f[mask] += dfs(mask | 1 << j);
            }
        }
        return f[mask];
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
    int selfDivisiblePermutationCount(int n) {
        int f[1 << (n + 1)];
        memset(f, -1, sizeof(f));
        function<int(int)> dfs = [&](int mask) {
            if (f[mask] != -1) {
                return f[mask];
            }
            int i = __builtin_popcount(mask) + 1;
            if (i > n) {
                return 1;
            }
            f[mask] = 0;
            for (int j = 1; j <= n; ++j) {
                if ((mask >> j & 1) == 0 && __gcd(i, j) == 1) {
                    f[mask] += dfs(mask | 1 << j);
                }
            }
            return f[mask];
        };
        return dfs(0);
    }
};
```

#### Go

```go
func selfDivisiblePermutationCount(n int) int {
	f := make([]int, 1<<(n+1))
	for i := range f {
		f[i] = -1
	}
	var dfs func(int) int
	dfs = func(mask int) int {
		if f[mask] != -1 {
			return f[mask]
		}
		i := bits.OnesCount(uint(mask)) + 1
		if i > n {
			return 1
		}
		f[mask] = 0
		for j := 1; j <= n; j++ {
			if mask>>j&1 == 0 && gcd(i, j) == 1 {
				f[mask] += dfs(mask | 1<<j)
			}
		}
		return f[mask]
	}
	return dfs(0)
}

func gcd(a, b int) int {
	for b != 0 {
		a, b = b, a%b
	}
	return a
}
```

#### TypeScript

```ts
function selfDivisiblePermutationCount(n: number): number {
    const f: number[] = Array(1 << (n + 1)).fill(-1);
    const dfs = (mask: number): number => {
        if (f[mask] !== -1) {
            return f[mask];
        }
        const i = bitCount(mask) + 1;
        if (i > n) {
            return 1;
        }
        f[mask] = 0;
        for (let j = 1; j <= n; ++j) {
            if (((mask >> j) & 1) === 0 && gcd(i, j) === 1) {
                f[mask] += dfs(mask | (1 << j));
            }
        }
        return f[mask];
    };
    return dfs(0);
}

function gcd(a: number, b: number): number {
    while (b !== 0) {
        const t = a % b;
        a = b;
        b = t;
    }
    return a;
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Nén trạng thái + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 đệ quy trên tập hợp các giá trị đã sử dụng. Tương đương, $f[mask]$ đếm số cách đạt được tập hợp đó, bằng cách chuyển trạng thái qua việc thêm một $j$ chưa sử dụng và nguyên tố cùng nhau với vị trí của nó. Bắt đầu từ $f[0]=1$ và lấy $f[2^n-1]$.
>
> Chỉ có thứ tự tính toán thay đổi; đệ quy được loại bỏ.

<!-- thinking:end -->

Ta có thể viết lại phương pháp tìm kiếm có ghi nhớ trong Lời giải 1 dưới dạng quy hoạch động. Đặt $f[mask]$ là số hoán vị mà trạng thái hoán vị hiện tại là $mask$ và thỏa mãn yêu cầu của đề bài. Ban đầu, $f[0]=1$, các giá trị còn lại bằng $0$.

Ta liệt kê $mask$ trong khoảng $[0, 2^n)$. Với mỗi $mask$, ta dùng $i$ để biểu diễn vị trí của phần tử cuối cùng được thêm vào hoán vị, sau đó liệt kê phần tử cuối cùng $j$ được thêm vào hoán vị hiện tại. Nếu $i$ và $j$ thỏa mãn yêu cầu của đề bài, trạng thái $f[mask]$ có thể được chuyển từ trạng thái $f[mask \oplus 2^(j-1)]$, trong đó $\oplus$ biểu diễn phép XOR theo bit. Ta cộng tất cả giá trị của trạng thái chuyển tiếp $f[mask \oplus 2^(j-1)]$ vào $f[mask]$, đó là giá trị của $f[mask]$.

Cuối cùng, ta nhận được giá trị của $f[2^n - 1]$, đây chính là đáp án.

Độ phức tạp thời gian là $O(n \times 2^n)$, và độ phức tạp không gian là $O(2^n)$. Trong đó, $n$ là độ dài của hoán vị.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def selfDivisiblePermutationCount(self, n: int) -> int:
        f = [0] * (1 << n)
        f[0] = 1
        for mask in range(1 << n):
            i = mask.bit_count()
            for j in range(1, n + 1):
                if (mask >> (j - 1) & 1) == 1 and gcd(i, j) == 1:
                    f[mask] += f[mask ^ (1 << (j - 1))]
        return f[-1]
```

#### Java

```java
class Solution {
    public int selfDivisiblePermutationCount(int n) {
        int[] f = new int[1 << n];
        f[0] = 1;
        for (int mask = 0; mask < 1 << n; ++mask) {
            int i = Integer.bitCount(mask);
            for (int j = 1; j <= n; ++j) {
                if (((mask >> (j - 1)) & 1) == 1 && gcd(i, j) == 1) {
                    f[mask] += f[mask ^ (1 << (j - 1))];
                }
            }
        }
        return f[(1 << n) - 1];
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
    int selfDivisiblePermutationCount(int n) {
        int f[1 << n];
        memset(f, 0, sizeof(f));
        f[0] = 1;
        for (int mask = 0; mask < 1 << n; ++mask) {
            int i = __builtin_popcount(mask);
            for (int j = 1; j <= n; ++j) {
                if (((mask >> (j - 1)) & 1) == 1 && __gcd(i, j) == 1) {
                    f[mask] += f[mask ^ (1 << (j - 1))];
                }
            }
        }
        return f[(1 << n) - 1];
    }
};
```

#### Go

```go
func selfDivisiblePermutationCount(n int) int {
	f := make([]int, 1<<n)
	f[0] = 1
	for mask := 0; mask < 1<<n; mask++ {
		i := bits.OnesCount(uint(mask))
		for j := 1; j <= n; j++ {
			if mask>>(j-1)&1 == 1 && gcd(i, j) == 1 {
				f[mask] += f[mask^(1<<(j-1))]
			}
		}
	}
	return f[(1<<n)-1]
}

func gcd(a, b int) int {
	for b != 0 {
		a, b = b, a%b
	}
	return a
}
```

#### TypeScript

```ts
function selfDivisiblePermutationCount(n: number): number {
    const f: number[] = Array(1 << n).fill(0);
    f[0] = 1;
    for (let mask = 0; mask < 1 << n; ++mask) {
        const i = bitCount(mask);
        for (let j = 1; j <= n; ++j) {
            if ((mask >> (j - 1)) & 1 && gcd(i, j) === 1) {
                f[mask] += f[mask ^ (1 << (j - 1))];
            }
        }
    }
    return f.at(-1)!;
}

function gcd(a: number, b: number): number {
    while (b !== 0) {
        const t = a % b;
        a = b;
        b = t;
    }
    return a;
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
