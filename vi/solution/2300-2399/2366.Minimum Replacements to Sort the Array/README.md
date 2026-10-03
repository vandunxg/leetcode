---
comments: true
difficulty: Hard
rating: 2060
source: Biweekly Contest 84 Q4
tags:
    - Greedy
    - Array
    - Math
---

<!-- problem:start -->

# [2366. Minimum Replacements to Sort the Array](https://leetcode.com/problems/minimum-replacements-to-sort-the-array)

[中文文档](/solution/2300-2399/2366.Minimum%20Replacements%20to%20Sort%20the%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>. Trong một thao tác, bạn có thể thay thế bất kỳ phần tử nào của mảng bằng <strong>hai</strong> phần tử bất kỳ có <strong>tổng</strong> bằng phần tử đó.</p>

<ul>
	<li>Ví dụ, xét <code>nums = [5,6,7]</code>. Trong một thao tác, ta có thể thay <code>nums[1]</code> bằng <code>2</code> và <code>4</code>, biến <code>nums</code> thành <code>[5,2,4,7]</code>.</li>
</ul>

<p>Trả về <em>số thao tác ít nhất để biến mảng thành mảng được sắp xếp theo thứ tự <strong>không giảm</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,9,3]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Dưới đây là các bước sắp xếp mảng theo thứ tự không giảm:
- Từ [3,9,3], thay 9 bằng 3 và 6 để mảng trở thành [3,3,6,3]
- Từ [3,3,6,3], thay 6 bằng 3 và 3 để mảng trở thành [3,3,3,3,3]
Cần 2 bước để sắp xếp mảng theo thứ tự không giảm. Vì vậy, ta trả về 2.

</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mảng đã được sắp xếp theo thứ tự không giảm. Vì vậy, ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Chiến lược tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Chia một số thành các phần dương để mảng trở thành không giảm, với số lần chia ít nhất có thể. $n \le 10^5$ khiến ta phải duy trì một cận từ phải sang trái.
>
> Không nên chia nhỏ giá trị lớn nhất hiện tại $mx$ ở bên phải. Nếu $nums[i] \le mx$, cập nhật $mx$; ngược lại, chia thành $k=\lceil nums[i]/mx \rceil$ phần, cộng $k-1$, rồi đặt $mx=\lfloor nums[i]/k \rfloor$ để phần bên trái giữ được giá trị lớn nhất có thể.

<!-- thinking:end -->

Ta nhận thấy rằng để mảng $nums$ trở thành không giảm hoặc tăng đơn điệu, các phần tử ở cuối mảng nên lớn nhất có thể. Vì vậy, không cần thay phần tử cuối $nums[n-1]$ của mảng $nums$ bằng nhiều số nhỏ hơn.

Nói cách khác, ta có thể duyệt mảng $nums$ từ cuối về đầu, duy trì giá trị lớn nhất hiện tại $mx$, ban đầu $mx = nums[n-1]$.

- Nếu phần tử hiện tại $nums[i] \leq mx$, không cần thay $nums[i]$. Ta chỉ cần cập nhật $mx = nums[i]$.
- Ngược lại, ta cần thay $nums[i]$ bằng nhiều số có tổng bằng $nums[i]$. Giá trị lớn nhất trong các số này là $mx$, và tổng số phần sau khi thay thế là $k=\left \lceil \frac{nums[i]}{mx} \right \rceil$. Do đó, cần thực hiện $k-1$ thao tác và cộng số này vào đáp án. Trong $k$ số trên, số nhỏ nhất là $\left \lfloor \frac{nums[i]}{k} \right \rfloor$. Vì vậy, ta cập nhật $mx = \left \lfloor \frac{nums[i]}{k} \right \rfloor$.

Sau khi duyệt xong, ta trả về tổng số thao tác.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumReplacement(self, nums: List[int]) -> int:
        ans = 0
        n = len(nums)
        mx = nums[-1]
        for i in range(n - 2, -1, -1):
            if nums[i] <= mx:
                mx = nums[i]
                continue
            k = (nums[i] + mx - 1) // mx
            ans += k - 1
            mx = nums[i] // k
        return ans
```

#### Java

```java
class Solution {
    public long minimumReplacement(int[] nums) {
        long ans = 0;
        int n = nums.length;
        int mx = nums[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            if (nums[i] <= mx) {
                mx = nums[i];
                continue;
            }
            int k = (nums[i] + mx - 1) / mx;
            ans += k - 1;
            mx = nums[i] / k;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumReplacement(vector<int>& nums) {
        long long ans = 0;
        int n = nums.size();
        int mx = nums[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            if (nums[i] <= mx) {
                mx = nums[i];
                continue;
            }
            int k = (nums[i] + mx - 1) / mx;
            ans += k - 1;
            mx = nums[i] / k;
        }
        return ans;
    }
};
```

#### Go

```go
func minimumReplacement(nums []int) (ans int64) {
	n := len(nums)
	mx := nums[n-1]
	for i := n - 2; i >= 0; i-- {
		if nums[i] <= mx {
			mx = nums[i]
			continue
		}
		k := (nums[i] + mx - 1) / mx
		ans += int64(k - 1)
		mx = nums[i] / k
	}
	return
}
```

#### TypeScript

```ts
function minimumReplacement(nums: number[]): number {
    const n = nums.length;
    let mx = nums[n - 1];
    let ans = 0;
    for (let i = n - 2; i >= 0; --i) {
        if (nums[i] <= mx) {
            mx = nums[i];
            continue;
        }
        const k = Math.ceil(nums[i] / mx);
        ans += k - 1;
        mx = Math.floor(nums[i] / k);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn minimum_replacement(nums: Vec<i32>) -> i64 {
        if nums.len() == 1 {
            return 0;
        }

        let n = nums.len();
        let mut max = *nums.last().unwrap();
        let mut ret = 0;

        for i in (0..=n - 2).rev() {
            if nums[i] <= max {
                max = nums[i];
                continue;
            }
            // Otherwise make the substitution
            let k = (nums[i] + max - 1) / max;
            ret += (k - 1) as i64;
            // Update the max value, which should be the minimum among the substitution
            max = nums[i] / k;
        }

        ret
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
