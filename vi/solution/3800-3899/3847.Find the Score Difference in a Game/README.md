---
comments: true
difficulty: Medium
rating: 1223
source: Weekly Contest 490 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [3847. Find the Score Difference in a Game](https://leetcode.com/problems/find-the-score-difference-in-a-game)

[中文文档](/solution/3800-3899/3847.Find%20the%20Score%20Difference%20in%20a%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>, trong đó <code>nums[i]</code> biểu thị số điểm ghi được trong trò chơi thứ <code>i<sup>th</sup></code>.</p>

<p>Có <strong>chính xác </strong>hai người chơi. Ban đầu, người chơi thứ nhất là người chơi <strong>active</strong> và người chơi thứ hai là người chơi <strong>inactive</strong>.</p>

<p>Các quy tắc sau được áp dụng <strong>theo thứ tự</strong> cho mỗi trò chơi <code>i</code>:</p>

<ul>
	<li>Nếu <code>nums[i]</code> là số lẻ, người chơi active và inactive đổi vai cho nhau.</li>
	<li>Trong mỗi trò chơi thứ 6 (tức là các chỉ số trò chơi <code>5, 11, 17, ...</code>), người chơi active và inactive đổi vai cho nhau.</li>
	<li>Người chơi active chơi trò chơi thứ <code>i<sup>th</sup></code> và nhận <code>nums[i]</code> điểm.</li>
</ul>

<p>Trả về <strong>hiệu điểm</strong>, được định nghĩa là <strong>tổng</strong> điểm của người chơi thứ nhất <strong>trừ đi</strong> <strong>tổng</strong> điểm của người chơi thứ hai.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Trò chơi 0: Vì số điểm là số lẻ, người chơi thứ hai trở thành người chơi active và nhận <code>nums[0] = 1</code> điểm.</li>
	<li>Trò chơi 1: Không có việc đổi vai. Người chơi thứ hai nhận <code>nums[1] = 2</code> điểm.</li>
	<li>Trò chơi 2: Vì số điểm là số lẻ, người chơi thứ nhất trở thành người chơi active và nhận <code>nums[2] = 3</code> điểm.</li>
	<li>Hiệu điểm là <code>3 - 3 = 0</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,4,2,1,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các trò chơi từ 0 đến 2: Người chơi thứ nhất nhận <code>2 + 4 + 2 = 8</code> điểm.</li>
	<li>Trò chơi 3: Vì số điểm là số lẻ, người chơi thứ hai trở thành người chơi active và nhận <code>nums[3] = 1</code> điểm.</li>
	<li>Trò chơi 4: Người chơi thứ hai nhận <code>nums[4] = 2</code> điểm.</li>
	<li>Trò chơi 5: Vì số điểm là số lẻ, hai người chơi đổi vai. Sau đó, vì đây là trò chơi thứ 6, hai người chơi lại đổi vai một lần nữa. Người chơi thứ hai nhận <code>nums[5] = 1</code> điểm.</li>
	<li>Hiệu điểm là <code>8 - 4 = 4</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Trò chơi 0: Vì số điểm là số lẻ, người chơi thứ hai trở thành người chơi active và nhận <code>nums[0] = 1</code> điểm.</li>
	<li>Hiệu điểm là <code>0 - 1 = -1</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Người chơi active ghi điểm; nếu giá trị là số lẻ hoặc đây là trò chơi thứ sáu thì hai người chơi đổi vai. Ta cần hiệu giữa điểm của người chơi thứ nhất và người chơi thứ hai. Vì $n \le 1000$, ta mô phỏng trực tiếp.
>
> Một dấu $k=\pm 1$ ghi lại việc người chơi thứ nhất hiện có đang active hay không; ta cộng $k$ nhân với số điểm.
>
> Đổi dấu $k$ khi giá trị là số lẻ, đổi dấu một lần nữa khi $i \bmod 6=5$, rồi cộng $k \cdot x$ vào kết quả.
>
> Thứ tự này khớp với đề bài: kiểm tra số lẻ, kiểm tra trò chơi thứ sáu, rồi ghi điểm.

<!-- thinking:end -->

Ta dùng biến $k$ để biểu diễn vai trò của người chơi hiện tại. Ban đầu $k = 1$, khi $k = 1$ nghĩa là người chơi thứ nhất đang active, còn khi $k = -1$ nghĩa là người chơi thứ hai đang active. Với mỗi trò chơi, ta cập nhật giá trị của $k$ theo mô tả của đề bài, rồi cộng điểm của trò chơi hiện tại nhân với $k$ vào đáp án. Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def scoreDifference(self, nums: List[int]) -> int:
        ans, k = 0, 1
        for i, x in enumerate(nums):
            if x % 2:
                k *= -1
            if i % 6 == 5:
                k *= -1
            ans += k * x
        return ans
```

#### Java

```java
class Solution {
    public int scoreDifference(int[] nums) {
        int ans = 0;
        int k = 1;
        for (int i = 0; i < nums.length; ++i) {
            int x = nums[i];
            if ((x & 1) == 1) {
                k = -k;
            }
            if (i % 6 == 5) {
                k = -k;
            }
            ans += k * x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int scoreDifference(vector<int>& nums) {
        int ans = 0;
        int k = 1;
        for (int i = 0; i < nums.size(); ++i) {
            int x = nums[i];
            if (x & 1) {
                k = -k;
            }
            if (i % 6 == 5) {
                k = -k;
            }
            ans += k * x;
        }
        return ans;
    }
};
```

#### Go

```go
func scoreDifference(nums []int) int {
	ans := 0
	k := 1
	for i, x := range nums {
		if x%2 != 0 {
			k = -k
		}
		if i%6 == 5 {
			k = -k
		}
		ans += k * x
	}
	return ans
}
```

#### TypeScript

```ts
function scoreDifference(nums: number[]): number {
    let ans = 0;
    let k = 1;

    nums.forEach((x, i) => {
        if (x % 2 !== 0) {
            k = -k;
        }
        if (i % 6 === 5) {
            k = -k;
        }
        ans += k * x;
    });

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
