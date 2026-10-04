---
comments: true
difficulty: Medium
rating: 1294
source: Weekly Contest 393 Q2
tags:
    - Array
    - Math
    - Number Theory
    - Primality Test
---

<!-- problem:start -->

# [3115. Maximum Prime Difference](https://leetcode.com/problems/maximum-prime-difference)

[中文文档](/solution/3100-3199/3115.Maximum%20Prime%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Trả về một số nguyên là khoảng cách <strong>lớn nhất</strong> giữa <strong>chỉ số</strong> của hai số nguyên tố (không nhất thiết khác nhau) trong <code>nums</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,2,9,5,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong> <code>nums[1]</code>, <code>nums[3]</code> và <code>nums[4]</code> là các số nguyên tố. Vì vậy, đáp án là <code>|4 - 1| = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,8,2,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong> <code>nums[2]</code> là số nguyên tố. Vì chỉ có một số nguyên tố nên đáp án là <code>|2 - 2| = 0</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 3 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
	<li>Đầu vào được tạo sao cho số lượng số nguyên tố trong <code>nums</code> ít nhất là một.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Đáp án là khoảng cách chỉ số giữa số nguyên tố ngoài cùng bên trái và ngoài cùng bên phải. Việc so sánh mọi cặp số nguyên tố chỉ là tìm lại hai đầu mút đó.
>
> Việc kiểm tra một giá trị có phải số nguyên tố có độ phức tạp là $O(\sqrt{M})$; vì cả độ dài và độ lớn đều vừa phải, chỉ cần xác định hai đầu mút.
>
> Duyệt từ trái để tìm chỉ số số nguyên tố đầu tiên $i$ và từ phải để tìm chỉ số số nguyên tố cuối cùng $j$, sau đó trả về $j-i$. Các giá trị ở giữa không ảnh hưởng đến khoảng cách.

<!-- thinking:end -->

Theo mô tả bài toán, chúng ta cần tìm chỉ số $i$ của số nguyên tố đầu tiên, sau đó tìm chỉ số $j$ của số nguyên tố cuối cùng và trả về $j - i$ làm đáp án.

Do đó, chúng ta có thể duyệt mảng từ trái sang phải để tìm chỉ số $i$ của số nguyên tố đầu tiên, sau đó duyệt mảng từ phải sang trái để tìm chỉ số $j$ của số nguyên tố cuối cùng. Đáp án là $j - i$.

Độ phức tạp thời gian là $O(n \times \sqrt{M})$, trong đó $n$ và $M$ lần lượt là độ dài của mảng $nums$ và giá trị lớn nhất trong mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumPrimeDifference(self, nums: List[int]) -> int:
        def is_prime(x: int) -> bool:
            if x < 2:
                return False
            return all(x % i for i in range(2, int(sqrt(x)) + 1))

        for i, x in enumerate(nums):
            if is_prime(x):
                for j in range(len(nums) - 1, i - 1, -1):
                    if is_prime(nums[j]):
                        return j - i
```

#### Java

```java
class Solution {
    public int maximumPrimeDifference(int[] nums) {
        for (int i = 0;; ++i) {
            if (isPrime(nums[i])) {
                for (int j = nums.length - 1;; --j) {
                    if (isPrime(nums[j])) {
                        return j - i;
                    }
                }
            }
        }
    }

    private boolean isPrime(int x) {
        if (x < 2) {
            return false;
        }
        for (int v = 2; v * v <= x; ++v) {
            if (x % v == 0) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumPrimeDifference(vector<int>& nums) {
        for (int i = 0;; ++i) {
            if (isPrime(nums[i])) {
                for (int j = nums.size() - 1;; --j) {
                    if (isPrime(nums[j])) {
                        return j - i;
                    }
                }
            }
        }
    }

    bool isPrime(int n) {
        if (n < 2) {
            return false;
        }
        for (int i = 2; i <= n / i; ++i) {
            if (n % i == 0) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func maximumPrimeDifference(nums []int) int {
	for i := 0; ; i++ {
		if isPrime(nums[i]) {
			for j := len(nums) - 1; ; j-- {
				if isPrime(nums[j]) {
					return j - i
				}
			}
		}
	}
}

func isPrime(n int) bool {
	if n < 2 {
		return false
	}
	for i := 2; i <= n/i; i++ {
		if n%i == 0 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function maximumPrimeDifference(nums: number[]): number {
    const isPrime = (x: number): boolean => {
        if (x < 2) {
            return false;
        }
        for (let i = 2; i <= x / i; i++) {
            if (x % i === 0) {
                return false;
            }
        }
        return true;
    };
    for (let i = 0; ; ++i) {
        if (isPrime(nums[i])) {
            for (let j = nums.length - 1; ; --j) {
                if (isPrime(nums[j])) {
                    return j - i;
                }
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
