---
comments: true
difficulty: Medium
rating: 1771
source: Weekly Contest 414 Q3
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [3282. Reach End of Array With Max Score](https://leetcode.com/problems/reach-end-of-array-with-max-score)

[中文文档](/solution/3200-3299/3282.Reach%20End%20of%20Array%20With%20Max%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Mục tiêu của bạn là bắt đầu từ chỉ số <code>0</code> và đến chỉ số <code>n - 1</code>. Bạn chỉ có thể nhảy đến các chỉ số <strong>lớn hơn</strong> chỉ số hiện tại.</p>

<p>Điểm số của một lần nhảy từ chỉ số <code>i</code> đến chỉ số <code>j</code> được tính bằng <code>(j - i) * nums[i]</code>.</p>

<p>Trả về <strong>tổng điểm</strong> <b>lớn nhất</b> có thể đạt được khi đến chỉ số cuối cùng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,1,5]</span></p>

<p><strong>Đầu ra:</strong> 7</p>

<p><strong>Giải thích:</strong></p>

<p>Đầu tiên, nhảy đến chỉ số 1, sau đó nhảy đến chỉ số cuối cùng. Điểm số cuối cùng là <code>1 * 1 + 2 * 3 = 7</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,3,1,3,2]</span></p>

<p><strong>Đầu ra:</strong> 16</p>

<p><strong>Giải thích:</strong></p>

<p>Nhảy trực tiếp đến chỉ số cuối cùng. Điểm số cuối cùng là <code>4 * 4 = 16</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Một bước nhảy $i\to j$ có điểm $(j-i)\times nums[i]$, tức là mỗi bước bị bỏ qua đều mang lại $nums[i]$ điểm. Vì $n\le 10^5$, việc nhảy đến một $nums[j]$ nhỏ hơn không thể tốt hơn việc tiếp tục dùng $nums[i]$ cho các bước đó, nên ta không bao giờ nhảy đến một giá trị nhỏ hơn.
>
> Duy trì giá trị lớn nhất $mx$ trên tiền tố và cộng nó tại mọi chỉ số trừ chỉ số cuối. Đây chính là điểm số khi luôn sử dụng giá trị lớn nhất đã gặp. Độ phức tạp thời gian là tuyến tính.

<!-- thinking:end -->

Giả sử ta nhảy từ chỉ số $i$ đến chỉ số $j$, khi đó điểm số là $(j - i) \times \text{nums}[i]$. Điều này tương đương với việc thực hiện $j - i$ bước, trong đó mỗi bước nhận được điểm số là $\text{nums}[i]$. Sau đó, ta tiếp tục nhảy từ $j$ đến chỉ số tiếp theo $k$, với điểm số là $(k - j) \times \text{nums}[j]$, và cứ tiếp tục như vậy. Nếu $\text{nums}[i] \gt \text{nums}[j]$, ta không nên nhảy từ $i$ đến $j$, vì điểm số nhận được theo cách này chắc chắn nhỏ hơn điểm số khi nhảy trực tiếp từ $i$ đến $k$. Do đó, mỗi lần ta nên nhảy đến chỉ số tiếp theo có giá trị lớn hơn chỉ số hiện tại.

Ta có thể duy trì một biến $mx$ biểu diễn giá trị lớn nhất của $\text{nums}[i]$ đã gặp. Sau đó, ta duyệt mảng từ trái sang phải cho đến phần tử áp chót, mỗi lần cập nhật $mx$ và cộng dồn điểm số.

Sau khi duyệt xong, kết quả là tổng điểm lớn nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\text{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaximumScore(self, nums: List[int]) -> int:
        ans = mx = 0
        for x in nums[:-1]:
            mx = max(mx, x)
            ans += mx
        return ans
```

#### Java

```java
class Solution {
    public long findMaximumScore(List<Integer> nums) {
        long ans = 0;
        int mx = 0;
        for (int i = 0; i + 1 < nums.size(); ++i) {
            mx = Math.max(mx, nums.get(i));
            ans += mx;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long findMaximumScore(vector<int>& nums) {
        long long ans = 0;
        int mx = 0;
        for (int i = 0; i + 1 < nums.size(); ++i) {
            mx = max(mx, nums[i]);
            ans += mx;
        }
        return ans;
    }
};
```

#### Go

```go
func findMaximumScore(nums []int) (ans int64) {
	mx := 0
	for _, x := range nums[:len(nums)-1] {
		mx = max(mx, x)
		ans += int64(mx)
	}
	return
}
```

#### TypeScript

```ts
function findMaximumScore(nums: number[]): number {
    let [ans, mx]: [number, number] = [0, 0];
    for (const x of nums.slice(0, -1)) {
        mx = Math.max(mx, x);
        ans += mx;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
