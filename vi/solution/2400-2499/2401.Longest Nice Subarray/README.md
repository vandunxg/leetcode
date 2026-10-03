---
comments: true
difficulty: Medium
rating: 1749
source: Weekly Contest 309 Q3
tags:
    - Bit Manipulation
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [2401. Longest Nice Subarray](https://leetcode.com/problems/longest-nice-subarray)

[中文文档](/solution/2400-2499/2401.Longest%20Nice%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm các số nguyên <strong>dương</strong>.</p>

<p>Ta gọi một subarray của <code>nums</code> là <strong>nice</strong> nếu phép <strong>AND</strong> bit của mọi cặp phần tử nằm ở <strong>các vị trí khác nhau</strong> trong subarray bằng <code>0</code>.</p>

<p>Hãy trả về <em>độ dài của subarray <strong>nice</strong> dài nhất</em>.</p>

<p><strong>Subarray</strong> là một phần <strong>liên tiếp</strong> của mảng.</p>

<p><strong>Lưu ý</strong> rằng subarray có độ dài <code>1</code> luôn được xem là nice.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,8,48,10]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Subarray nice dài nhất là [3,8,48]. Subarray này thỏa mãn các điều kiện:
- 3 AND 8 = 0.
- 3 AND 48 = 0.
- 8 AND 48 = 0.
Có thể chứng minh rằng không thể thu được subarray nice dài hơn, nên ta trả về 3.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1,5,11,13]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Độ dài của subarray nice dài nhất là 1. Có thể chọn bất kỳ subarray nào có độ dài 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra mọi subarray để xác định AND bitwise của từng cặp có bằng 0 hay không sẽ tạo ra $O(n^2)$ cửa sổ; với $n\le 10^5$ thì không thể chạy qua. Quét từ trái sang cho từng điểm cuối bên phải vẫn có độ phức tạp bậc hai trong trường hợp xấu nhất.
>
> Việc AND theo cặp bằng 0 có nghĩa là các bit $1$ của các số không bao giờ chồng lấp, nên OR bitwise của cửa sổ là một mask biểu diễn đầy đủ các bit đang được sử dụng. Khi giá trị mới xung đột, ta phải loại bỏ các phần tử từ bên trái cho đến khi xung đột biến mất, vì vậy điểm đầu bên trái chỉ di chuyển sang phải.
>
> Duy trì OR của cửa sổ trong $\textit{mask}$. Di chuyển con trỏ phải; khi xảy ra xung đột, dùng XOR để loại bỏ giá trị ở bên trái. Mỗi chỉ số được thêm vào và loại bỏ nhiều nhất một lần, từ đó tìm được cửa sổ nice dài nhất trong thời gian tuyến tính.

<!-- thinking:end -->

Theo mô tả bài toán, các vị trí của bit nhị phân $1$ trong mỗi phần tử của subarray phải là duy nhất để đảm bảo kết quả AND bitwise của bất kỳ hai phần tử nào cũng bằng $0$.

Vì vậy, ta có thể sử dụng hai con trỏ $l$ và $r$ để duy trì một cửa sổ trượt sao cho các phần tử trong cửa sổ thỏa mãn điều kiện của bài toán.

Ta sử dụng biến $\textit{mask}$ để biểu diễn kết quả OR bitwise của các phần tử trong cửa sổ. Tiếp theo, ta duyệt qua từng phần tử của mảng. Với phần tử hiện tại $x$, nếu kết quả AND bitwise của $\textit{mask}$ và $x$ khác $0$, điều đó có nghĩa là phần tử hiện tại $x$ có các bit nhị phân trùng với các phần tử trong cửa sổ. Khi đó, ta cần di chuyển con trỏ trái $l$ cho đến khi kết quả AND bitwise của $\textit{mask}$ và $x$ bằng $0$. Sau đó, ta gán kết quả OR bitwise của $\textit{mask}$ và $x$ cho $\textit{mask}$ rồi cập nhật đáp án $\textit{ans} = \max(\textit{ans}, r - l + 1)$.

Sau khi duyệt xong, trả về đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestNiceSubarray(self, nums: List[int]) -> int:
        ans = mask = l = 0
        for r, x in enumerate(nums):
            while mask & x:
                mask ^= nums[l]
                l += 1
            mask |= x
            ans = max(ans, r - l + 1)
        return ans
```

#### Java

```java
class Solution {
    public int longestNiceSubarray(int[] nums) {
        int ans = 0, mask = 0;
        for (int l = 0, r = 0; r < nums.length; ++r) {
            while ((mask & nums[r]) != 0) {
                mask ^= nums[l++];
            }
            mask |= nums[r];
            ans = Math.max(ans, r - l + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestNiceSubarray(vector<int>& nums) {
        int ans = 0, mask = 0;
        for (int l = 0, r = 0; r < nums.size(); ++r) {
            while (mask & nums[r]) {
                mask ^= nums[l++];
            }
            mask |= nums[r];
            ans = max(ans, r - l + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func longestNiceSubarray(nums []int) (ans int) {
	mask, l := 0, 0
	for r, x := range nums {
		for mask&x != 0 {
			mask ^= nums[l]
			l++
		}
		mask |= x
		ans = max(ans, r-l+1)
	}
	return
}
```

#### TypeScript

```ts
function longestNiceSubarray(nums: number[]): number {
    let [ans, mask] = [0, 0];
    for (let l = 0, r = 0; r < nums.length; ++r) {
        while (mask & nums[r]) {
            mask ^= nums[l++];
        }
        mask |= nums[r];
        ans = Math.max(ans, r - l + 1);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_nice_subarray(nums: Vec<i32>) -> i32 {
        let mut ans = 0;
        let mut mask = 0;
        let mut l = 0;
        for (r, &x) in nums.iter().enumerate() {
            while mask & x != 0 {
                mask ^= nums[l];
                l += 1;
            }
            mask |= x;
            ans = ans.max((r - l + 1) as i32);
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int LongestNiceSubarray(int[] nums) {
        int ans = 0, mask = 0;
        for (int l = 0, r = 0; r < nums.Length; ++r) {
            while ((mask & nums[r]) != 0) {
                mask ^= nums[l++];
            }
            mask |= nums[r];
            ans = Math.Max(ans, r - l + 1);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
