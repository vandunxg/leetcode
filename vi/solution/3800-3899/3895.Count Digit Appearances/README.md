---
comments: true
difficulty: Medium
rating: 1269
source: Biweekly Contest 180 Q2
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3895. Count Digit Appearances](https://leetcode.com/problems/count-digit-appearances)

[中文文档](/solution/3800-3899/3895.Count%20Digit%20Appearances/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một chữ số nguyên <code>digit</code>.</p>

<p>Hãy trả về tổng số lần <code>digit</code> xuất hiện trong biểu diễn thập phân của tất cả các phần tử trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [12,54,32,22], digit = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chữ số 2 xuất hiện một lần trong 12 và 32, và hai lần trong 22. Do đó, tổng số lần chữ số 2 xuất hiện là 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,34,7], digit = 9</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chữ số 9 không xuất hiện trong biểu diễn thập phân của bất kỳ phần tử nào trong <code>nums</code>, nên tổng số lần chữ số 9 xuất hiện là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup>​​​​​​​</code></li>
	<li><code>0 &lt;= digit &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số lần $\textit{digit}$ xuất hiện trong biểu diễn thập phân của $\textit{nums}$. Độ dài và giá trị đều vừa phải; ta chia cho mười và lấy phần dư.
>
> Thứ tự các chữ số không quan trọng, nên không cần chuyển đổi sang chuỗi.
>
> Vòng lặp dừng ở $0$; vì $nums[i] \ge 1$ nên không có vấn đề với các số 0 ở đầu.
>
> Cộng dồn các lần khớp.

<!-- thinking:end -->

Ta duyệt qua từng phần tử trong mảng và đếm số lần $\textit{digit}$ xuất hiện. Với mỗi phần tử, ta có thể lấy từng chữ số bằng cách liên tục lấy modulo và chia cho 10, rồi so sánh từng chữ số với $\textit{digit}$. Nếu chúng bằng nhau, ta tăng đáp án lên 1.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log_{10} M)$, còn độ phức tạp không gian là $O(1)$. Ở đây, $n$ và $M$ lần lượt là độ dài của mảng và giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countDigitOccurrences(self, nums: list[int], digit: int) -> int:
        ans = 0
        for x in nums:
            while x:
                v = x % 10
                if v == digit:
                    ans += 1
                x //= 10
        return ans
```

#### Java

```java
class Solution {
    public int countDigitOccurrences(int[] nums, int digit) {
        int ans = 0;
        for (int x : nums) {
            for (; x > 0; x /= 10) {
                if (x % 10 == digit) {
                    ++ans;
                }
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
    int countDigitOccurrences(vector<int>& nums, int digit) {
        int ans = 0;
        for (int x : nums) {
            for (; x > 0; x /= 10) {
                if (x % 10 == digit) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countDigitOccurrences(nums []int, digit int) (ans int) {
	for _, x := range nums {
		for ; x > 0; x /= 10 {
			if x%10 == digit {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countDigitOccurrences(nums: number[], digit: number): number {
    let ans = 0;
    for (let x of nums) {
        for (; x; x = Math.floor(x / 10)) {
            if (x % 10 === digit) {
                ++ans;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
