---
comments: true
difficulty: Medium
rating: 2419
source: Weekly Contest 333 Q3
tags:
    - Bit Manipulation
    - Array
    - Math
    - Dynamic Programming
    - Bitmask
    - Number Theory
---

<!-- problem:start -->

# [2572. Count the Number of Square-Free Subsets](https://leetcode.com/problems/count-the-number-of-square-free-subsets)

[中文文档](/solution/2500-2599/2572.Count%20the%20Number%20of%20Square-Free%20Subsets/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên dương <strong>0-indexed</strong>&nbsp;<code>nums</code>.</p>

<p>Một tập con của mảng <code>nums</code> được gọi là <strong>square-free</strong> nếu tích các phần tử của nó là một <strong>số nguyên square-free</strong>.</p>

<p>Một <strong>số nguyên square-free</strong> là số nguyên không chia hết cho bất kỳ số chính phương nào ngoài <code>1</code>.</p>

<p>Trả về <em>số lượng tập con khác rỗng và square-free của mảng</em> <strong>nums</strong>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Một <strong>tập con</strong> <strong>khác rỗng</strong> của <code>nums</code> là một mảng có thể thu được bằng cách xóa một số phần tử (có thể không xóa phần tử nào nhưng không được xóa tất cả) khỏi <code>nums</code>. Hai tập con khác nhau khi và chỉ khi các chỉ số phần tử bị xóa khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,4,5]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 3 tập con square-free trong ví dụ này:
- Tập con chỉ gồm phần tử thứ 0<sup>th</sup> [3]. Tích các phần tử là 3, đây là một số nguyên square-free.
- Tập con chỉ gồm phần tử thứ 3<sup>rd</sup> [5]. Tích các phần tử là 5, đây là một số nguyên square-free.
- Tập con gồm các phần tử thứ 0<sup>th</sup> và thứ 3<sup>rd</sup> [3,5]. Tích các phần tử là 15, đây là một số nguyên square-free.
Có thể chứng minh rằng mảng đã cho không có quá 3 tập con square-free.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có 1 tập con square-free trong ví dụ này:
- Tập con chỉ gồm phần tử thứ 0<sup>th</sup> [1]. Tích các phần tử là 1, đây là một số nguyên square-free.
Có thể chứng minh rằng mảng đã cho không có quá 1 tập con square-free.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length&nbsp;&lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 30</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động nén trạng thái

<!-- thinking:start -->

> **Tư duy**
>
> Cần đếm các tập con khác rỗng có tích square-free. Các giá trị nằm trong $[1,30]$, vì vậy không cần xét $2^n$: chỉ có ba mươi số phân biệt, và mọi số có thừa số chính phương đều bị loại.
>
> Có mười số nguyên tố nhỏ hơn $30$, nên tập các thừa số nguyên tố của một tập con được biểu diễn bằng mask $10$ bit. $f[\textit{state}]$ là số cách tạo ra mask đó; các số 1 có thể được chọn tùy ý, nên $f[0]=2^{\textit{cnt}[1]}$. Mỗi $x$ square-free là một item $0$-$1$ được chuyển từ các mask lớn xuống nhỏ. Cộng tất cả các trạng thái rồi loại bỏ tập con rỗng.

<!-- thinking:end -->

Lưu ý rằng trong bài toán này, miền giá trị của $nums[i]$ là $[1, 30]$. Do đó, ta có thể tiền xử lý tất cả các số nguyên tố nhỏ hơn hoặc bằng $30$, đó là $[2, 3, 5, 7, 11, 13, 17, 19, 23, 29]$.

Trong một tập con không chứa số chính phương, tích của tất cả các phần tử có thể được biểu diễn thành tích của một hoặc nhiều số nguyên tố phân biệt, nghĩa là mỗi thừa số nguyên tố xuất hiện nhiều nhất một lần. Vì vậy, ta có thể dùng một số nhị phân để biểu diễn các thừa số nguyên tố trong một tập con, trong đó bit thứ $i$ của số nhị phân cho biết số nguyên tố $primes[i]$ có xuất hiện trong tập con hay không.

