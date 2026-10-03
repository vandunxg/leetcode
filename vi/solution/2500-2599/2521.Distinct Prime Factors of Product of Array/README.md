---
comments: true
difficulty: Medium
rating: 1413
source: Weekly Contest 326 Q2
tags:
    - Array
    - Hash Table
    - Math
    - Greatest Common Divisor
    - Number Theory
    - Primality Test
    - Sieve
    - Sieve of Eratosthenes
    - Prime Factorization
    - Euclidean Algorithm
---

<!-- problem:start -->

# [2521. Distinct Prime Factors of Product of Array](https://leetcode.com/problems/distinct-prime-factors-of-product-of-array)

[中文文档](/solution/2500-2599/2521.Distinct%20Prime%20Factors%20of%20Product%20of%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng các số nguyên dương <code>nums</code>, hãy trả về <em>số lượng <strong>thừa số nguyên tố phân biệt</strong> trong tích các phần tử của</em> <code>nums</code>.</p>

<p><strong>Lưu ý</strong> rằng:</p>

<ul>
	<li>Một số lớn hơn <code>1</code> được gọi là <strong>số nguyên tố</strong> nếu nó chỉ chia hết cho <code>1</code> và chính nó.</li>
	<li>Một số nguyên <code>val1</code> là thừa số của một số nguyên khác <code>val2</code> nếu <code>val2 / val1</code> là một số nguyên.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,3,7,10,6]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Tích của tất cả các phần tử trong nums là: 2 * 4 * 3 * 7 * 10 * 6 = 10080 = 2<sup>5</sup> * 3<sup>2</sup> * 5 * 7.
Có 4 thừa số nguyên tố phân biệt, nên ta trả về 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,8,16]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Tích của tất cả các phần tử trong nums là: 2 * 4 * 8 * 16 = 1024 = 2<sup>10</sup>.
Có 1 thừa số nguyên tố phân biệt, nên ta trả về 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>2 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Phân tích thừa số nguyên tố

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số lượng thừa số nguyên tố phân biệt của $\prod nums[i]$. Việc tính tích trước sẽ gây tràn số và không cần thiết: các thừa số nguyên tố của tích chính là hợp của các thừa số nguyên tố trong từng phần tử.
>
> Ta chia thử từng $n$, đưa các thừa số vào một set, rồi trả về kích thước của set. Mỗi lần phân tích thừa số có độ phức tạp $O(\sqrt{m})$.

<!-- thinking:end -->

Với mỗi phần tử trong mảng, trước tiên ta phân tích nó thành các thừa số nguyên tố, sau đó thêm các thừa số nguyên tố thu được vào hash table. Cuối cùng, trả về kích thước của hash table.

Độ phức tạp thời gian là $O(n \times \sqrt{m})$, và độ phức tạp không gian là $O(\frac{m}{\log m})$. Trong đó, $n$ và $m$ lần lượt là độ dài của mảng và giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distinctPrimeFactors(self, nums: List[int]) -> int:
        s = set()
        for n in nums:
            i = 2
            while i <= n // i:
                if n % i == 0:
                    s.add(i)
                    while n % i == 0:
                        n //= i
                i += 1
            if n > 1:
                s.add(n)
        return len(s)
```

#### Java

```java
class Solution {
    public int distinctPrimeFactors(int[] nums) {
        Set<Integer> s = new HashSet<>();
        for (int n : nums) {
            for (int i = 2; i <= n / i; ++i) {
                if (n % i == 0) {
                    s.add(i);
                    while (n % i == 0) {
                        n /= i;
                    }
                }
            }
            if (n > 1) {
                s.add(n);
            }
        }
        return s.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int distinctPrimeFactors(vector<int>& nums) {
        unordered_set<int> s;
        for (int& n : nums) {
            for (int i = 2; i <= n / i; ++i) {
                if (n % i == 0) {
                    s.insert(i);
                    while (n % i == 0) {
                        n /= i;
                    }
                }
            }
            if (n > 1) {
                s.insert(n);
            }
        }
        return s.size();
    }
};
```

#### Go

```go
func distinctPrimeFactors(nums []int) int {
	s := map[int]bool{}
	for _, n := range nums {
		for i := 2; i <= n/i; i++ {
			if n%i == 0 {
				s[i] = true
				for n%i == 0 {
					n /= i
				}
			}
		}
		if n > 1 {
			s[n] = true
		}
	}
	return len(s)
}
```

#### TypeScript

```ts
function distinctPrimeFactors(nums: number[]): number {
    const s: Set<number> = new Set();
    for (let n of nums) {
        let i = 2;
        while (i <= n / i) {
            if (n % i === 0) {
                s.add(i);
                while (n % i === 0) {
                    n = Math.floor(n / i);
                }
            }
            ++i;
        }
        if (n > 1) {
            s.add(n);
        }
    }
    return s.size;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
