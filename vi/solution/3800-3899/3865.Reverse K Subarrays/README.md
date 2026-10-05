---
comments: true
difficulty: Medium
tags:
    - Array
    - Two Pointers
---

<!-- problem:start -->

# [3865. Reverse K Subarrays 🔒](https://leetcode.com/problems/reverse-k-subarrays)

[中文文档](/solution/3800-3899/3865.Reverse%20K%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>k</code>.</p>

<p>Hãy <strong>chia</strong> mảng thành <code>k</code> mảng con liên tiếp có độ dài <strong>bằng nhau</strong> và <strong>đảo ngược</strong> từng mảng con.</p>

<p>Đảm bảo rằng <code>n</code> chia hết cho <code>k</code>.</p>

<p>Trả về mảng thu được sau khi thực hiện thao tác trên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,4,3,5,6], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,1,3,4,6,5]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mảng được chia thành <code>k = 3</code> mảng con: <code>[1, 2]</code>, <code>[4, 3]</code> và <code>[5, 6]</code>.</li>
	<li>Sau khi đảo ngược từng mảng con: <code>[2, 1]</code>, <code>[3, 4]</code> và <code>[6, 5]</code>.</li>
	<li>Ghép chúng lại ta được mảng cuối cùng <code>[2, 1, 3, 4, 6, 5]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,4,4,2], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,4,4,5]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mảng được chia thành <code>k = 1</code> mảng con: <code>[5, 4, 4, 2]</code>.</li>
	<li>Đảo ngược mảng con này ta được <code>[2, 4, 4, 5]</code>, chính là mảng cuối cùng.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
	<li><code>n</code> chia hết cho <code>k</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chia mảng thành $k$ phần bằng nhau rồi đảo ngược từng phần. Vì $n$ chia hết cho $k$, ta có thể mô phỏng theo từng đoạn.
>
> Mỗi phần có độ dài $m=n/k$; đảo ngược các lát cắt với bước nhảy $m$.
>
> Ghi trực tiếp tại chỗ nên không cần cấu trúc dữ liệu phụ.
>
> Tổng số phần tử được di chuyển là $O(n)$.

<!-- thinking:end -->

Vì cần chia mảng thành $k$ mảng con có cùng độ dài, độ dài của mỗi mảng con là $m = \frac{n}{k}$. Ta có thể dùng một vòng lặp duyệt qua mảng với bước nhảy $m$, và đảo ngược mảng con hiện tại trong mỗi lần lặp.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$ vì ta chỉ sử dụng một lượng không gian phụ hằng số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reverseSubarrays(self, nums: list[int], k: int) -> list[int]:
        n = len(nums)
        m = n // k
        for i in range(0, n, m):
            nums[i : i + m] = nums[i : i + m][::-1]
        return nums
```

#### Java

```java
class Solution {
    public int[] reverseSubarrays(int[] nums, int k) {
        int n = nums.length;
        int m = n / k;
        for (int i = 0; i < n; i += m) {
            int l = i, r = i + m - 1;
            while (l < r) {
                int t = nums[l];
                nums[l++] = nums[r];
                nums[r--] = t;
            }
        }
        return nums;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> reverseSubarrays(vector<int>& nums, int k) {
        int n = nums.size();
        int m = n / k;
        for (int i = 0; i < n; i += m) {
            int l = i, r = i + m - 1;
            while (l < r) {
                swap(nums[l++], nums[r--]);
            }
        }
        return nums;
    }
};
```

#### Go

```go
func reverseSubarrays(nums []int, k int) []int {
	n := len(nums)
	m := n / k
	for i := 0; i < n; i += m {
		l, r := i, i+m-1
		for l < r {
			nums[l], nums[r] = nums[r], nums[l]
			l++
			r--
		}
	}
	return nums
}
```

#### TypeScript

```ts
function reverseSubarrays(nums: number[], k: number): number[] {
    const n = nums.length;
    const m = Math.floor(n / k);
    for (let i = 0; i < n; i += m) {
        let l = i,
            r = i + m - 1;
        while (l < r) {
            const t = nums[l];
            nums[l++] = nums[r];
            nums[r--] = t;
        }
    }
    return nums;
}
```

#### Rust

```rust
impl Solution {
    pub fn reverse_subarrays(mut nums: Vec<i32>, k: i32) -> Vec<i32> {
        let n = nums.len();
        let m = n / k as usize;

        for i in (0..n).step_by(m) {
            let mut l = i;
            let mut r = i + m - 1;
            while l < r {
                nums.swap(l, r);
                l += 1;
                r -= 1;
            }
        }

        nums
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
