---
comments: true
difficulty: Medium
rating: 1928
source: Weekly Contest 342 Q4
tags:
    - Array
    - Math
    - Greatest Common Divisor
    - Number Theory
    - Euclidean Algorithm
---

<!-- problem:start -->

# [2654. Minimum Number of Operations to Make All Array Elements Equal to 1](https://leetcode.com/problems/minimum-number-of-operations-to-make-all-array-elements-equal-to-1)

[Tài liệu tiếng Trung](/solution/2600-2699/2654.Minimum%20Number%20of%20Operations%20to%20Make%20All%20Array%20Elements%20Equal%20to%201/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> được đánh chỉ số từ <strong>0</strong>, gồm các số nguyên <strong>dương</strong>. Bạn có thể thực hiện thao tác sau trên mảng <strong>bao nhiêu lần cũng được</strong>:</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> sao cho <code>0 &lt;= i &lt; n - 1</code> và thay thế <code>nums[i]</code> hoặc <code>nums[i+1]</code> bằng giá trị gcd của chúng.</li>
</ul>

<p>Trả về <em><strong>số thao tác nhỏ nhất</strong> để biến mọi phần tử của </em><code>nums</code><em> thành </em><code>1</code>. Nếu không thể thực hiện, trả về <code>-1</code>.</p>

<p>Gcd của hai số nguyên là ước chung lớn nhất của hai số đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,6,3,4]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác sau:
- Chọn chỉ số i = 2 và thay thế nums[2] bằng gcd(3,4) = 1. Khi đó nums = [2,6,1,4].
- Chọn chỉ số i = 1 và thay thế nums[1] bằng gcd(6,1) = 1. Khi đó nums = [2,1,1,4].
- Chọn chỉ số i = 0 và thay thế nums[0] bằng gcd(2,1) = 1. Khi đó nums = [1,1,1,4].
- Chọn chỉ số i = 2 và thay thế nums[3] bằng gcd(1,4) = 1. Khi đó nums = [1,1,1,1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,10,6,14]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Có thể chứng minh rằng không thể biến tất cả phần tử thành 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 50</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác ghi đè một phần tử kề bên bằng gcd của hai phần tử. Nếu đã có sẵn một phần tử bằng $1$, mỗi vị trí còn lại trong $n-cnt$ vị trí chỉ cần một lần ghi đè. Nếu chưa có, trước tiên phải tạo ra một phần tử bằng $1$.
>
> Gcd giảm khi xét các đoạn dài hơn; đoạn ngắn nhất có gcd bằng $1$ cần $mi-1$ thao tác, sau đó cần thêm $n-1$ thao tác để lan truyền phần tử bằng $1$ đó.
>
> Vì $n \le 50$, ta có thể duyệt mọi đoạn; nếu gcd của toàn mảng lớn hơn $1$ thì không có lời giải.

<!-- thinking:end -->

Trước tiên, ta đếm số phần tử bằng $1$ trong mảng $nums$ là $cnt$. Nếu $cnt \gt 0$, ta chỉ cần $n - cnt$ thao tác để biến toàn bộ mảng thành $1$.

Nếu không có phần tử nào bằng 1, trước hết ta cần biến một phần tử trong mảng thành $1$, sau đó cần thêm tối thiểu $n - 1$ thao tác.

Hãy xét cách biến một phần tử trong mảng thành $1$ với số thao tác nhỏ nhất. Thực tế, ta chỉ cần tìm một đoạn con liên tiếp nhỏ nhất $nums[i,..j]$ sao cho ước chung lớn nhất của tất cả phần tử trong đoạn con bằng $1$, với độ dài đoạn con là $mi = \min(mi, j - i + 1)$. Cuối cùng, tổng số thao tác là $n - 1 + mi - 1$.

Độ phức tạp thời gian là $O(n \times (n + \log M))$ và độ phức tạp không gian là $O(\log M)$, trong đó $n$ và $M$ lần lượt là độ dài của mảng $nums$ và giá trị lớn nhất trong mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        n = len(nums)
        cnt = nums.count(1)
        if cnt:
            return n - cnt
        mi = n + 1
        for i in range(n):
            g = 0
            for j in range(i, n):
                g = gcd(g, nums[j])
                if g == 1:
                    mi = min(mi, j - i + 1)
        return -1 if mi > n else n - 1 + mi - 1
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums) {
        int n = nums.length;
        int cnt = 0;
        for (int x : nums) {
            if (x == 1) {
                ++cnt;
            }
        }
        if (cnt > 0) {
            return n - cnt;
        }
        int mi = n + 1;
        for (int i = 0; i < n; ++i) {
            int g = 0;
            for (int j = i; j < n; ++j) {
                g = gcd(g, nums[j]);
                if (g == 1) {
                    mi = Math.min(mi, j - i + 1);
                }
            }
        }
        return mi > n ? -1 : n - 1 + mi - 1;
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
    int minOperations(vector<int>& nums) {
        int n = nums.size();
        int cnt = 0;
        for (int x : nums) {
            if (x == 1) {
                ++cnt;
            }
        }
        if (cnt) {
            return n - cnt;
        }
        int mi = n + 1;
        for (int i = 0; i < n; ++i) {
            int g = 0;
            for (int j = i; j < n; ++j) {
                g = gcd(g, nums[j]);
                if (g == 1) {
                    mi = min(mi, j - i + 1);
                }
            }
        }
        return mi > n ? -1 : n - 1 + mi - 1;
    }
};
```

#### Go

```go
func minOperations(nums []int) int {
	n := len(nums)
	cnt := 0
	for _, x := range nums {
		if x == 1 {
			cnt++
		}
	}
	if cnt > 0 {
		return n - cnt
	}
	mi := n + 1
	for i := 0; i < n; i++ {
		g := 0
		for j := i; j < n; j++ {
			g = gcd(g, nums[j])
			if g == 1 {
				mi = min(mi, j-i+1)
			}
		}
	}
	if mi > n {
		return -1
	}
	return n - 1 + mi - 1
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
function minOperations(nums: number[]): number {
    const n = nums.length;
    let cnt = 0;
    for (const x of nums) {
        if (x === 1) {
            ++cnt;
        }
    }
    if (cnt > 0) {
        return n - cnt;
    }
    let mi = n + 1;
    for (let i = 0; i < n; ++i) {
        let g = 0;
        for (let j = i; j < n; ++j) {
            g = gcd(g, nums[j]);
            if (g === 1) {
                mi = Math.min(mi, j - i + 1);
            }
        }
    }
    return mi > n ? -1 : n - 1 + mi - 1;
}

function gcd(a: number, b: number): number {
    return b === 0 ? a : gcd(b, a % b);
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(nums: Vec<i32>) -> i32 {
        let n = nums.len() as i32;
        let cnt = nums.iter().filter(|&&x| x == 1).count() as i32;
        if cnt > 0 {
            return n - cnt;
        }
        let mut mi = n + 1;
        for i in 0..nums.len() {
            let mut g = 0;
            for j in i..nums.len() {
                g = gcd(g, nums[j]);
                if g == 1 {
                    mi = mi.min((j - i + 1) as i32);
                    break;
                }
            }
        }
        if mi > n {
            -1
        } else {
            n - 1 + mi - 1
        }
    }
}

fn gcd(mut a: i32, mut b: i32) -> i32 {
    while b != 0 {
        let tmp = a % b;
        a = b;
        b = tmp;
    }
    a.abs()
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
