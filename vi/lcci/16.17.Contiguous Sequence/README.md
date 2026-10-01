---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [16.17. Contiguous Sequence](https://leetcode.cn/problems/contiguous-sequence-lcci)

[中文文档](/lcci/16.17.Contiguous%20Sequence/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên (gồm cả số dương và số âm). Hãy tìm dãy liên tiếp có tổng lớn nhất. Trả về tổng đó.</p>

<p><strong>Ví dụ: </strong></p>

<pre>



<strong>Đầu vào: </strong> [-2,1,-3,4,-1,2,1,-5,4]



<strong>Đầu ra: </strong> 6



<strong>Giải thích: </strong> [4,-1,2,1] có tổng lớn nhất là 6.



</pre>

<p><strong>Câu hỏi mở rộng: </strong></p>

<p>Nếu đã tìm ra lời giải O(n), hãy thử viết một lời giải khác bằng phương pháp chia để trị, vốn tinh tế hơn.</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Bài toán tìm tổng lớn nhất của mảng con. Nếu thử mọi cặp đầu mút, độ phức tạp sẽ là bậc ba hoặc bậc hai.
>
> Mảng con tốt nhất kết thúc tại $i$ hoặc nối dài mảng con tốt nhất kết thúc tại $i-1$, hoặc bắt đầu lại; đó chính là công thức truy hồi của Kadane.
>
> Cập nhật $f=\max(f,0)+x$ và theo dõi giá trị lớn nhất trên toàn bộ mảng. Không gian bổ sung là hằng số.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là tổng lớn nhất của một mảng con liên tiếp kết thúc tại $nums[i]$. Công thức chuyển trạng thái là:

$$
f[i] = \max(f[i-1], 0) + nums[i]
$$

trong đó $f[0] = nums[0]$.

Đáp án là $\max\limits_{i=0}^{n-1}f[i]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

Ta nhận thấy $f[i]$ chỉ phụ thuộc vào $f[i-1]$, vì vậy có thể dùng một biến $f$ để biểu diễn $f[i-1]$, qua đó giảm độ phức tạp không gian xuống còn $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        ans = f = -inf
        for x in nums:
            f = max(f, 0) + x
            ans = max(ans, f)
        return ans
```

#### Java

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int ans = Integer.MIN_VALUE, f = Integer.MIN_VALUE;
        for (int x : nums) {
            f = Math.max(f, 0) + x;
            ans = Math.max(ans, f);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int ans = INT_MIN, f = INT_MIN;
        for (int x : nums) {
            f = max(f, 0) + x;
            ans = max(ans, f);
        }
        return ans;
    }
};
```

#### Go

```go
func maxSubArray(nums []int) int {
	ans, f := math.MinInt32, math.MinInt32
	for _, x := range nums {
		f = max(f, 0) + x
		ans = max(ans, f)
	}
	return ans
}
```

#### TypeScript

```ts
function maxSubArray(nums: number[]): number {
    let [ans, f] = [-Infinity, -Infinity];
    for (const x of nums) {
        f = Math.max(f, 0) + x;
        ans = Math.max(ans, f);
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
var maxSubArray = function (nums) {
    let [ans, f] = [-Infinity, -Infinity];
    for (const x of nums) {
        f = Math.max(f, 0) + x;
        ans = Math.max(ans, f);
    }
    return ans;
};
```

#### Swift

```swift
class Solution {
    func maxSubArray(_ nums: [Int]) -> Int {
        var ans = Int.min
        var f = Int.min

        for x in nums {
            f = max(f, 0) + x
            ans = max(ans, f)
        }

        return ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
