---
comments: true
difficulty: Easy
rating: 1201
source: Biweekly Contest 124 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [3038. Maximum Number of Operations With the Same Score I](https://leetcode.com/problems/maximum-number-of-operations-with-the-same-score-i)

[中文文档](/solution/3000-3099/3038.Maximum%20Number%20of%20Operations%20With%20the%20Same%20Score%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>. Xét thao tác sau:</p>

<ul>
	<li>Xóa hai phần tử đầu tiên của <code>nums</code> và định nghĩa <em>điểm số</em> của thao tác là tổng của hai phần tử này.</li>
</ul>

<p>Bạn có thể thực hiện thao tác này cho đến khi <code>nums</code> còn ít hơn hai phần tử. Ngoài ra, phải đạt được <strong>cùng một</strong> <em>điểm số</em> trong <strong>tất cả</strong> các thao tác.</p>

<p>Hãy trả về số thao tác <strong>lớn nhất</strong> mà bạn có thể thực hiện.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,2,1,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ta có thể thực hiện thao tác đầu tiên với điểm số <code>3 + 2 = 5</code>. Sau thao tác này, <code>nums = [1,4,5]</code>.</li>
	<li>Ta có thể thực hiện thao tác thứ hai vì điểm số của nó là <code>4 + 1 = 5</code>, giống với thao tác trước đó. Sau thao tác này, <code>nums = [5]</code>.</li>
	<li>Vì còn ít hơn hai phần tử nên ta không thể thực hiện thêm thao tác nào.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,5,3,3,4,1,3,2,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ta có thể thực hiện thao tác đầu tiên với điểm số <code>1 + 5 = 6</code>. Sau thao tác này, <code>nums = [3,3,4,1,3,2,2,3]</code>.</li>
	<li>Ta có thể thực hiện thao tác thứ hai vì điểm số của nó là <code>3 + 3 = 6</code>, giống với thao tác trước đó. Sau thao tác này, <code>nums = [4,1,3,2,2,3]</code>.</li>
	<li>Ta không thể thực hiện thao tác tiếp theo vì điểm số của nó là <code>4 + 1 = 5</code>, khác với các điểm số trước đó.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác lấy hai số còn lại đầu tiên và phải giữ nguyên điểm số đầu tiên. Vì $n \le 100$, ta mô phỏng quy tắc này.
>
> Tổng đầu tiên $s$ cố định tất cả các thao tác sau đó. Ta dừng khi còn ít hơn hai phần tử hoặc tổng không còn bằng $s$.
>
> Duyệt với bước nhảy $2$ sẽ đếm được số thao tác.

<!-- thinking:end -->

Trước tiên, ta tính tổng của hai phần tử đầu tiên, ký hiệu là $s$. Sau đó, ta duyệt mảng và lấy hai phần tử trong mỗi lần. Nếu tổng của chúng không bằng $s$, ta dừng quá trình duyệt. Cuối cùng, ta trả về số thao tác đã thực hiện.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxOperations(self, nums: List[int]) -> int:
        s = nums[0] + nums[1]
        ans, n = 0, len(nums)
        for i in range(0, n, 2):
            if i + 1 == n or nums[i] + nums[i + 1] != s:
                break
            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int maxOperations(int[] nums) {
        int s = nums[0] + nums[1];
        int ans = 0, n = nums.length;
        for (int i = 0; i + 1 < n && nums[i] + nums[i + 1] == s; i += 2) {
            ++ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxOperations(vector<int>& nums) {
        int s = nums[0] + nums[1];
        int ans = 0, n = nums.size();
        for (int i = 0; i + 1 < n && nums[i] + nums[i + 1] == s; i += 2) {
            ++ans;
        }
        return ans;
    }
};
```

#### Go

```go
func maxOperations(nums []int) (ans int) {
	s, n := nums[0]+nums[1], len(nums)
	for i := 0; i+1 < n && nums[i]+nums[i+1] == s; i += 2 {
		ans++
	}
	return
}
```

#### TypeScript

```ts
function maxOperations(nums: number[]): number {
    const s = nums[0] + nums[1];
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i + 1 < n && nums[i] + nums[i + 1] === s; i += 2) {
        ++ans;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
