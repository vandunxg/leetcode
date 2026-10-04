---
comments: true
difficulty: Medium
rating: 1404
source: Weekly Contest 391 Q3
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3101. Count Alternating Subarrays](https://leetcode.com/problems/count-alternating-subarrays)

[中文文档](/solution/3100-3199/3101.Count%20Alternating%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một <span data-keyword="binary-array">mảng nhị phân</span> <code>nums</code>.</p>

<p>Một <span data-keyword="subarray-nonempty">mảng con</span> được gọi là <strong>xen kẽ</strong> nếu <strong>không có</strong> hai phần tử <strong>liền kề</strong> nào trong mảng con có <strong>cùng</strong> giá trị.</p>

<p>Trả về <em>số lượng mảng con xen kẽ trong </em><code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con xen kẽ là: <code>[0]</code>, <code>[1]</code>, <code>[1]</code>, <code>[1]</code> và <code>[0,1]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,0,1,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi mảng con của mảng đều xen kẽ. Có 10 mảng con có thể chọn.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums[i]</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Các phần tử kề nhau trong một mảng con xen kẽ có giá trị khác nhau. Nếu duyệt tất cả các đoạn $O(n^2)$ rồi kiểm tra từng đoạn, ta sẽ lặp lại cùng một phép kiểm tra các phần tử kề nhau.
>
> Các mảng con kết thúc tại $i$ hoặc là mảng chỉ gồm một phần tử $nums[i]$, hoặc là phần mở rộng của các mảng con kết thúc tại $i-1$ khi $nums[i] \neq nums[i-1]$. Hai giá trị bằng nhau sẽ ngắt mọi đoạn xen kẽ dài hơn.
>
> Ta duy trì độ dài xen kẽ $s$ kết thúc tại chỉ số hiện tại, tăng nó khi có sự thay đổi và đặt lại thành $1$ nếu không, sau đó cộng $s$ vào đáp án. Duyệt một lần là đủ để đếm mọi mảng con hợp lệ.

<!-- thinking:end -->

Ta có thể duyệt các mảng con kết thúc tại mỗi vị trí, tính số mảng con thỏa mãn điều kiện rồi cộng số lượng đó tại tất cả các vị trí.

Cụ thể, ta định nghĩa biến $s$ là số mảng con thỏa mãn điều kiện và kết thúc bằng phần tử $nums[i]$. Ban đầu, ta đặt $s$ bằng $1$, nghĩa là có $1$ mảng con thỏa mãn điều kiện và kết thúc bằng phần tử đầu tiên.

Tiếp theo, ta bắt đầu duyệt mảng từ phần tử thứ hai. Với mỗi vị trí $i$, ta cập nhật giá trị của $s$ dựa trên mối quan hệ giữa $nums[i]$ và $nums[i-1]$:

- Nếu $nums[i] \neq nums[i-1]$, giá trị của $s$ tăng thêm $1$, tức là $s = s + 1$;
- Nếu $nums[i] = nums[i-1]$, giá trị của $s$ được đặt lại thành $1$, tức là $s = 1$.

Sau đó, ta cộng giá trị của $s$ vào đáp án và tiếp tục duyệt vị trí tiếp theo của mảng cho đến khi duyệt hết mảng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countAlternatingSubarrays(self, nums: List[int]) -> int:
        ans = s = 1
        for a, b in pairwise(nums):
            s = s + 1 if a != b else 1
            ans += s
        return ans
```

#### Java

```java
class Solution {
    public long countAlternatingSubarrays(int[] nums) {
        long ans = 1, s = 1;
        for (int i = 1; i < nums.length; ++i) {
            s = nums[i] != nums[i - 1] ? s + 1 : 1;
            ans += s;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countAlternatingSubarrays(vector<int>& nums) {
        long long ans = 1, s = 1;
        for (int i = 1; i < nums.size(); ++i) {
            s = nums[i] != nums[i - 1] ? s + 1 : 1;
            ans += s;
        }
        return ans;
    }
};
```

#### Go

```go
func countAlternatingSubarrays(nums []int) int64 {
	ans, s := int64(1), int64(1)
	for i, x := range nums[1:] {
		if x != nums[i] {
			s++
		} else {
			s = 1
		}
		ans += s
	}
	return ans
}
```

#### TypeScript

```ts
function countAlternatingSubarrays(nums: number[]): number {
    let [ans, s] = [1, 1];
    for (let i = 1; i < nums.length; ++i) {
        s = nums[i] !== nums[i - 1] ? s + 1 : 1;
        ans += s;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
