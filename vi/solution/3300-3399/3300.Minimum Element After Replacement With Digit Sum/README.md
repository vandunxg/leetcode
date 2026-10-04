---
comments: true
difficulty: Easy
rating: 1181
source: Biweekly Contest 140 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3300. Minimum Element After Replacement With Digit Sum](https://leetcode.com/problems/minimum-element-after-replacement-with-digit-sum)

[中文文档](/solution/3300-3399/3300.Minimum%20Element%20After%20Replacement%20With%20Digit%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Thay mỗi phần tử trong <code>nums</code> bằng <strong>tổng</strong> các chữ số của nó.</p>

<p>Trả về phần tử <strong>nhỏ nhất</strong> trong <code>nums</code> sau khi thay thế toàn bộ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,12,13,14]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>nums</code> trở thành <code>[1, 3, 4, 5]</code> sau khi thay thế toàn bộ, phần tử nhỏ nhất là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>nums</code> trở thành <code>[1, 2, 3, 4]</code> sau khi thay thế toàn bộ, phần tử nhỏ nhất là 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [999,19,199]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>nums</code> trở thành <code>[27, 10, 19]</code> sau khi thay thế toàn bộ, phần tử nhỏ nhất là 10.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ta thay mỗi phần tử bằng tổng các chữ số của nó rồi lấy giá trị nhỏ nhất. Với $n \le 100$ và $M \le 10^4$, việc tách các chữ số chỉ tốn $O(\log M)$ cho mỗi giá trị, hoàn toàn đáp ứng được ràng buộc.
>
> Các tổng chữ số độc lập với nhau, nên không cần sắp xếp hay lập bảng. Chỉ cần duyệt một lần và duy trì giá trị nhỏ nhất hiện tại.
>
> Với mỗi $x$, ta cộng dồn các chữ số thập phân của nó và trả về giá trị nhỏ nhất trong các tổng đó.

<!-- thinking:end -->

Ta có thể duyệt qua mảng $\textit{nums}$. Với mỗi số $x$, ta tính tổng các chữ số của nó là $y$. Giá trị nhỏ nhất trong tất cả các $y$ là đáp án.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ và $M$ lần lượt là độ dài của mảng $\textit{nums}$ và giá trị lớn nhất trong mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minElement(self, nums: List[int]) -> int:
        return min(sum(int(b) for b in str(x)) for x in nums)
```

#### Java

```java
class Solution {
    public int minElement(int[] nums) {
        int ans = 100;
        for (int x : nums) {
            int y = 0;
            for (; x > 0; x /= 10) {
                y += x % 10;
            }
            ans = Math.min(ans, y);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minElement(vector<int>& nums) {
        int ans = 100;
        for (int x : nums) {
            int y = 0;
            for (; x > 0; x /= 10) {
                y += x % 10;
            }
            ans = min(ans, y);
        }
        return ans;
    }
};
```

#### Go

```go
func minElement(nums []int) int {
	ans := 100
	for _, x := range nums {
		y := 0
		for ; x > 0; x /= 10 {
			y += x % 10
		}
		ans = min(ans, y)
	}
	return ans
}
```

#### TypeScript

```ts
function minElement(nums: number[]): number {
    let ans: number = 100;
    for (let x of nums) {
        let y = 0;
        for (; x; x = Math.floor(x / 10)) {
            y += x % 10;
        }
        ans = Math.min(ans, y);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
