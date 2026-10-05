---
comments: true
difficulty: Easy
rating: 1273
source: Weekly Contest 499 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3912. Valid Elements in an Array](https://leetcode.com/problems/valid-elements-in-an-array)

[中文文档](/solution/3900-3999/3912.Valid%20Elements%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Một phần tử <code>nums[i]</code> được xem là <strong>hợp lệ</strong> nếu thỏa mãn <strong>ít nhất</strong> một trong các điều kiện sau:</p>

<ul>
	<li>Nó <strong>lớn hơn nghiêm ngặt</strong> mọi phần tử bên trái.</li>
	<li>Nó <strong>lớn hơn nghiêm ngặt</strong> mọi phần tử bên phải.</li>
</ul>

<p>Phần tử đầu tiên và phần tử cuối cùng luôn hợp lệ.</p>

<p>Trả về một mảng gồm tất cả các phần tử hợp lệ theo đúng thứ tự xuất hiện trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,4,2,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,4,3,2]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>nums[0]</code> và <code>nums[5]</code> luôn hợp lệ.</li>
	<li><code>nums[1]</code> và <code>nums[2]</code> lớn hơn nghiêm ngặt mọi phần tử bên trái chúng.</li>
	<li><code>nums[4]</code> lớn hơn nghiêm ngặt mọi phần tử bên phải nó.</li>
	<li>Vì vậy, đáp án là <code>[1, 2, 4, 3, 2]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,5,5,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[5,5]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Phần tử đầu tiên và phần tử cuối cùng luôn hợp lệ.</li>
	<li>Không có phần tử nào khác lớn hơn nghiêm ngặt tất cả phần tử bên trái hoặc bên phải nó.</li>
	<li>Vì vậy, đáp án là <code>[5, 5]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì chỉ có một phần tử nên phần tử đó luôn hợp lệ. Do đó, đáp án là <code>[1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý mảng

<!-- thinking:start -->

> **Tư duy**
>
> Việc quét lại giá trị lớn nhất bên trái và bên phải tại mỗi chỉ số có độ phức tạp $O(n^2)$. Điều này vẫn phù hợp với $n\le 100$, nhưng tính hợp lệ chỉ phụ thuộc vào hai giá trị cực trị đó.
>
> Ta tiền xử lý các giá trị lớn nhất của hậu tố vào mảng $\textit{right}$ từ phải sang trái, đồng thời duy trì giá trị lớn nhất của tiền tố $\textit{left}$ khi duyệt từ trái sang phải. Một phần tử hợp lệ khi và chỉ khi nó lớn hơn nghiêm ngặt $\textit{left}$, là phần tử cuối cùng, hoặc lớn hơn nghiêm ngặt giá trị lớn nhất của hậu tố bên phải.
>
> Một lượt tiền xử lý cùng một lượt quét sẽ thu thập được mọi giá trị hợp lệ.

<!-- thinking:end -->

Ta có thể tiền xử lý mảng để tính giá trị lớn nhất ở bên phải mỗi phần tử và lưu các giá trị đó vào mảng $\textit{right}$.

Sau đó, ta duyệt mảng từ trái sang phải, dùng biến $\textit{left}$ để lưu giá trị lớn nhất ở bên trái phần tử hiện tại. Với mỗi phần tử, nếu thỏa mãn một trong các điều kiện sau thì ta thêm phần tử đó vào đáp án:

- Nó lớn hơn nghiêm ngặt $\textit{left}$.
- Nó là phần tử cuối cùng của mảng.
- Nó lớn hơn nghiêm ngặt $\textit{right}[i + 1]$.

Trong quá trình duyệt, ta liên tục cập nhật giá trị của $\textit{left}$.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findValidElements(self, nums: list[int]) -> list[int]:
        n = len(nums)
        right = [nums[-1]] * n
        for i in range(n - 2, -1, -1):
            right[i] = max(right[i + 1], nums[i])
        left = 0
        ans = []
        for i, x in enumerate(nums):
            if x > left or i == n - 1 or x > right[i + 1]:
                ans.append(x)
            left = max(left, x)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> findValidElements(int[] nums) {
        int n = nums.length;
        int[] right = new int[n];
        right[n - 1] = nums[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            right[i] = Math.max(right[i + 1], nums[i]);
        }
        int left = 0;
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            int x = nums[i];
            if (x > left || i == n - 1 || x > right[i + 1]) {
                ans.add(x);
            }
            left = Math.max(left, x);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findValidElements(vector<int>& nums) {
        int n = nums.size();
        vector<int> right(n);
        right[n - 1] = nums[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            right[i] = max(right[i + 1], nums[i]);
        }
        int left = 0;
        vector<int> ans;
        for (int i = 0; i < n; i++) {
            int x = nums[i];
            if (x > left || i == n - 1 || x > right[i + 1]) {
                ans.push_back(x);
            }
            left = max(left, x);
        }
        return ans;
    }
};
```

#### Go

```go
func findValidElements(nums []int) []int {
	n := len(nums)
	right := make([]int, n)
	right[n-1] = nums[n-1]
	for i := n - 2; i >= 0; i-- {
		right[i] = max(right[i+1], nums[i])
	}
	left := 0
	ans := []int{}
	for i, x := range nums {
		if x > left || i == n-1 || x > right[i+1] {
			ans = append(ans, x)
		}
		left = max(left, x)
	}
	return ans
}
```

#### TypeScript

```ts
function findValidElements(nums: number[]): number[] {
    const n = nums.length;
    const right = new Array(n);
    right[n - 1] = nums[n - 1];
    for (let i = n - 2; i >= 0; i--) {
        right[i] = Math.max(right[i + 1], nums[i]);
    }
    let left = 0;
    const ans: number[] = [];
    for (let i = 0; i < n; i++) {
        const x = nums[i];
        if (x > left || i === n - 1 || x > right[i + 1]) {
            ans.push(x);
        }
        left = Math.max(left, x);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
