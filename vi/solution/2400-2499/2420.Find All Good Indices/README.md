---
comments: true
difficulty: Medium
rating: 1695
source: Weekly Contest 312 Q3
tags:
    - Array
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [2420. Find All Good Indices](https://leetcode.com/problems/find-all-good-indices)

[中文文档](/solution/2400-2499/2420.Find%20All%20Good%20Indices/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>, có kích thước <code>n</code>, và một số nguyên dương <code>k</code>.</p>

<p>Một chỉ số <code>i</code> trong khoảng <code>k &lt;= i &lt; n - k</code> được gọi là <strong>tốt</strong> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li><code>k</code> phần tử ngay <strong>trước</strong> chỉ số <code>i</code> được sắp xếp theo thứ tự <strong>không tăng</strong>.</li>
	<li><code>k</code> phần tử ngay <strong>sau</strong> chỉ số <code>i</code> được sắp xếp theo thứ tự <strong>không giảm</strong>.</li>
</ul>

<p>Hãy trả về <em>một mảng chứa tất cả các chỉ số tốt theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,1,1,3,4,1], k = 2
<strong>Đầu ra:</strong> [2,3]
<strong>Giải thích:</strong> Có hai chỉ số tốt trong mảng:
- Chỉ số 2. Subarray [2,1] được sắp xếp theo thứ tự không tăng, còn subarray [1,3] được sắp xếp theo thứ tự không giảm.
- Chỉ số 3. Subarray [1,1] được sắp xếp theo thứ tự không tăng, còn subarray [3,4] được sắp xếp theo thứ tự không giảm.
Lưu ý rằng chỉ số 4 không tốt vì [4,1] không được sắp xếp theo thứ tự không giảm.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,1,2], k = 2
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Không có chỉ số tốt nào trong mảng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>3 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= k &lt;= n / 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 10^5$, nếu kiểm tra $k$ phần tử lân cận ở mỗi bên của từng chỉ số thì độ phức tạp có thể lên tới $O(nk)$. Hai phía độc lập với nhau, nên ta tiền xử lý đoạn không tăng kết thúc tại $i-1$ và đoạn không giảm bắt đầu tại $i+1$.
>
> Ta điền $\textit{decr}$ từ trái sang phải và $\textit{incr}$ từ phải sang trái, sau đó chấp nhận $i\in[k,n-k)$ khi cả hai đoạn đều có độ dài ít nhất $k$. Cả hai phía đều không tăng, tương ứng với hai phép so sánh $\le$ trong code.

<!-- thinking:end -->

Ta định nghĩa hai mảng `decr` và `incr`, lần lượt biểu diễn độ dài của subarray không tăng và không giảm dài nhất khi duyệt từ trái sang phải và từ phải sang trái.

Ta duyệt mảng, cập nhật các mảng `decr` và `incr`.

Sau đó, ta lần lượt duyệt chỉ số $i$ (với $k\le i \lt n - k$). Nếu $decr[i] \geq k$ và $incr[i] \geq k$ thì $i$ là một chỉ số tốt.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def goodIndices(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        decr = [1] * (n + 1)
        incr = [1] * (n + 1)
        for i in range(2, n - 1):
            if nums[i - 1] <= nums[i - 2]:
                decr[i] = decr[i - 1] + 1
        for i in range(n - 3, -1, -1):
            if nums[i + 1] <= nums[i + 2]:
                incr[i] = incr[i + 1] + 1
        return [i for i in range(k, n - k) if decr[i] >= k and incr[i] >= k]
```

#### Java

```java
class Solution {
    public List<Integer> goodIndices(int[] nums, int k) {
        int n = nums.length;
        int[] decr = new int[n];
        int[] incr = new int[n];
        Arrays.fill(decr, 1);
        Arrays.fill(incr, 1);
        for (int i = 2; i < n - 1; ++i) {
            if (nums[i - 1] <= nums[i - 2]) {
                decr[i] = decr[i - 1] + 1;
            }
        }
        for (int i = n - 3; i >= 0; --i) {
            if (nums[i + 1] <= nums[i + 2]) {
                incr[i] = incr[i + 1] + 1;
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = k; i < n - k; ++i) {
            if (decr[i] >= k && incr[i] >= k) {
                ans.add(i);
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
    vector<int> goodIndices(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> decr(n, 1);
        vector<int> incr(n, 1);
        for (int i = 2; i < n; ++i) {
            if (nums[i - 1] <= nums[i - 2]) {
                decr[i] = decr[i - 1] + 1;
            }
        }
        for (int i = n - 3; ~i; --i) {
            if (nums[i + 1] <= nums[i + 2]) {
                incr[i] = incr[i + 1] + 1;
            }
        }
        vector<int> ans;
        for (int i = k; i < n - k; ++i) {
            if (decr[i] >= k && incr[i] >= k) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func goodIndices(nums []int, k int) []int {
	n := len(nums)
	decr := make([]int, n)
	incr := make([]int, n)
	for i := range decr {
		decr[i] = 1
		incr[i] = 1
	}
	for i := 2; i < n; i++ {
		if nums[i-1] <= nums[i-2] {
			decr[i] = decr[i-1] + 1
		}
	}
	for i := n - 3; i >= 0; i-- {
		if nums[i+1] <= nums[i+2] {
			incr[i] = incr[i+1] + 1
		}
	}
	ans := []int{}
	for i := k; i < n-k; i++ {
		if decr[i] >= k && incr[i] >= k {
			ans = append(ans, i)
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
