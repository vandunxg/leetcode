---
comments: true
difficulty: Easy
tags:
    - Array
    - Math
    - Sorting
---

<!-- problem:start -->

# [628. Maximum Product of Three Numbers](https://leetcode.com/problems/maximum-product-of-three-numbers)

[中文文档](/solution/0600-0699/0628.Maximum%20Product%20of%20Three%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho mảng số nguyên <code>nums</code>.</p>

<p>Hãy tìm ba số có tích <strong>lớn nhất</strong> và trả về tích <strong>lớn nhất</strong> đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ba số duy nhất là 1, 2 và 3, nên tích lớn nhất là <code>1 * 2 * 3 = 6</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">24</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tích lớn nhất được tạo bởi ba số lớn nhất: <code>2 * 3 * 4 = 24</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-1,-2,-3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ba số duy nhất là -1, -2 và -3, nên tích lớn nhất là <code>(-1) * (-2) * (-3) = -6</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;=&nbsp;10<sup>4</sup></code></li>
	<li><code>-1000 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + xét trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Tích lớn nhất của ba số có thể là tích của ba giá trị lớn nhất, hoặc tích của hai giá trị nhỏ nhất (có thể âm) với giá trị lớn nhất. Sắp xếp không tốn nhiều chi phí khi $n\le 10^4$.
>
> Sau khi sắp xếp, so sánh $nums[-1]\times nums[-2]\times nums[-3]$ với $nums[-1]\times nums[0]\times nums[1]$.

<!-- thinking:end -->

Đầu tiên, ta sắp xếp mảng $\textit{nums}$, sau đó xét hai trường hợp:

- Nếu $\textit{nums}$ chỉ chứa số không âm hoặc chỉ chứa số không dương, đáp án là tích của ba số cuối, tức $\textit{nums}[n-1] \times \textit{nums}[n-2] \times \textit{nums}[n-3]$;
- Nếu $\textit{nums}$ có cả số dương và số âm, đáp án có thể là tích của hai số âm nhỏ nhất với số dương lớn nhất, tức $\textit{nums}[n-1] \times \textit{nums}[0] \times \textit{nums}[1]$, hoặc tích của ba số cuối, tức $\textit{nums}[n-1] \times \textit{nums}[n-2] \times \textit{nums}[n-3]$.

Cuối cùng, trả về giá trị lớn hơn trong hai trường hợp.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumProduct(self, nums: List[int]) -> int:
        nums.sort()
        a = nums[-1] * nums[-2] * nums[-3]
        b = nums[-1] * nums[0] * nums[1]
        return max(a, b)
```

#### Java

```java
class Solution {
    public int maximumProduct(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        int a = nums[n - 1] * nums[n - 2] * nums[n - 3];
        int b = nums[n - 1] * nums[0] * nums[1];
        return Math.max(a, b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        int a = nums[n - 1] * nums[n - 2] * nums[n - 3];
        int b = nums[n - 1] * nums[0] * nums[1];
        return max(a, b);
    }
};
```

#### Go

```go
func maximumProduct(nums []int) int {
	sort.Ints(nums)
	n := len(nums)
	a := nums[n-1] * nums[n-2] * nums[n-3]
	b := nums[n-1] * nums[0] * nums[1]
	if a > b {
		return a
	}
	return b
}
```

#### TypeScript

```ts
function maximumProduct(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    const a = nums[n - 1] * nums[n - 2] * nums[n - 3];
    const b = nums[n - 1] * nums[0] * nums[1];
    return Math.max(a, b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Không cần sắp xếp toàn bộ mảng: chỉ cần hai giá trị nhỏ nhất và ba giá trị lớn nhất. Một lượt duyệt tuyến tính (hoặc dùng `nlargest`) là đủ để lưu năm giá trị cực trị này.

<!-- thinking:end -->

Ta có thể bỏ qua bước sắp xếp bằng cách duy trì năm biến: $\textit{mi1}$ và $\textit{mi2}$ lần lượt lưu hai số nhỏ nhất trong mảng, còn $\textit{mx1}$, $\textit{mx2}$ và $\textit{mx3}$ lưu ba số lớn nhất.

Cuối cùng, trả về $\max(\textit{mi1} \times \textit{mi2} \times \textit{mx1}, \textit{mx1} \times \textit{mx2} \times \textit{mx3})$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumProduct(self, nums: List[int]) -> int:
        top3 = nlargest(3, nums)
        bottom2 = nlargest(2, nums, key=lambda x: -x)
        return max(top3[0] * top3[1] * top3[2], top3[0] * bottom2[0] * bottom2[1])
```

#### Java

```java
class Solution {
    public int maximumProduct(int[] nums) {
        final int inf = 1 << 30;
        int mi1 = inf, mi2 = inf;
        int mx1 = -inf, mx2 = -inf, mx3 = -inf;
        for (int x : nums) {
            if (x < mi1) {
                mi2 = mi1;
                mi1 = x;
            } else if (x < mi2) {
                mi2 = x;
            }
            if (x > mx1) {
                mx3 = mx2;
                mx2 = mx1;
                mx1 = x;
            } else if (x > mx2) {
                mx3 = mx2;
                mx2 = x;
            } else if (x > mx3) {
                mx3 = x;
            }
        }
        return Math.max(mi1 * mi2 * mx1, mx1 * mx2 * mx3);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        const int inf = 1 << 30;
        int mi1 = inf, mi2 = inf;
        int mx1 = -inf, mx2 = -inf, mx3 = -inf;
        for (int x : nums) {
            if (x < mi1) {
                mi2 = mi1;
                mi1 = x;
            } else if (x < mi2) {
                mi2 = x;
            }
            if (x > mx1) {
                mx3 = mx2;
                mx2 = mx1;
                mx1 = x;
            } else if (x > mx2) {
                mx3 = mx2;
                mx2 = x;
            } else if (x > mx3) {
                mx3 = x;
            }
        }
        return max(mi1 * mi2 * mx1, mx1 * mx2 * mx3);
    }
};
```

#### Go

```go
func maximumProduct(nums []int) int {
	const inf = 1 << 30
	mi1, mi2 := inf, inf
	mx1, mx2, mx3 := -inf, -inf, -inf
	for _, x := range nums {
		if x < mi1 {
			mi1, mi2 = x, mi1
		} else if x < mi2 {
			mi2 = x
		}
		if x > mx1 {
			mx1, mx2, mx3 = x, mx1, mx2
		} else if x > mx2 {
			mx2, mx3 = x, mx2
		} else if x > mx3 {
			mx3 = x
		}
	}
	return max(mi1*mi2*mx1, mx1*mx2*mx3)
}
```

#### TypeScript

```ts
function maximumProduct(nums: number[]): number {
    const inf = 1 << 30;
    let mi1 = inf,
        mi2 = inf;
    let mx1 = -inf,
        mx2 = -inf,
        mx3 = -inf;
    for (const x of nums) {
        if (x < mi1) {
            mi2 = mi1;
            mi1 = x;
        } else if (x < mi2) {
            mi2 = x;
        }
        if (x > mx1) {
            mx3 = mx2;
            mx2 = mx1;
            mx1 = x;
        } else if (x > mx2) {
            mx3 = mx2;
            mx2 = x;
        } else if (x > mx3) {
            mx3 = x;
        }
    }
    return Math.max(mi1 * mi2 * mx1, mx1 * mx2 * mx3);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
