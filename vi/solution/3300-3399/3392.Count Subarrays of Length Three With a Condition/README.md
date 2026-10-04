---
comments: true
difficulty: Easy
rating: 1200
source: Biweekly Contest 146 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3392. Count Subarrays of Length Three With a Condition](https://leetcode.com/problems/count-subarrays-of-length-three-with-a-condition)

[中文文档](/solution/3300-3399/3392.Count%20Subarrays%20of%20Length%20Three%20With%20a%20Condition/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>. Hãy trả về số lượng <span data-keyword="subarray-nonempty">mảng con</span> có độ dài 3 sao cho tổng của phần tử thứ nhất và thứ ba <em>chính xác</em> bằng một nửa phần tử thứ hai.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,4,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có mảng con <code>[1,4,1]</code> chứa đúng 3 phần tử, trong đó tổng của phần tử thứ nhất và thứ ba bằng một nửa phần tử ở giữa.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>[1,1,1]</code> là mảng con duy nhất có độ dài 3. Tuy nhiên, phần tử thứ nhất và thứ ba không có tổng bằng một nửa phần tử ở giữa.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 100</code></li>
	<li><code><font face="monospace">-100 &lt;= nums[i] &lt;= 100</font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con độ dài-$3$ được đếm khi hai lần tổng của hai phần tử biên bằng phần tử ở giữa. Với $n \le 100$, ta duyệt chỉ số phần tử ở giữa.
>
> Với mỗi $i \in [1,n-2]$, ta kiểm tra $(nums[i-1]+nums[i+1])\times 2 = nums[i]$ và đếm số trường hợp thỏa mãn.

<!-- thinking:end -->

Chúng ta duyệt qua từng mảng con có độ dài $3$ trong mảng $\textit{nums}$ và kiểm tra xem hai lần tổng của phần tử thứ nhất và thứ ba có bằng phần tử thứ hai hay không. Nếu đúng, ta tăng đáp án thêm $1$.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubarrays(self, nums: List[int]) -> int:
        return sum(
            (nums[i - 1] + nums[i + 1]) * 2 == nums[i] for i in range(1, len(nums) - 1)
        )
```

#### Java

```java
class Solution {
    public int countSubarrays(int[] nums) {
        int ans = 0;
        for (int i = 1; i + 1 < nums.length; ++i) {
            if ((nums[i - 1] + nums[i + 1]) * 2 == nums[i]) {
                ++ans;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countSubarrays(vector<int>& nums) {
        int ans = 0;
        for (int i = 1; i + 1 < nums.size(); ++i) {
            if ((nums[i - 1] + nums[i + 1]) * 2 == nums[i]) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countSubarrays(nums []int) (ans int) {
	for i := 1; i+1 < len(nums); i++ {
		if (nums[i-1]+nums[i+1])*2 == nums[i] {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function countSubarrays(nums: number[]): number {
    let ans: number = 0;
    for (let i = 1; i + 1 < nums.length; ++i) {
        if ((nums[i - 1] + nums[i + 1]) * 2 === nums[i]) {
            ++ans;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_subarrays(nums: Vec<i32>) -> i32 {
        let mut ans = 0;
        for i in 1..nums.len() - 1 {
            if (nums[i - 1] + nums[i + 1]) * 2 == nums[i] {
                ans += 1;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
