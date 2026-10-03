---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Brainteaser
    - Array
    - Math
    - Prefix Sum
---

<!-- problem:start -->

# [2505. Bitwise OR of All Subsequence Sums 🔒](https://leetcode.com/problems/bitwise-or-of-all-subsequence-sums)

[中文文档](/solution/2500-2599/2505.Bitwise%20OR%20of%20All%20Subsequence%20Sums/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, hãy trả về <em>giá trị của phép </em><strong>OR</strong><em> bitwise của tổng của tất cả các <strong>dãy con</strong> có thể có trong mảng</em>.</p>

<p><strong>Dãy con</strong> là một dãy có thể được tạo ra từ một dãy khác bằng cách xóa không hoặc nhiều phần tử mà không thay đổi thứ tự của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,0,3]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Tất cả các tổng dãy con có thể có là: 0, 1, 2, 3, 4, 5, 6.
Và 0 OR 1 OR 2 OR 3 OR 4 OR 5 OR 6 = 7, nên ta trả về 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,0,0]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> 0 là tổng dãy con duy nhất có thể có, nên ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Có $2^n$ tổng dãy con, không thể liệt kê khi $n\le 10^5$. Đáp án là phép OR bit của chúng, nên chỉ cần biết mỗi bit có thể xuất hiện trong một tổng nào đó hay không.
>
> Bit $i$ bằng $1$ có thể xuất phát từ một số ban đầu hoặc từ các phép nhớ khi cộng theo cặp ở các bit thấp hơn. Sau khi đếm số bit 1 ở mỗi vị trí, duyệt từ thấp lên cao: nếu số lượng dương, OR bit đó vào đáp án và cộng $\lfloor cnt[i]/2\rfloor$ vào bit tiếp theo, qua đó tính tất cả phép nhớ có thể xảy ra.

<!-- thinking:end -->

Trước tiên, ta dùng một mảng $cnt$ để đếm số bit 1 ở mỗi vị trí. Sau đó, duyệt từ bit thấp nhất đến bit cao nhất. Nếu số bit 1 ở vị trí đó lớn hơn 0, ta thêm giá trị tương ứng với bit đó vào đáp án. Tiếp theo, kiểm tra xem có thể phát sinh phép nhớ hay không; nếu có, ta cộng nó vào bit kế tiếp.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ là độ dài của mảng và $M$ là giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subsequenceSumOr(self, nums: List[int]) -> int:
        cnt = [0] * 64
        ans = 0
        for v in nums:
            for i in range(31):
                if (v >> i) & 1:
                    cnt[i] += 1
        for i in range(63):
            if cnt[i]:
                ans |= 1 << i
            cnt[i + 1] += cnt[i] // 2
        return ans
```

#### Java

```java
class Solution {
    public long subsequenceSumOr(int[] nums) {
        long[] cnt = new long[64];
        long ans = 0;
        for (int v : nums) {
            for (int i = 0; i < 31; ++i) {
                if (((v >> i) & 1) == 1) {
                    ++cnt[i];
                }
            }
        }
        for (int i = 0; i < 63; ++i) {
            if (cnt[i] > 0) {
                ans |= 1l << i;
            }
            cnt[i + 1] += cnt[i] / 2;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long subsequenceSumOr(vector<int>& nums) {
        vector<long long> cnt(64);
        long long ans = 0;
        for (int v : nums) {
            for (int i = 0; i < 31; ++i) {
                if (v >> i & 1) {
                    ++cnt[i];
                }
            }
        }
        for (int i = 0; i < 63; ++i) {
            if (cnt[i]) {
                ans |= 1ll << i;
            }
            cnt[i + 1] += cnt[i] / 2;
        }
        return ans;
    }
};
```

#### Go

```go
func subsequenceSumOr(nums []int) int64 {
	cnt := make([]int, 64)
	ans := 0
	for _, v := range nums {
		for i := 0; i < 31; i++ {
			if v>>i&1 == 1 {
				cnt[i]++
			}
		}
	}
	for i := 0; i < 63; i++ {
		if cnt[i] > 0 {
			ans |= 1 << i
		}
		cnt[i+1] += cnt[i] / 2
	}
	return int64(ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
