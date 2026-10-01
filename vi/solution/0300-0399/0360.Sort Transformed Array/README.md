---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [360. Sort Transformed Array 🔒](https://leetcode.com/problems/sort-transformed-array)

[中文文档](/solution/0300-0399/0360.Sort%20Transformed%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> đã <strong>sắp xếp</strong> và ba số nguyên <code>a</code>, <code>b</code>, <code>c</code>. Áp dụng hàm bậc hai dạng <code>f(x) = ax<sup>2</sup> + bx + c</code> lên từng phần tử <code>nums[i]</code>, rồi trả về <em>mảng đã được sắp xếp</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [-4,-2,2,4], a = 1, b = 3, c = 5
<strong>Đầu ra:</strong> [3,9,15,33]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [-4,-2,2,4], a = -1, b = 3, c = 5
<strong>Đầu ra:</strong> [-23,-5,1,7]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 200</code></li>
	<li><code>-100 &lt;= nums[i], a, b, c &lt;= 100</code></li>
	<li><code>nums</code> được sắp xếp theo thứ tự <strong>tăng dần</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài toán trong thời gian <code>O(n)</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học + Two pointers

<!-- thinking:start -->

> **Tư duy**
>
> $nums$ đã được sắp xếp; ta cần trả về các giá trị $ax^2+bx+c$ theo thứ tự. Tính tất cả giá trị rồi sắp xếp tốn $O(n\log n)$, nhưng hàm bậc hai đơn điệu ở mỗi phía của đỉnh parabol.
>
> Nếu $a>0$, giá trị ở hai đầu lớn hơn nên điền đáp án từ cuối về đầu; nếu $a\le 0$, điền từ đầu. Hai con trỏ tiến dần vào giữa, nên thuật toán chạy tuyến tính.

<!-- thinking:end -->

Đồ thị của hàm bậc hai là một parabol. Khi $a \gt 0$, parabol mở lên và đỉnh là điểm có giá trị nhỏ nhất; khi $a \lt 0$, parabol mở xuống và đỉnh là điểm có giá trị lớn nhất.

Vì mảng $\textit{nums}$ đã được sắp xếp, ta đặt hai con trỏ ở hai đầu mảng. Tùy theo dấu của $a$, ta quyết định điền các giá trị lớn hơn (hoặc nhỏ hơn) vào mảng kết quả từ đầu hay từ cuối.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortTransformedArray(
        self, nums: List[int], a: int, b: int, c: int
    ) -> List[int]:
        def f(x: int) -> int:
            return a * x * x + b * x + c

        n = len(nums)
        i, j = 0, n - 1
        ans = [0] * n
        for k in range(n):
            y1, y2 = f(nums[i]), f(nums[j])
            if a > 0:
                if y1 > y2:
                    ans[n - k - 1] = y1
                    i += 1
                else:
                    ans[n - k - 1] = y2
                    j -= 1
            else:
                if y1 > y2:
                    ans[k] = y2
                    j -= 1
                else:
                    ans[k] = y1
                    i += 1
        return ans
```

#### Java

```java
class Solution {
    public int[] sortTransformedArray(int[] nums, int a, int b, int c) {
        int n = nums.length;
        int[] ans = new int[n];
        int i = 0, j = n - 1;

        IntUnaryOperator f = x -> a * x * x + b * x + c;

        for (int k = 0; k < n; k++) {
            int y1 = f.applyAsInt(nums[i]);
            int y2 = f.applyAsInt(nums[j]);
            if (a > 0) {
                if (y1 > y2) {
                    ans[n - k - 1] = y1;
                    i++;
                } else {
                    ans[n - k - 1] = y2;
                    j--;
                }
            } else {
                if (y1 > y2) {
                    ans[k] = y2;
                    j--;
                } else {
                    ans[k] = y1;
                    i++;
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
    vector<int> sortTransformedArray(vector<int>& nums, int a, int b, int c) {
        int n = nums.size();
        vector<int> ans(n);
        int i = 0, j = n - 1;

        auto f = [&](int x) {
            return a * x * x + b * x + c;
        };

        for (int k = 0; k < n; k++) {
            int y1 = f(nums[i]);
            int y2 = f(nums[j]);
            if (a > 0) {
                if (y1 > y2) {
                    ans[n - k - 1] = y1;
                    i++;
                } else {
                    ans[n - k - 1] = y2;
                    j--;
                }
            } else {
                if (y1 > y2) {
                    ans[k] = y2;
                    j--;
                } else {
                    ans[k] = y1;
                    i++;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func sortTransformedArray(nums []int, a int, b int, c int) []int {
	f := func(x int) int {
		return a*x*x + b*x + c
	}

	n := len(nums)
	ans := make([]int, n)
	i, j := 0, n-1

	for k := 0; k < n; k++ {
		y1, y2 := f(nums[i]), f(nums[j])
		if a > 0 {
			if y1 > y2 {
				ans[n-k-1] = y1
				i++
			} else {
				ans[n-k-1] = y2
				j--
			}
		} else {
			if y1 > y2 {
				ans[k] = y2
				j--
			} else {
				ans[k] = y1
				i++
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function sortTransformedArray(nums: number[], a: number, b: number, c: number): number[] {
    const f = (x: number): number => a * x * x + b * x + c;
    const n = nums.length;
    let [i, j] = [0, n - 1];
    const ans: number[] = Array(n);
    for (let k = 0; k < n; ++k) {
        const y1 = f(nums[i]);
        const y2 = f(nums[j]);
        if (a > 0) {
            if (y1 > y2) {
                ans[n - k - 1] = y1;
                ++i;
            } else {
                ans[n - k - 1] = y2;
                --j;
            }
        } else {
            if (y1 > y2) {
                ans[k] = y2;
                --j;
            } else {
                ans[k] = y1;
                ++i;
            }
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} a
 * @param {number} b
 * @param {number} c
 * @return {number[]}
 */
var sortTransformedArray = function (nums, a, b, c) {
    const f = x => a * x * x + b * x + c;
    const n = nums.length;
    let [i, j] = [0, n - 1];
    const ans = Array(n);
    for (let k = 0; k < n; ++k) {
        const y1 = f(nums[i]);
        const y2 = f(nums[j]);
        if (a > 0) {
            if (y1 > y2) {
                ans[n - k - 1] = y1;
                ++i;
            } else {
                ans[n - k - 1] = y2;
                --j;
            }
        } else {
            if (y1 > y2) {
                ans[k] = y2;
                --j;
            } else {
                ans[k] = y1;
                ++i;
            }
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
