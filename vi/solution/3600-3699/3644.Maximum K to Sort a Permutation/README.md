---
comments: true
difficulty: Medium
rating: 1775
source: Weekly Contest 462 Q2
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [3644. Maximum K to Sort a Permutation](https://leetcode.com/problems/maximum-k-to-sort-a-permutation)

[中文文档](/solution/3600-3699/3644.Maximum%20K%20to%20Sort%20a%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>, trong đó <code>nums</code> là một <strong><span data-keyword="permutation-array">hoán vị</span></strong> của các số trong khoảng <code>[0..n - 1]</code>.</p>

<p>Bạn chỉ được phép hoán đổi các phần tử tại các chỉ số <code>i</code> và <code>j</code> <strong>khi và chỉ khi</strong> <code>nums[i] AND nums[j] == k</code>, trong đó <code>AND</code> là phép AND theo bit và <code>k</code> là một số nguyên <strong>không âm</strong>.</p>

<p>Trả về giá trị <strong>lớn nhất</strong> của <code>k</code> sao cho có thể sắp xếp mảng theo thứ tự <strong>không giảm</strong> bằng cách sử dụng một số lần hoán đổi bất kỳ như trên. Nếu <code>nums</code> đã được sắp xếp, trả về 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,3,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn <code>k = 1</code>. Phép hoán đổi giữa <code>nums[1] = 3</code> và <code>nums[3] = 1</code> được phép vì <code>nums[1] AND nums[3] == 1</code>, kết quả là hoán vị đã được sắp xếp: <code>[0, 1, 2, 3]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn <code>k = 2</code>. Phép hoán đổi giữa <code>nums[2] = 3</code> và <code>nums[3] = 2</code> được phép vì <code>nums[2] AND nums[3] == 2</code>, kết quả là hoán vị đã được sắp xếp: <code>[0, 1, 2, 3]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,2,1,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có <code>k = 0</code> cho phép sắp xếp, vì không có <code>k</code> nào lớn hơn cho phép thực hiện các phép hoán đổi cần thiết với điều kiện <code>nums[i] AND nums[j] == k</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= n - 1</code></li>
	<li><code>nums</code> là một hoán vị của các số nguyên từ <code>0</code> đến <code>n - 1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể hoán đổi các chỉ số (hoặc giá trị) mà phép AND theo bit của chúng bằng $k$ cho đến khi hoán vị được sắp xếp. Các giá trị đang ở sai vị trí phải luôn có thể tiếp cận nhau dưới điều kiện AND đó.
>
> Mỗi phép hoán đổi bảo toàn các bit chung với $k$. Phép AND của mọi giá trị đang ở sai vị trí là $k$ lớn nhất có thể: một $k$ lớn hơn sẽ yêu cầu một bit bằng $1$ mà một trong các giá trị sai vị trí không có.
>
> Hãy AND các giá trị đang ở sai vị trí vào $\textit{ans}$. Nếu không có giá trị nào sai vị trí, $k=0$. Khởi tạo $\textit{ans}$ bằng $-1$ giúp bắt đầu phép AND từ một giá trị toàn bit 1.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortPermutation(self, nums: List[int]) -> int:
        ans = -1
        for i, x in enumerate(nums):
            if i != x:
                ans &= x
        return max(ans, 0)
```

#### Java

```java
class Solution {
    public int sortPermutation(int[] nums) {
        int ans = -1;
        for (int i = 0; i < nums.length; ++i) {
            if (i != nums[i]) {
                ans &= nums[i];
            }
        }
        return Math.max(ans, 0);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sortPermutation(vector<int>& nums) {
        int ans = -1;
        for (int i = 0; i < nums.size(); ++i) {
            if (i != nums[i]) {
                ans &= nums[i];
            }
        }
        return max(ans, 0);
    }
};
```

#### Go

```go
func sortPermutation(nums []int) int {
	ans := -1
	for i, x := range nums {
		if i != x {
			ans &= x
		}
	}
	return max(ans, 0)
}
```

#### TypeScript

```ts
function sortPermutation(nums: number[]): number {
    let ans = -1;
    for (let i = 0; i < nums.length; ++i) {
        if (i != nums[i]) {
            ans &= nums[i];
        }
    }
    return Math.max(ans, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
