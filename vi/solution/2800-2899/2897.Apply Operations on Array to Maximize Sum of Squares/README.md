---
comments: true
difficulty: Hard
rating: 2301
source: Weekly Contest 366 Q4
tags:
    - Greedy
    - Bit Manipulation
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2897. Apply Operations on Array to Maximize Sum of Squares](https://leetcode.com/problems/apply-operations-on-array-to-maximize-sum-of-squares)

[中文文档](/solution/2800-2899/2897.Apply%20Operations%20on%20Array%20to%20Maximize%20Sum%20of%20Squares/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> <strong>được đánh chỉ số từ 0</strong> và một số nguyên <code>k</code> <strong>dương</strong>.</p>

<p>Bạn có thể thực hiện thao tác sau trên mảng <strong>bao nhiêu lần tùy ý</strong>:</p>

<ul>
	<li>Chọn hai chỉ số phân biệt bất kỳ <code>i</code> và <code>j</code>, <strong>đồng thời</strong> cập nhật giá trị của <code>nums[i]</code> thành <code>(nums[i] AND nums[j])</code> và giá trị của <code>nums[j]</code> thành <code>(nums[i] OR nums[j])</code>. Ở đây, <code>OR</code> biểu thị phép toán <code>OR</code> theo bit, còn <code>AND</code> biểu thị phép toán <code>AND</code> theo bit.</li>
</ul>

<p>Bạn phải chọn <code>k</code> phần tử từ mảng cuối cùng và tính tổng <strong>bình phương</strong> của chúng.</p>

<p>Trả về <em>tổng bình phương <strong>lớn nhất</strong> có thể đạt được</em>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>lấy modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,6,5,8], k = 2
<strong>Đầu ra:</strong> 261
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác sau trên mảng:
- Chọn i = 0 và j = 3, sau đó thay nums[0] bằng (2 AND 8) = 0 và nums[3] bằng (2 OR 8) = 10. Mảng nhận được là nums = [0,6,5,10].
- Chọn i = 2 và j = 3, sau đó thay nums[2] bằng (5 AND 10) = 0 và nums[3] bằng (5 OR 10) = 15. Mảng nhận được là nums = [0,6,0,15].
Ta có thể chọn các phần tử 15 và 6 từ mảng cuối cùng. Tổng bình phương là 15<sup>2</sup> + 6<sup>2</sup> = 261.
Có thể chứng minh rằng đây là giá trị lớn nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,5,4,7], k = 3
<strong>Đầu ra:</strong> 90
<strong>Giải thích:</strong> Ta không cần thực hiện thao tác nào.
Ta có thể chọn các phần tử 7, 5 và 4, với tổng bình phương là 7<sup>2</sup> + 5<sup>2</sup> + 4<sup>2</sup> = 90.
Có thể chứng minh rằng đây là giá trị lớn nhất có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phép toán bit + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác chuyển một bit $1$ sang vị trí bit tương ứng của một số khác; tổng bình phương tăng khi các bit 1 được tập trung. Sau khi đếm số bit 1 ở mỗi vị trí, ta lần lượt tham lam xây dựng $k$ số lớn nhất có thể tạo được (chọn một bit khi vẫn còn bit 1) và cộng bình phương của chúng.

<!-- thinking:end -->

Theo mô tả đề bài, trong một thao tác, ta thay đổi $nums[i]$ thành $nums[i] \textit{ AND } nums[j]$, đồng thời thay đổi $nums[j]$ thành $nums[i] \textit{ OR } nums[j]$. Hãy xét các bit của những số này. Nếu hai bit đều là $1$ hoặc đều là $0$, kết quả của thao tác không làm thay đổi các bit đó. Nếu hai bit khác nhau, kết quả sẽ lần lượt trở thành $0$ và $1$. Vì vậy, ta có thể chuyển các bit $1$ thành bit $0$, nhưng không thể làm ngược lại.

Ta có thể dùng một mảng $cnt$ để đếm số bit $1$ ở mỗi vị trí, sau đó chọn $k$ số từ các bit này. Để tối đa hóa tổng bình phương, ta nên chọn các số lớn nhất có thể. Bởi vì, giả sử tổng bình phương của hai số là $a^2 + b^2$ (trong đó $a \gt b$), khi biến đổi chúng thành $(a + c)^2 + (b - c)^2 = a^2 + b^2 + 2c(a - b) + 2c^2 \gt a^2 + b^2$, tổng bình phương sẽ tăng. Do đó, để tối đa hóa tổng bình phương, ta nên chọn số lớn nhất.

Độ phức tạp thời gian là $O(n \times \log M)$, độ phức tạp không gian là $O(\log M)$. Trong đó, $M$ là giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSum(self, nums: List[int], k: int) -> int:
        mod = 10**9 + 7
        cnt = [0] * 31
        for x in nums:
            for i in range(31):
                if x >> i & 1:
                    cnt[i] += 1
        ans = 0
        for _ in range(k):
            x = 0
            for i in range(31):
                if cnt[i]:
                    x |= 1 << i
                    cnt[i] -= 1
            ans = (ans + x * x) % mod
        return ans
```

#### Java

```java
class Solution {
    public int maxSum(List<Integer> nums, int k) {
        final int mod = (int) 1e9 + 7;
        int[] cnt = new int[31];
        for (int x : nums) {
            for (int i = 0; i < 31; ++i) {
                if ((x >> i & 1) == 1) {
                    ++cnt[i];
                }
            }
        }
        long ans = 0;
        while (k-- > 0) {
            int x = 0;
            for (int i = 0; i < 31; ++i) {
                if (cnt[i] > 0) {
                    x |= 1 << i;
                    --cnt[i];
                }
            }
            ans = (ans + 1L * x * x) % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSum(vector<int>& nums, int k) {
        int cnt[31]{};
        for (int x : nums) {
            for (int i = 0; i < 31; ++i) {
                if (x >> i & 1) {
                    ++cnt[i];
                }
            }
        }
        long long ans = 0;
        const int mod = 1e9 + 7;
        while (k--) {
            int x = 0;
            for (int i = 0; i < 31; ++i) {
                if (cnt[i]) {
                    x |= 1 << i;
                    --cnt[i];
                }
            }
            ans = (ans + 1LL * x * x) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func maxSum(nums []int, k int) (ans int) {
	cnt := [31]int{}
	for _, x := range nums {
		for i := 0; i < 31; i++ {
			if x>>i&1 == 1 {
				cnt[i]++
			}
		}
	}
	const mod int = 1e9 + 7
	for ; k > 0; k-- {
		x := 0
		for i := 0; i < 31; i++ {
			if cnt[i] > 0 {
				x |= 1 << i
				cnt[i]--
			}
		}
		ans = (ans + x*x) % mod
	}
	return
}
```

#### TypeScript

```ts
function maxSum(nums: number[], k: number): number {
    const cnt: number[] = Array(31).fill(0);
    for (const x of nums) {
        for (let i = 0; i < 31; ++i) {
            if ((x >> i) & 1) {
                ++cnt[i];
            }
        }
    }
    let ans = 0n;
    const mod = 1e9 + 7;
    while (k-- > 0) {
        let x = 0;
        for (let i = 0; i < 31; ++i) {
            if (cnt[i] > 0) {
                x |= 1 << i;
                --cnt[i];
            }
        }
        ans = (ans + BigInt(x) * BigInt(x)) % BigInt(mod);
    }
    return Number(ans);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