Ta có thể dùng phương pháp quy hoạch động nén trạng thái để giải bài toán này. Gọi $f[i]$ là số phương án mà tích các thừa số nguyên tố trong tập con được biểu diễn bởi số nhị phân $i$ là tích của một hoặc nhiều số nguyên tố phân biệt. Ban đầu, $f[0]=1$.

Ta duyệt một số $x$ trong miền $[2,..30]$. Nếu $x$ không xuất hiện trong $nums$, hoặc $x$ là bội của $4, 9, 25$, ta có thể bỏ qua ngay. Ngược lại, ta biểu diễn các thừa số nguyên tố của $x$ bằng một số nhị phân $mask$. Sau đó, ta duyệt trạng thái hiện tại $state$ từ lớn đến nhỏ. Nếu phép AND bit giữa $state$ và $mask$ cho kết quả là $mask$, ta có thể chuyển từ trạng thái $f[state \oplus mask]$ sang trạng thái $f[state]$; công thức chuyển là $f[state] = f[state] + cnt[x] \times f[state \oplus mask]$, trong đó $cnt[x]$ là số lần $x$ xuất hiện trong $nums$.

Lưu ý rằng ta không bắt đầu duyệt từ số $1$, vì ta có thể chọn tùy ý một số lượng các số $1$ và thêm chúng vào tập con không chứa số chính phương. Hoặc ta cũng có thể chọn một số lượng tùy ý các số $1$ nhưng không thêm chúng vào tập con không chứa số chính phương. Cả hai trường hợp đều hợp lệ. Vì vậy, đáp án là $(\sum_{i=0}^{2^{10}-1} f[i]) - 1$.

Độ phức tạp thời gian là $O(n + C \times M)$, và độ phức tạp không gian là $O(M)$. Trong đó, $n$ là độ dài của $nums$; còn $C$ và $M$ lần lượt là miền giá trị của $nums[i]$ và số lượng trạng thái trong bài toán. Với bài toán này, $C=30$, $M=2^{10}$.

Các bài toán tương tự:

- [1994. The Number of Good Subsets](https://github.com/doocs/leetcode/blob/main/solution/1900-1999/1994.The%20Number%20of%20Good%20Subsets/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def squareFreeSubsets(self, nums: List[int]) -> int:
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
        return sum(v for v in f) % mod - 1
```

#### Java

```java
class Solution {
    public int squareFreeSubsets(int[] nums) {
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
        for (int i = 0; i < 1 << n; ++i) {
            ans = (ans + f[i]) % mod;
        }
        ans -= 1;
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int squareFreeSubsets(vector<int>& nums) {
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
        long long ans = -1;
        for (int i = 0; i < 1 << n; ++i) {
            ans = (ans + f[i]) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func squareFreeSubsets(nums []int) (ans int) {
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
	ans = -1
	for _, v := range f {
		ans = (ans + v) % mod
	}
	return
}
```

#### TypeScript

```ts
function squareFreeSubsets(nums: number[]): number {
    const primes: number[] = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29];
    const cnt: number[] = Array(31).fill(0);
    for (const x of nums) {
        ++cnt[x];
    }
    const mod: number = Math.pow(10, 9) + 7;
    const n: number = primes.length;
    const f: number[] = Array(1 << n).fill(0);
    f[0] = 1;
    for (let i = 0; i < cnt[1]; ++i) {
        f[0] = (f[0] * 2) % mod;
    }
    for (let x = 2; x < 31; ++x) {
        if (cnt[x] === 0 || x % 4 === 0 || x % 9 === 0 || x % 25 === 0) {
            continue;
        }
        let mask: number = 0;
        for (let i = 0; i < n; ++i) {
            if (x % primes[i] === 0) {
                mask |= 1 << i;
            }
        }
        for (let state = (1 << n) - 1; state > 0; --state) {
            if ((state & mask) === mask) {
                f[state] = (f[state] + cnt[x] * f[state ^ mask]) % mod;
            }
        }
    }
    let ans: number = 0;
    for (let i = 0; i < 1 << n; ++i) {
        ans = (ans + f[i]) % mod;
    }
    ans -= 1;
    return ans >= 0 ? ans : ans + mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
