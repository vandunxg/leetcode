---
comments: true
difficulty: Medium
rating: 1653
source: Biweekly Contest 30 Q3
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [1509. Minimum Difference Between Largest and Smallest Value in Three Moves](https://leetcode.com/problems/minimum-difference-between-largest-and-smallest-value-in-three-moves)

[中文文档](/solution/1500-1599/1509.Minimum%20Difference%20Between%20Largest%20and%20Smallest%20Value%20in%20Three%20Moves/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Trong một lần thay đổi, bạn có thể chọn một phần tử của <code>nums</code> và đổi nó thành <strong>bất kỳ giá trị nào</strong>.</p>

<p>Trả về <em>độ chênh lệch nhỏ nhất giữa giá trị lớn nhất và nhỏ nhất của <code>nums</code> <strong>sau khi thực hiện nhiều nhất ba lần thay đổi</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,3,2,4]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Ta có thể thực hiện nhiều nhất 3 lần thay đổi.
Trong lần thay đổi đầu tiên, đổi 2 thành 3. nums trở thành [5,3,3,4].
Trong lần thay đổi thứ hai, đổi 4 thành 3. nums trở thành [5,3,3,3].
Trong lần thay đổi thứ ba, đổi 5 thành 3. nums trở thành [3,3,3,3].
Sau 3 lần thay đổi, độ chênh lệch giữa giá trị nhỏ nhất và lớn nhất là 3 - 3 = 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5,0,10,14]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể thực hiện nhiều nhất 3 lần thay đổi.
Trong lần thay đổi đầu tiên, đổi 5 thành 0. nums trở thành [1,0,0,10,14].
Trong lần thay đổi thứ hai, đổi 10 thành 0. nums trở thành [1,0,0,0,14].
Trong lần thay đổi thứ ba, đổi 14 thành 1. nums trở thành [1,0,0,0,1].
Sau 3 lần thay đổi, độ chênh lệch giữa giá trị nhỏ nhất và lớn nhất là 1 - 0 = 1.
Có thể chứng minh rằng không có cách nào làm cho độ chênh lệch bằng 0 trong 3 lần thay đổi.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,100,20]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Ta có thể thực hiện nhiều nhất 3 lần thay đổi.
Trong lần thay đổi đầu tiên, đổi 100 thành 7. nums trở thành [3,7,20].
Trong lần thay đổi thứ hai, đổi 20 thành 7. nums trở thành [3,7,7].
Trong lần thay đổi thứ ba, đổi 3 thành 7. nums trở thành [7,7,7].
Sau 3 lần thay đổi, độ chênh lệch giữa giá trị nhỏ nhất và lớn nhất là 7 - 7 = 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể thay đổi nhiều nhất ba số để tối thiểu hóa khoảng cách giữa giá trị lớn nhất và nhỏ nhất còn lại. Vì $n\le 10^5$, ta không thể duyệt qua mọi bộ ba. Nếu độ dài nhỏ hơn $5$, ba lần thay đổi chỉ còn lại nhiều nhất một giá trị có ý nghĩa, nên đáp án là $0$.
>
> Sau khi sắp xếp, các giá trị không bị thay đổi tạo thành một cửa sổ liên tiếp: ba lần thay đổi loại bỏ ba phần tử cực trị ở hai đầu. Ta thử loại bỏ $l\in\{0,1,2,3\}$ phần tử ở bên trái và $3-l$ phần tử ở bên phải, rồi lấy giá trị nhỏ nhất của $nums[n-1-r]-nums[l]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDifference(self, nums: List[int]) -> int:
        n = len(nums)
        if n < 5:
            return 0
        nums.sort()
        ans = inf
        for l in range(4):
            r = 3 - l
            ans = min(ans, nums[n - 1 - r] - nums[l])
        return ans
```

#### Java

```java
class Solution {
    public int minDifference(int[] nums) {
        int n = nums.length;
        if (n < 5) {
            return 0;
        }
        Arrays.sort(nums);
        long ans = 1L << 60;
        for (int l = 0; l <= 3; ++l) {
            int r = 3 - l;
            ans = Math.min(ans, (long) nums[n - 1 - r] - nums[l]);
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minDifference(vector<int>& nums) {
        int n = nums.size();
        if (n < 5) {
            return 0;
        }
        sort(nums.begin(), nums.end());
        long long ans = 1L << 60;
        for (int l = 0; l <= 3; ++l) {
            int r = 3 - l;
            ans = min(ans, 1LL * nums[n - 1 - r] - nums[l]);
        }
        return ans;
    }
};
```

#### Go

```go
func minDifference(nums []int) int {
	n := len(nums)
	if n < 5 {
		return 0
	}
	sort.Ints(nums)
	ans := 1 << 60
	for l := 0; l <= 3; l++ {
		r := 3 - l
		ans = min(ans, nums[n-1-r]-nums[l])
	}
	return ans
}
```

#### TypeScript

```ts
function minDifference(nums: number[]): number {
    if (nums.length < 5) {
        return 0;
    }
    nums.sort((a, b) => a - b);
    let ans = Number.POSITIVE_INFINITY;
    for (let i = 0; i < 4; i++) {
        ans = Math.min(ans, nums.at(i - 4)! - nums[i]);
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var minDifference = function (nums) {
    if (nums.length < 5) {
        return 0;
    }
    nums.sort((a, b) => a - b);
    let ans = Number.POSITIVE_INFINITY;
    for (let i = 0; i < 4; i++) {
        ans = Math.min(ans, nums.at(i - 4) - nums[i]);
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
