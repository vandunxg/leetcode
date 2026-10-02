---
comments: true
difficulty: Medium
rating: 1558
source: Weekly Contest 148 Q1
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [1144. Decrease Elements To Make Array Zigzag](https://leetcode.com/problems/decrease-elements-to-make-array-zigzag)

[中文文档](/solution/1100-1199/1144.Decrease%20Elements%20To%20Make%20Array%20Zigzag/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>. Mỗi <em>thao tác</em> gồm chọn một phần tử bất kỳ và <strong>giảm giá trị của nó đi 1</strong>.</p>

<p>Mảng <code>A</code> được gọi là <em>mảng zigzag</em> nếu thỏa mãn một trong hai điều kiện:</p>

<ul>
	<li>Mọi phần tử ở chỉ số chẵn đều lớn hơn các phần tử liền kề, tức là <code>A[0] &gt; A[1] &lt; A[2] &gt; A[3] &lt; A[4] &gt; ...</code></li>
	<li>Hoặc mọi phần tử ở chỉ số lẻ đều lớn hơn các phần tử liền kề, tức là <code>A[0] &lt; A[1] &gt; A[2] &lt; A[3] &gt; A[4] &lt; ...</code></li>
</ul>

<p>Hãy trả về số thao tác ít nhất để biến mảng <code>nums</code> đã cho thành mảng zigzag.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Ta có thể giảm 2 xuống 0 hoặc giảm 3 xuống 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [9,6,1,6,2]
<strong>Output:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ có thể giảm giá trị. Có hai dạng zigzag: “chỉ số chẵn nhỏ hơn” hoặc “chỉ số lẻ nhỏ hơn”; tính số thao tác cho cả hai rồi chọn phương án ít hơn. Với mỗi vị trí cần trở thành phần tử nhỏ hơn, mức giảm cần thiết được xác định bởi hai phần tử kề nó; các vị trí này không ảnh hưởng lẫn nhau.

<!-- thinking:end -->

Ta lần lượt xét các vị trí chẵn và lẻ là những vị trí cần có giá trị nhỏ hơn các phần tử kề bên, rồi tính số thao tác cần thiết. Đáp án là giá trị nhỏ hơn trong hai trường hợp.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def movesToMakeZigzag(self, nums: List[int]) -> int:
        ans = [0, 0]
        n = len(nums)
        for i in range(2):
            for j in range(i, n, 2):
                d = 0
                if j:
                    d = max(d, nums[j] - nums[j - 1] + 1)
                if j < n - 1:
                    d = max(d, nums[j] - nums[j + 1] + 1)
                ans[i] += d
        return min(ans)
```

#### Java

```java
class Solution {
    public int movesToMakeZigzag(int[] nums) {
        int[] ans = new int[2];
        int n = nums.length;
        for (int i = 0; i < 2; ++i) {
            for (int j = i; j < n; j += 2) {
                int d = 0;
                if (j > 0) {
                    d = Math.max(d, nums[j] - nums[j - 1] + 1);
                }
                if (j < n - 1) {
                    d = Math.max(d, nums[j] - nums[j + 1] + 1);
                }
                ans[i] += d;
            }
        }
        return Math.min(ans[0], ans[1]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int movesToMakeZigzag(vector<int>& nums) {
        vector<int> ans(2);
        int n = nums.size();
        for (int i = 0; i < 2; ++i) {
            for (int j = i; j < n; j += 2) {
                int d = 0;
                if (j) d = max(d, nums[j] - nums[j - 1] + 1);
                if (j < n - 1) d = max(d, nums[j] - nums[j + 1] + 1);
                ans[i] += d;
            }
        }
        return min(ans[0], ans[1]);
    }
};
```

#### Go

```go
func movesToMakeZigzag(nums []int) int {
	ans := [2]int{}
	n := len(nums)
	for i := 0; i < 2; i++ {
		for j := i; j < n; j += 2 {
			d := 0
			if j > 0 {
				d = max(d, nums[j]-nums[j-1]+1)
			}
			if j < n-1 {
				d = max(d, nums[j]-nums[j+1]+1)
			}
			ans[i] += d
		}
	}
	return min(ans[0], ans[1])
}
```

#### TypeScript

```ts
function movesToMakeZigzag(nums: number[]): number {
    const ans: number[] = Array(2).fill(0);
    const n = nums.length;
    for (let i = 0; i < 2; ++i) {
        for (let j = i; j < n; j += 2) {
            let d = 0;
            if (j > 0) {
                d = Math.max(d, nums[j] - nums[j - 1] + 1);
            }
            if (j < n - 1) {
                d = Math.max(d, nums[j] - nums[j + 1] + 1);
            }
            ans[i] += d;
        }
    }
    return Math.min(...ans);
}
```

#### C#

```cs
public class Solution {
    public int MovesToMakeZigzag(int[] nums) {
        int[] ans = new int[2];
        int n = nums.Length;
        for (int i = 0; i < 2; ++i) {
            for (int j = i; j < n; j += 2) {
                int d = 0;
                if (j > 0) {
                    d = Math.Max(d, nums[j] - nums[j - 1] + 1);
                }
                if (j < n - 1) {
                    d = Math.Max(d, nums[j] - nums[j + 1] + 1);
                }
                ans[i] += d;
            }
        }
        return Math.Min(ans[0], ans[1]);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
