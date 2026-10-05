---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [3730. Maximum Calories Burnt from Jumps 🔒](https://leetcode.com/problems/maximum-calories-burnt-from-jumps)

[Tài liệu tiếng Trung](/solution/3700-3799/3730.Maximum%20Calories%20Burnt%20from%20Jumps/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>heights</code> có kích thước <code>n</code>, trong đó <code>heights[i]</code> biểu diễn chiều cao của khối thứ <code>i<sup>th</sup></code> trong một bài tập.</p>

<p>Bạn bắt đầu trên mặt đất (chiều cao 0) và <strong>phải</strong> nhảy lên mỗi khối <strong>đúng một lần</strong> theo bất kỳ thứ tự nào.</p>

<ul>
	<li><strong>Lượng calo đốt cháy</strong> khi nhảy từ khối có chiều cao <code>a</code> đến khối có chiều cao <code>b</code> là <code>(a - b)<sup>2</sup></code>.</li>
	<li><strong>Lượng calo đốt cháy</strong> khi thực hiện cú nhảy đầu tiên từ mặt đất đến khối đầu tiên được chọn <code>heights[i]</code> là <code>(0 - heights[i])<sup>2</sup></code>.</li>
</ul>

<p>Hãy trả về <strong>tổng lượng calo lớn nhất</strong> mà bạn có thể đốt cháy bằng cách chọn một thứ tự nhảy tối ưu.</p>

<p><strong>Lưu ý:</strong> Sau khi nhảy lên khối đầu tiên, bạn không thể quay lại mặt đất.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">heights = [1,7,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">181</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<p>Thứ tự tối ưu là <code>[9, 1, 7]</code>.</p>

<ul>
	<li>Cú nhảy đầu tiên từ mặt đất đến <code>heights[2] = 9</code>: <code>(0 - 9)<sup>2</sup> = 81</code>.</li>
	<li>Cú nhảy tiếp theo đến <code>heights[0] = 1</code>: <code>(9 - 1)<sup>2</sup> = 64</code>.</li>
	<li>Cú nhảy cuối cùng đến <code>heights[1] = 7</code>: <code>(1 - 7)<sup>2</sup> = 36</code>.</li>
</ul>

<p>Tổng lượng calo đốt cháy = <code>81 + 64 + 36 = 181</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">heights = [5,2,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">38</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thứ tự tối ưu là <code>[5, 2, 4]</code>.</p>

<ul>
	<li>Cú nhảy đầu tiên từ mặt đất đến <code>heights[0] = 5</code>: <code>(0 - 5)<sup>2</sup> = 25</code>.</li>
	<li>Cú nhảy tiếp theo đến <code>heights[1] = 2</code>: <code>(5 - 2)<sup>2</sup> = 9</code>.</li>
	<li>Cú nhảy cuối cùng đến <code>heights[2] = 4</code>: <code>(2 - 4)<sup>2</sup> = 4</code>.</li>
</ul>

<p>Tổng lượng calo đốt cháy = <code>25 + 9 + 4 = 38</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">heights = [3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thứ tự tối ưu là <code>[3, 3]</code>.</p>

<ul>
	<li>Cú nhảy đầu tiên từ mặt đất đến <code>heights[0] = 3</code>: <code>(0 - 3)<sup>2</sup> = 9</code>.</li>
	<li>Cú nhảy tiếp theo đến <code>heights[1] = 3</code>: <code>(3 - 3)<sup>2</sup> = 0</code>.</li>
</ul>

<p>Tổng lượng calo đốt cháy = <code>9 + 0 = 9</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == heights.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= heights[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Chi phí là bình phương chênh lệch chiều cao, bắt đầu từ $0$ và không bao giờ quay lại mặt đất. Bình phương của các khoảng chênh lệch lớn có lợi hơn, vì vậy sau khi sắp xếp, ta lần lượt nhảy qua lại giữa khối cao nhất và khối thấp nhất chưa sử dụng để duy trì mỗi lần hạ thấp có khoảng chênh lệch lớn.

<!-- thinking:end -->

Theo đề bài, thứ tự các cú nhảy ảnh hưởng đến tổng lượng calo đốt cháy. Để tối đa hóa lượng calo, ta có thể sử dụng chiến lược tham lam bằng cách ưu tiên các cú nhảy có chênh lệch chiều cao lớn nhất.

Do đó, trước tiên ta sắp xếp chiều cao các khối, sau đó bắt đầu nhảy từ khối cao nhất, rồi đến khối thấp nhất, cứ tiếp tục như vậy cho đến khi đã nhảy lên tất cả các khối.

Các bước cụ thể như sau:

1. Sắp xếp mảng $\text{heights}$.
1. Khởi tạo biến $\text{pre} = 0$ để biểu diễn chiều cao của khối trước đó, và $\text{ans} = 0$ để biểu diễn tổng lượng calo đã đốt cháy.
1. Sử dụng hai con trỏ: con trỏ trái $\text{l}$ trỏ đến đầu mảng, còn con trỏ phải $\text{r}$ trỏ đến cuối mảng.
1. Khi $\text{l} < \text{r}$, thực hiện các bước sau:
    1. Tính lượng calo đốt cháy khi nhảy từ khối trước đó đến khối được con trỏ phải trỏ tới và cộng vào $\text{ans}$.
    1. Tính lượng calo đốt cháy khi nhảy từ khối được con trỏ phải trỏ tới đến khối được con trỏ trái trỏ tới và cộng vào $\text{ans}$.
    1. Cập nhật $\text{pre}$ thành chiều cao của khối được con trỏ trái trỏ tới.
    1. Di chuyển con trỏ trái một bước sang phải và con trỏ phải một bước sang trái.
1. Cuối cùng, tính lượng calo đốt cháy khi nhảy từ khối trước đó đến khối ở giữa và cộng vào $\text{ans}$.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxCaloriesBurnt(self, heights: list[int]) -> int:
        heights.sort()
        pre = 0
        l, r = 0, len(heights) - 1
        ans = 0
        while l < r:
            ans += (heights[r] - pre) ** 2
            ans += (heights[l] - heights[r]) ** 2
            pre = heights[l]
            l, r = l + 1, r - 1
        ans += (heights[r] - pre) ** 2
        return ans
```

#### Java

```java
class Solution {
    public long maxCaloriesBurnt(int[] heights) {
        Arrays.sort(heights);
        long ans = 0;
        int pre = 0;
        int r = heights.length - 1;
        for (int l = 0; l < r; ++l, --r) {
            ans += 1L * (heights[r] - pre) * (heights[r] - pre);
            ans += 1L * (heights[l] - heights[r]) * (heights[l] - heights[r]);
            pre = heights[l];
        }
        ans += 1L * (heights[r] - pre) * (heights[r] - pre);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxCaloriesBurnt(vector<int>& heights) {
        ranges::sort(heights);
        long long ans = 0;
        int pre = 0;
        int r = heights.size() - 1;
        for (int l = 0; l < r; ++l, --r) {
            ans += 1LL * (heights[r] - pre) * (heights[r] - pre);
            ans += 1LL * (heights[l] - heights[r]) * (heights[l] - heights[r]);
            pre = heights[l];
        }
        ans += 1LL * (heights[r] - pre) * (heights[r] - pre);
        return ans;
    }
};
```

#### Go

```go
func maxCaloriesBurnt(heights []int) (ans int64) {
	sort.Ints(heights)
	pre := 0
	l, r := 0, len(heights)-1
	for l < r {
		ans += int64(heights[r]-pre) * int64(heights[r]-pre)
		ans += int64(heights[l]-heights[r]) * int64(heights[l]-heights[r])
		pre = heights[l]
		l++
		r--
	}
	ans += int64(heights[r]-pre) * int64(heights[r]-pre)
	return
}
```

#### TypeScript

```ts
function maxCaloriesBurnt(heights: number[]): number {
    heights.sort((a, b) => a - b);
    let ans = 0;
    let pre = 0;
    let [l, r] = [0, heights.length - 1];
    while (l < r) {
        ans += (heights[r] - pre) ** 2;
        ans += (heights[l] - heights[r]) ** 2;
        pre = heights[l];
        l++;
        r--;
    }
    ans += (heights[r] - pre) ** 2;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
