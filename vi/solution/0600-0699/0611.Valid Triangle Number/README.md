---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [611. Valid Triangle Number](https://leetcode.com/problems/valid-triangle-number)

[中文文档](/solution/0600-0699/0611.Valid%20Triangle%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, hãy trả về <em>số bộ ba phần tử được chọn từ mảng có thể làm độ dài ba cạnh của một tam giác</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,3,4]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các tổ hợp hợp lệ là:
2,3,4 (dùng số 2 thứ nhất)
2,3,4 (dùng số 2 thứ hai)
2,2,3
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,2,3,4]
<strong>Đầu ra:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sorting + Binary Search

<!-- thinking:start -->

> **Tư duy**
>
> Ba vòng lặp vẫn chấp nhận được với $n\le 10^3$, nhưng sắp xếp giúp giảm số điều kiện cần kiểm tra: khi $a\le b\le c$, chỉ còn cần thỏa mãn $a+b>c$.
>
> Cố định hai cạnh nhỏ hơn $i,j$, rồi dùng binary search tìm chỉ số đầu tiên $\ge nums[i]+nums[j]$; mọi $k$ nằm giữa hai vị trí đó là cạnh thứ ba hợp lệ.

<!-- thinking:end -->

Một tam giác hợp lệ phải thỏa mãn: **tổng độ dài của hai cạnh bất kỳ phải lớn hơn cạnh còn lại**. Cụ thể:

$$a + b \gt c \tag{1}$$

$$a + c \gt b \tag{2}$$

$$b + c \gt a \tag{3}$$

Nếu sắp xếp độ dài các cạnh theo thứ tự tăng dần, tức là $a \leq b \leq c$, thì hiển nhiên điều kiện (2) và (3) được thỏa mãn. Ta chỉ cần đảm bảo điều kiện (1) cũng đúng để tạo thành tam giác hợp lệ.

Ta duyệt $i$ trong đoạn $[0, n - 3]$, duyệt $j$ trong đoạn $[i + 1, n - 2]$, rồi binary search trong đoạn $[j + 1, n - 1]$ để tìm chỉ số đầu tiên $left$ sao cho giá trị tại đó lớn hơn hoặc bằng $nums[i] + nums[j]$. Khi đó, các giá trị của $k$ trong đoạn $[j + 1, left - 1]$ thỏa mãn điều kiện, nên ta cộng số lượng này vào kết quả $\textit{ans}$.

Độ phức tạp thời gian là $O(n^2\log n)$, còn độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def triangleNumber(self, nums: List[int]) -> int:
        nums.sort()
        ans, n = 0, len(nums)
        for i in range(n - 2):
            for j in range(i + 1, n - 1):
                k = bisect_left(nums, nums[i] + nums[j], lo=j + 1) - 1
                ans += k - j
        return ans
```

#### Java

```java
class Solution {
    public int triangleNumber(int[] nums) {
        Arrays.sort(nums);
        int ans = 0;
        for (int i = 0, n = nums.length; i < n - 2; ++i) {
            for (int j = i + 1; j < n - 1; ++j) {
                int left = j + 1, right = n;
                while (left < right) {
                    int mid = (left + right) >> 1;
                    if (nums[mid] >= nums[i] + nums[j]) {
                        right = mid;
                    } else {
                        left = mid + 1;
                    }
                }
                ans += left - j - 1;
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
    int triangleNumber(vector<int>& nums) {
        ranges::sort(nums);
        int ans = 0, n = nums.size();
        for (int i = 0; i < n - 2; ++i) {
            for (int j = i + 1; j < n - 1; ++j) {
                int sum = nums[i] + nums[j];
                auto it = ranges::lower_bound(nums.begin() + j + 1, nums.end(), sum);
                int k = int(it - nums.begin()) - 1;
                ans += max(0, k - j);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func triangleNumber(nums []int) int {
	sort.Ints(nums)
	n := len(nums)
	ans := 0
	for i := 0; i < n-2; i++ {
		for j := i + 1; j < n-1; j++ {
			sum := nums[i] + nums[j]
			k := sort.SearchInts(nums[j+1:], sum) + j + 1 - 1
			if k > j {
				ans += k - j
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function triangleNumber(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i < n - 2; i++) {
        for (let j = i + 1; j < n - 1; j++) {
            const sum = nums[i] + nums[j];
            let k = _.sortedIndex(nums, sum, j + 1) - 1;
            if (k > j) {
                ans += k - j;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn triangle_number(mut nums: Vec<i32>) -> i32 {
        nums.sort();
        let n = nums.len();
        let mut ans = 0;
        for i in 0..n.saturating_sub(2) {
            for j in i + 1..n.saturating_sub(1) {
                let sum = nums[i] + nums[j];
                let mut left = j + 1;
                let mut right = n;
                while left < right {
                    let mid = (left + right) / 2;
                    if nums[mid] < sum {
                        left = mid + 1;
                    } else {
                        right = mid;
                    }
                }
                if left > j + 1 {
                    ans += (left - 1 - j) as i32;
                }
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
