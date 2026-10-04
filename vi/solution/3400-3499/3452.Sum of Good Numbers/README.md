---
comments: true
difficulty: Easy
rating: 1199
source: Biweekly Contest 150 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3452. Sum of Good Numbers](https://leetcode.com/problems/sum-of-good-numbers)

[中文文档](/solution/3400-3499/3452.Sum%20of%20Good%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>, phần tử <code>nums[i]</code> được xem là <strong>tốt</strong> nếu nó <strong>lớn hơn nghiêm ngặt</strong> các phần tử tại chỉ số <code>i - k</code> và <code>i + k</code> (nếu các chỉ số đó tồn tại). Nếu cả hai chỉ số này đều <em>không tồn tại</em>, <code>nums[i]</code> vẫn được xem là <strong>tốt</strong>.</p>

<p>Trả về <strong>tổng</strong> của tất cả các phần tử <strong>tốt</strong> trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,2,1,5,4], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số tốt là <code>nums[1] = 3</code>, <code>nums[4] = 5</code> và <code>nums[5] = 4</code> vì chúng lớn hơn nghiêm ngặt các số tại chỉ số <code>i - k</code> và <code>i + k</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số tốt duy nhất là <code>nums[0] = 2</code> vì nó lớn hơn nghiêm ngặt <code>nums[1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>1 &lt;= k &lt;= floor(nums.length / 2)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Một số tốt lớn hơn nghiêm ngặt các phần tử cách nó $k$ vị trí, nếu các phần tử đó tồn tại. Vì $n\le 100$, ta kiểm tra từng chỉ số.
>
> Nếu một phía không có phần tử, phía đó không tạo ra ràng buộc nào và không được xem như có giá trị $0$.
>
> Bỏ qua $i$ khi phần tử bên trái tồn tại và $x\le \textit{nums}[i-k]$, hoặc phần tử bên phải tồn tại và $x\le \textit{nums}[i+k]$; nếu không, cộng $x$ vào đáp án.

<!-- thinking:end -->

Ta có thể duyệt qua mảng $\textit{nums}$ và kiểm tra xem mỗi phần tử $\textit{nums}[i]$ có thỏa mãn các điều kiện hay không:

- Nếu $i \ge k$ và $\textit{nums}[i] \le \textit{nums}[i - k]$, thì $\textit{nums}[i]$ không phải là số tốt.
- Nếu $i + k < \textit{len}(\textit{nums})$ và $\textit{nums}[i] \le \textit{nums}[i + k]$, thì $\textit{nums}[i]$ không phải là số tốt.
- Nếu không, $\textit{nums}[i]$ là số tốt và ta cộng nó vào đáp án.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfGoodNumbers(self, nums: List[int], k: int) -> int:
        ans = 0
        for i, x in enumerate(nums):
            if i >= k and x <= nums[i - k]:
                continue
            if i + k < len(nums) and x <= nums[i + k]:
                continue
            ans += x
        return ans
```

#### Java

```java
class Solution {
    public int sumOfGoodNumbers(int[] nums, int k) {
        int ans = 0;
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            if (i >= k && nums[i] <= nums[i - k]) {
                continue;
            }
            if (i + k < n && nums[i] <= nums[i + k]) {
                continue;
            }
            ans += nums[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfGoodNumbers(vector<int>& nums, int k) {
        int ans = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            if (i >= k && nums[i] <= nums[i - k]) {
                continue;
            }
            if (i + k < n && nums[i] <= nums[i + k]) {
                continue;
            }
            ans += nums[i];
        }
        return ans;
    }
};
```

#### Go

```go
func sumOfGoodNumbers(nums []int, k int) (ans int) {
	for i, x := range nums {
		if i >= k && x <= nums[i-k] {
			continue
		}
		if i+k < len(nums) && x <= nums[i+k] {
			continue
		}
		ans += x
	}
	return
}
```

#### TypeScript

```ts
function sumOfGoodNumbers(nums: number[], k: number): number {
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        if (i >= k && nums[i] <= nums[i - k]) {
            continue;
        }
        if (i + k < n && nums[i] <= nums[i + k]) {
            continue;
        }
        ans += nums[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
