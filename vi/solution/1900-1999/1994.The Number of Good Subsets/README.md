---
comments: true
difficulty: Hard
rating: 2464
source: Biweekly Contest 60 Q4
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Math
    - Dynamic Programming
    - Bitmask
    - Counting
    - Number Theory
    - Sieve
---

<!-- problem:start -->

# [1994. The Number of Good Subsets](https://leetcode.com/problems/the-number-of-good-subsets)

[中文文档](/solution/1900-1999/1994.The%20Number%20of%20Good%20Subsets/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <code>nums</code>. Ta gọi một tập con của <code>nums</code> là <strong>tốt</strong> nếu tích của nó có thể biểu diễn dưới dạng tích của một hoặc nhiều số nguyên tố <strong>phân biệt</strong>.</p>

<ul>
	<li>Ví dụ, nếu <code>nums = [1, 2, 3, 4]</code>:

    <ul>
        <li><code>[2, 3]</code>, <code>[1, 2, 3]</code> và <code>[1, 3]</code> là các tập con <strong>tốt</strong>, với tích lần lượt là <code>6 = 2*3</code>, <code>6 = 2*3</code> và <code>3 = 3</code>.</li>
        <li><code>[1, 4]</code> và <code>[4]</code> không phải là các tập con <strong>tốt</strong>, với tích lần lượt là <code>4 = 2*2</code> và <code>4 = 2*2</code>.</li>
    </ul>
    </li>

</ul>

<p>Hãy trả về <em>số lượng các tập con <strong>tốt</strong> khác nhau trong </em><code>nums</code><em>, lấy <strong>phần dư</strong> khi chia cho </em><code>10<sup>9</sup> + 7</code>.</p>

<p>Một <strong>tập con</strong> của <code>nums</code> là một mảng có thể thu được từ <code>nums</code> bằng cách xóa đi một số phần tử (có thể không xóa hoặc xóa tất cả). Hai tập con khác nhau khi và chỉ khi các chỉ số phần tử bị xóa khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Các tập con tốt là:
- [1,2]: tích bằng 2, là tích của số nguyên tố phân biệt 2.
- [1,2,3]: tích bằng 6, là tích của các số nguyên tố phân biệt 2 và 3.
- [1,3]: tích bằng 3, là tích của số nguyên tố phân biệt 3.
- [2]: tích bằng 2, là tích của số nguyên tố phân biệt 2.
- [2,3]: tích bằng 6, là tích của các số nguyên tố phân biệt 2 và 3.
- [3]: tích bằng 3, là tích của số nguyên tố phân biệt 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,2,3,15]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Các tập con tốt là:
- [2]: tích bằng 2, là tích của số nguyên tố phân biệt 2.
- [2,3]: tích bằng 6, là tích của các số nguyên tố phân biệt 2 và 3.
- [2,15]: tích bằng 30, là tích của các số nguyên tố phân biệt 2, 3 và 5.
- [3]: tích bằng 3, là tích của số nguyên tố phân biệt 3.
- [15]: tích bằng 15, là tích của các số nguyên tố phân biệt 3 và 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 30</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một tập con tốt không chứa bình phương của một số nguyên tố và các giá trị nằm trong $[1,30]$. Ta bỏ qua các bội của bình phương; các số còn lại có các thừa số nguyên tố phân biệt, có thể biểu diễn bằng một mask $10$ bit.
>
> $f[\textit{state}]$ đếm các tập con có cùng tập số nguyên tố đó. Với mỗi $x$, ta cập nhật các state chứa mask của nó theo thứ tự từ lớn xuống nhỏ. Mỗi số 1 sẽ nhân số cách của tập rỗng lên tương ứng, sau đó ta tính tổng các state khác rỗng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfGoodSubsets(self, nums: List[int]) -> int:
        primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
        cnt = Counter(nums)
        mod = 10**9 + 7
        n = len(primes)
        f = [0] * (1 << n)
        f[0] = pow(2, cnt[1])
        for x in range(2, 31):
            if cnt[x] == 0 or x % 4 == 0 or x % 9 == 0 or x % 25 == 0:
                continue
            mask = 0
            for i, p in enumerate(primes):
                if x % p == 0:
                    mask |= 1 << i
            for state in range((1 << n) - 1, 0, -1):
                if state & mask == mask:
                    f[state] = (f[state] + cnt[x] * f[state ^ mask]) % mod
        return sum(f[i] for i in range(1, 1 << n)) % mod
```

#### Java

```java
class Solution {
    public int numberOfGoodSubsets(int[] nums) {
        int[] primes = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29};
        int[] cnt = new int[31];
        for (int x : nums) {
            ++cnt[x];
        }
        final int mod = (int) 1e9 + 7;
        int n = primes.length;
        long[] f = new long[1 << n];
        f[0] = 1;
        for (int i = 0; i < cnt[1]; ++i) {
            f[0] = (f[0] * 2) % mod;
        }
        for (int x = 2; x < 31; ++x) {
            if (cnt[x] == 0 || x % 4 == 0 || x % 9 == 0 || x % 25 == 0) {
                continue;
            }
            int mask = 0;
            for (int i = 0; i < n; ++i) {
                if (x % primes[i] == 0) {
                    mask |= 1 << i;
                }
            }
            for (int state = (1 << n) - 1; state > 0; --state) {
                if ((state & mask) == mask) {
                    f[state] = (f[state] + cnt[x] * f[state ^ mask]) % mod;
                }
            }
        }
        long ans = 0;
        for (int i = 1; i < 1 << n; ++i) {
            ans = (ans + f[i]) % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfGoodSubsets(vector<int>& nums) {
        int primes[10] = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29};
        int cnt[31]{};
        for (int& x : nums) {
            ++cnt[x];
        }
        int n = 10;
        const int mod = 1e9 + 7;
        vector<long long> f(1 << n);
        f[0] = 1;
        for (int i = 0; i < cnt[1]; ++i) {
            f[0] = f[0] * 2 % mod;
        }
        for (int x = 2; x < 31; ++x) {
            if (cnt[x] == 0 || x % 4 == 0 || x % 9 == 0 || x % 25 == 0) {
                continue;
            }
            int mask = 0;
            for (int i = 0; i < n; ++i) {
                if (x % primes[i] == 0) {
                    mask |= 1 << i;
                }
            }
            for (int state = (1 << n) - 1; state; --state) {
                if ((state & mask) == mask) {
                    f[state] = (f[state] + 1LL * cnt[x] * f[state ^ mask]) % mod;
                }
            }
        }
        long long ans = 0;
        for (int i = 1; i < 1 << n; ++i) {
            ans = (ans + f[i]) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfGoodSubsets(nums []int) (ans int) {
	primes := []int{2, 3, 5, 7, 11, 13, 17, 19, 23, 29}
	cnt := [31]int{}
	for _, x := range nums {
		cnt[x]++
	}
	const mod int = 1e9 + 7
	n := 10
	f := make([]int, 1<<n)
	f[0] = 1
	for i := 0; i < cnt[1]; i++ {
		f[0] = f[0] * 2 % mod
	}
	for x := 2; x < 31; x++ {
		if cnt[x] == 0 || x%4 == 0 || x%9 == 0 || x%25 == 0 {
			continue
		}
		mask := 0
		for i, p := range primes {
			if x%p == 0 {
				mask |= 1 << i
			}
		}
		for state := 1<<n - 1; state > 0; state-- {
			if state&mask == mask {
				f[state] = (f[state] + f[state^mask]*cnt[x]) % mod
			}
		}
	}
	for i := 1; i < 1<<n; i++ {
		ans = (ans + f[i]) % mod
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
