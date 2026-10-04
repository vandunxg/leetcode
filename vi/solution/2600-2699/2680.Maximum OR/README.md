---
comments: true
difficulty: Medium
rating: 1912
source: Biweekly Contest 104 Q3
tags:
    - Greedy
    - Bit Manipulation
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [2680. Maximum OR](https://leetcode.com/problems/maximum-or)

[中文文档](/solution/2600-2699/2680.Maximum%20OR/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>, có độ dài <code>n</code>, và một số nguyên <code>k</code>. Trong một thao tác, bạn có thể chọn một phần tử và nhân nó với <code>2</code>.</p>

<p>Hãy trả về <em>giá trị lớn nhất có thể đạt được của </em><code>nums[0] | nums[1] | ... | nums[n - 1]</code> <em>sau khi thực hiện thao tác trên nums không quá </em><code>k</code><em> lần</em>.</p>

<p>Lưu ý rằng <code>a | b</code> biểu thị <strong>phép OR theo bit</strong> giữa hai số nguyên <code>a</code> và <code>b</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [12,9], k = 1
<strong>Đầu ra:</strong> 30
<strong>Giải thích:</strong> Nếu thực hiện thao tác trên chỉ số 1, mảng nums mới sẽ là [12,18]. Do đó, ta trả về phép OR theo bit của 12 và 18, bằng 30.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [8,1,2], k = 2
<strong>Đầu ra:</strong> 35
<strong>Giải thích:</strong> Nếu thực hiện thao tác hai lần trên chỉ số 0, ta thu được mảng mới [32,1,2]. Do đó, ta trả về 32|1|2 = 35.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 15</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Tiền xử lý

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể dịch trái các số tổng cộng $k$ lần để tối đa hóa phép OR theo bit. Phân tán các lần dịch sẽ tách các bit cao; dồn cả $k$ lần dịch vào một giá trị là tối ưu.
>
> Khi $nums[i]$ là giá trị được tăng cường, phần còn lại là OR tiền tố và OR hậu tố của nó. Mảng $suf$ được tính trước cho phép ta thử mọi chỉ số chỉ với một lần duyệt.

<!-- thinking:end -->

Ta nhận thấy rằng để tối đa hóa kết quả, ta nên thực hiện phép OR theo bit $k$ lần trên cùng một số.

Trước tiên, ta tiền xử lý mảng các giá trị OR hậu tố $suf$ của mảng $nums$, trong đó $suf[i]$ biểu diễn giá trị OR theo bit của $nums[i], nums[i + 1], \cdots, nums[n - 1]$.

Tiếp theo, ta duyệt mảng $nums$ từ trái sang phải và duy trì giá trị OR tiền tố hiện tại $pre$. Với vị trí hiện tại $i$, ta thực hiện phép dịch trái theo bit $k$ lần trên $nums[i]$, tức là $nums[i] \times 2^k$, rồi thực hiện phép OR theo bit với $pre$ để thu được kết quả trung gian. Sau đó, ta thực hiện phép OR theo bit với $suf[i + 1]$ để thu được giá trị OR lớn nhất khi $nums[i]$ là số cuối cùng. Bằng cách liệt kê mọi vị trí $i$, ta có thể tìm được đáp án cuối cùng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumOr(self, nums: List[int], k: int) -> int:
        n = len(nums)
        suf = [0] * (n + 1)
        for i in range(n - 1, -1, -1):
            suf[i] = suf[i + 1] | nums[i]
        ans = pre = 0
        for i, x in enumerate(nums):
            ans = max(ans, pre | (x << k) | suf[i + 1])
            pre |= x
        return ans
```

#### Java

```java
class Solution {
    public long maximumOr(int[] nums, int k) {
        int n = nums.length;
        long[] suf = new long[n + 1];
        for (int i = n - 1; i >= 0; --i) {
            suf[i] = suf[i + 1] | nums[i];
        }
        long ans = 0, pre = 0;
        for (int i = 0; i < n; ++i) {
            ans = Math.max(ans, pre | (1L * nums[i] << k) | suf[i + 1]);
            pre |= nums[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumOr(vector<int>& nums, int k) {
        int n = nums.size();
        long long suf[n + 1];
        memset(suf, 0, sizeof(suf));
        for (int i = n - 1; i >= 0; --i) {
            suf[i] = suf[i + 1] | nums[i];
        }
        long long ans = 0, pre = 0;
        for (int i = 0; i < n; ++i) {
            ans = max(ans, pre | (1LL * nums[i] << k) | suf[i + 1]);
            pre |= nums[i];
        }
        return ans;
    }
};
```

#### Go

```go
func maximumOr(nums []int, k int) int64 {
	n := len(nums)
	suf := make([]int, n+1)
	for i := n - 1; i >= 0; i-- {
		suf[i] = suf[i+1] | nums[i]
	}
	ans, pre := 0, 0
	for i, x := range nums {
		ans = max(ans, pre|(nums[i]<<k)|suf[i+1])
		pre |= x
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function maximumOr(nums: number[], k: number): number {
    const n = nums.length;
    const suf: bigint[] = Array(n + 1).fill(0n);
    for (let i = n - 1; i >= 0; i--) {
        suf[i] = suf[i + 1] | BigInt(nums[i]);
    }
    let [ans, pre] = [0, 0n];
    for (let i = 0; i < n; i++) {
        ans = Math.max(Number(ans), Number(pre | (BigInt(nums[i]) << BigInt(k)) | suf[i + 1]));
        pre |= BigInt(nums[i]);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_or(nums: Vec<i32>, k: i32) -> i64 {
        let n = nums.len();
        let mut suf = vec![0; n + 1];

        for i in (0..n).rev() {
            suf[i] = suf[i + 1] | (nums[i] as i64);
        }

        let mut ans = 0i64;
        let mut pre = 0i64;
        let k64 = k as i64;
        for i in 0..n {
            ans = ans.max(pre | ((nums[i] as i64) << k64) | suf[i + 1]);
            pre |= nums[i] as i64;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
