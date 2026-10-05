---
comments: true
difficulty: Easy
rating: 1171
source: Weekly Contest 488 Q1
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [3833. Count Dominant Indices](https://leetcode.com/problems/count-dominant-indices)

[中文文档](/solution/3800-3899/3833.Count%20Dominant%20Indices/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Một phần tử tại chỉ số <code>i</code> được gọi là <strong>dominant</strong> nếu: <code>nums[i] &gt; average(nums[i + 1], nums[i + 2], ..., nums[n - 1])</code></p>

<p>Nhiệm vụ của bạn là đếm số chỉ số <code>i</code> là <strong>dominant</strong>.</p>

<p><strong>Trung bình cộng</strong> của một tập hợp các số là giá trị nhận được bằng cách cộng tất cả các số rồi chia tổng đó cho tổng số lượng các số.</p>

<p><strong>Lưu ý</strong>: Phần tử <strong>ngoài cùng bên phải</strong> của mọi mảng <strong>không</strong> phải là <strong>dominant</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,4,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tại chỉ số <code>i = 0</code>, giá trị 5 là dominant vì <code>5 &gt; average(4, 3) = 3.5</code>.</li>
	<li>Tại chỉ số <code>i = 1</code>, giá trị 4 là dominant so với mảng con <code>[3]</code>.</li>
	<li>Chỉ số <code>i = 2</code> không dominant vì không có phần tử nào bên phải nó. Do đó, đáp án là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tại chỉ số <code>i = 0</code>, giá trị 4 là dominant so với mảng con <code>[1, 2]</code>.</li>
	<li>Tại chỉ số <code>i = 1</code>, giá trị 1 không dominant.</li>
	<li>Chỉ số <code>i = 2</code> không dominant vì không có phần tử nào bên phải nó. Do đó, đáp án là 1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt ngược

<!-- thinking:start -->

> **Tư duy**
>
> Một chỉ số dominant có giá trị lớn hơn nghiêm ngặt trung bình của hậu tố bên phải; chỉ số cuối bị loại. Với $n \le 100$, ta có thể tính lại từng hậu tố, nhưng các tổng bị chồng lặp.
>
> Trung bình của hậu tố chỉ phụ thuộc vào tổng và độ dài hậu tố.
>
> Ta duyệt từ phải sang trái với tổng hậu tố đang được duy trì $\textit{suf}$, so sánh $nums[i]$ với $\textit{suf}/(n-i-1)$, rồi gộp $nums[i]$ vào hậu tố.
>
> Một lượt duyệt ngược là đủ để quyết định cho mọi chỉ số.

<!-- thinking:end -->

Ta có thể duyệt mảng từ cuối về đầu, duy trì tổng hậu tố $\text{suf}$, biểu thị tổng của tất cả phần tử bên phải phần tử hiện tại. Với mỗi phần tử, ta kiểm tra xem nó có lớn hơn giá trị trung bình của các phần tử bên phải hay không, tức $\frac{\text{suf}}{n - i - 1}$. Nếu có, ta tăng đáp án thêm một. Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\text{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def dominantIndices(self, nums: List[int]) -> int:
        n = len(nums)
        ans = 0
        suf = nums[-1]
        for i in range(n - 2, -1, -1):
            if nums[i] > suf / (n - i - 1):
                ans += 1
            suf += nums[i]
        return ans
```

#### Java

```java
class Solution {
    public int dominantIndices(int[] nums) {
        int n = nums.length;
        int ans = 0;
        int suf = nums[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            if (nums[i] * (n - i - 1) > suf) {
                ans++;
            }
            suf += nums[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int dominantIndices(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;
        int suf = nums[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            if (nums[i] * (n - i - 1) > suf) {
                ans++;
            }
            suf += nums[i];
        }
        return ans;
    }
};
```

#### Go

```go
func dominantIndices(nums []int) int {
	n := len(nums)
	ans := 0
	suf := nums[n-1]
	for i := n - 2; i >= 0; i-- {
		if nums[i]*(n-i-1) > suf {
			ans++
		}
		suf += nums[i]
	}
	return ans
}
```

#### TypeScript

```ts
function dominantIndices(nums: number[]): number {
    const n = nums.length;
    let ans = 0;
    let suf = nums[n - 1];
    for (let i = n - 2; i >= 0; --i) {
        if (nums[i] * (n - i - 1) > suf) {
            ans++;
        }
        suf += nums[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
