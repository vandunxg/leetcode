---
comments: true
difficulty: Medium
rating: 1478
source: Weekly Contest 181 Q2
tags:
    - Array
    - Math
    - Sieve
    - Prime Factorization
---

<!-- problem:start -->

# [1390. Four Divisors](https://leetcode.com/problems/four-divisors)

[中文文档](/solution/1300-1399/1390.Four%20Divisors/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>. Hãy trả về <em>tổng các ước của những số trong mảng có đúng bốn ước</em>. Nếu không có số nào như vậy, trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [21,4,7]
<strong>Đầu ra:</strong> 32
<strong>Giải thích:</strong> 
21 có 4 ước: 1, 3, 7, 21
4 có 3 ước: 1, 2, 4
7 có 2 ước: 1, 7
Đáp án là tổng các ước của riêng 21.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [21,21]
<strong>Đầu ra:</strong> 64
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích thừa số

<!-- thinking:start -->

> **Tư duy**
>
> Tính tổng các ước của mỗi số có đúng bốn ước. Vì giá trị không vượt quá $10^5$, ta có thể thử chia đến $\sqrt{x}$ để liệt kê tất cả ước. Vừa đếm vừa cộng các ước, rồi chỉ giữ tổng nếu số lượng ước bằng $4$.

<!-- thinking:end -->

Ta phân tích thừa số từng số. Nếu số đó có đúng $4$ ước thì thỏa mãn yêu cầu, và ta cộng các ước của nó vào đáp án.

Độ phức tạp thời gian là $O(n \times \sqrt{n})$, với $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumFourDivisors(self, nums: List[int]) -> int:
        def f(x: int) -> int:
            i = 2
            cnt, s = 2, x + 1
            while i <= x // i:
                if x % i == 0:
                    cnt += 1
                    s += i
                    if i * i != x:
                        cnt += 1
                        s += x // i
                i += 1
            return s if cnt == 4 else 0

        return sum(f(x) for x in nums)
```

#### Java

```java
class Solution {
    public int sumFourDivisors(int[] nums) {
        int ans = 0;
        for (int x : nums) {
            ans += f(x);
        }
        return ans;
    }

    private int f(int x) {
        int cnt = 2, s = x + 1;
        for (int i = 2; i <= x / i; ++i) {
            if (x % i == 0) {
                ++cnt;
                s += i;
                if (i * i != x) {
                    ++cnt;
                    s += x / i;
                }
            }
        }
        return cnt == 4 ? s : 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumFourDivisors(vector<int>& nums) {
        int ans = 0;
        for (int x : nums) {
            ans += f(x);
        }
        return ans;
    }

    int f(int x) {
        int cnt = 2, s = x + 1;
        for (int i = 2; i <= x / i; ++i) {
            if (x % i == 0) {
                ++cnt;
                s += i;
                if (i * i != x) {
                    ++cnt;
                    s += x / i;
                }
            }
        }
        return cnt == 4 ? s : 0;
    }
};
```

#### Go

```go
func sumFourDivisors(nums []int) (ans int) {
	f := func(x int) int {
		cnt, s := 2, x+1
		for i := 2; i <= x/i; i++ {
			if x%i == 0 {
				cnt++
				s += i
				if i*i != x {
					cnt++
					s += x / i
				}
			}
		}
		if cnt == 4 {
			return s
		}
		return 0
	}
	for _, x := range nums {
		ans += f(x)
	}
	return
}
```

#### TypeScript

```ts
function sumFourDivisors(nums: number[]): number {
    const f = (x: number): number => {
        let cnt = 2;
        let s = x + 1;
        for (let i = 2; i * i <= x; ++i) {
            if (x % i === 0) {
                ++cnt;
                s += i;
                if (i * i !== x) {
                    ++cnt;
                    s += Math.floor(x / i);
                }
            }
        }
        return cnt === 4 ? s : 0;
    };
    return nums.reduce((acc, x) => acc + f(x), 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_four_divisors(nums: Vec<i32>) -> i32 {
        let f = |x: i32| -> i32 {
            let mut cnt = 2;
            let mut s = x + 1;
            let mut i = 2;
            while i <= x / i {
                if x % i == 0 {
                    cnt += 1;
                    s += i;
                    if i * i != x {
                        cnt += 1;
                        s += x / i;
                    }
                }
                i += 1;
            }
            if cnt == 4 {
                s
            } else {
                0
            }
        };
        let mut ans = 0;
        for x in nums {
            ans += f(x);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
