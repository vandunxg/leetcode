---
comments: true
difficulty: Medium
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [259. 3Sum Smaller 🔒](https://leetcode.com/problems/3sum-smaller)

[中文文档](/solution/0200-0299/0259.3Sum%20Smaller/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng gồm <code>n</code> số nguyên <code>nums</code> và một số nguyên&nbsp;<code>target</code>, hãy tìm số bộ ba chỉ số <code>i</code>, <code>j</code>, <code>k</code> thỏa mãn <code>0 &lt;= i &lt; j &lt; k &lt; n</code> và điều kiện <code>nums[i] + nums[j] + nums[k] &lt; target</code>.</p>
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-2,0,1,3], target = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Vì có hai bộ ba có tổng nhỏ hơn 2:
[-2,0,1]
[-2,0,3]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [], target = 0
<strong>Đầu ra:</strong> 0
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0], target = 0
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>0 &lt;= n &lt;= 3500</code></li>
	<li><code>-100 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>-100 &lt;= target &lt;= 100</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho đáp án không vượt quá 10<sup>9</sup>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sorting + Two Pointers + Enumeration

<!-- thinking:start -->

> **Tư duy**
>
> Thứ tự các phần tử không ảnh hưởng đến kết quả nên ta sắp xếp mảng. Sau khi cố định chỉ số nhỏ nhất $i$, dùng two pointers để đếm các cặp ở bên phải có tổng nhỏ hơn $\textit{target}$.
>
> Nếu $nums[i]+nums[j]+nums[k]<\textit{target}$, mọi $k'$ thuộc $(j,k]$ đều thỏa mãn, nên ta cộng $k-j$ vào đáp án rồi tăng $j$; nếu không, ta giảm $k$.

<!-- thinking:end -->

Vì thứ tự các phần tử không ảnh hưởng đến kết quả, ta có thể sắp xếp mảng trước rồi dùng two pointers để giải bài toán.

Trước tiên, ta sắp xếp mảng rồi lần lượt chọn phần tử đầu tiên $\textit{nums}[i]$. Trong đoạn $\textit{nums}[i+1:n-1]$, ta dùng hai pointer trỏ đến $\textit{nums}[j]$ và $\textit{nums}[k]$, trong đó $j$ là phần tử ngay sau $\textit{nums}[i]$, còn $k$ là phần tử cuối mảng.

- Nếu $\textit{nums}[i] + \textit{nums}[j] + \textit{nums}[k] < \textit{target}$, thì với mọi phần tử $j \lt k' \leq k$, ta có $\textit{nums}[i] + \textit{nums}[j] + \textit{nums}[k'] < \textit{target}$. Có $k - j$ giá trị $k'$ như vậy, nên ta cộng $k - j$ vào đáp án. Tiếp theo, tăng $j$ một vị trí và tiếp tục cho đến khi $j \geq k$.
- Nếu $\textit{nums}[i] + \textit{nums}[j] + \textit{nums}[k] \geq \textit{target}$, thì với mọi phần tử $j \leq j' \lt k$, không thể có $\textit{nums}[i] + \textit{nums}[j'] + \textit{nums}[k] < \textit{target}$. Vì vậy, ta giảm $k$ một vị trí và tiếp tục cho đến khi $j \geq k$.

Sau khi xét hết các giá trị $i$, ta thu được số bộ ba thỏa điều kiện.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def threeSumSmaller(self, nums: List[int], target: int) -> int:
        nums.sort()
        ans, n = 0, len(nums)
        for i in range(n - 2):
            j, k = i + 1, n - 1
            while j < k:
                x = nums[i] + nums[j] + nums[k]
                if x < target:
                    ans += k - j
                    j += 1
                else:
                    k -= 1
        return ans
```

#### Java

```java
class Solution {
    public int threeSumSmaller(int[] nums, int target) {
        Arrays.sort(nums);
        int ans = 0, n = nums.length;
        for (int i = 0; i + 2 < n; ++i) {
            int j = i + 1, k = n - 1;
            while (j < k) {
                int x = nums[i] + nums[j] + nums[k];
                if (x < target) {
                    ans += k - j;
                    ++j;
                } else {
                    --k;
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
    int threeSumSmaller(vector<int>& nums, int target) {
        ranges::sort(nums);
        int ans = 0, n = nums.size();
        for (int i = 0; i + 2 < n; ++i) {
            int j = i + 1, k = n - 1;
            while (j < k) {
                int x = nums[i] + nums[j] + nums[k];
                if (x < target) {
                    ans += k - j;
                    ++j;
                } else {
                    --k;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func threeSumSmaller(nums []int, target int) (ans int) {
	sort.Ints(nums)
	n := len(nums)
	for i := 0; i < n-2; i++ {
		j, k := i+1, n-1
		for j < k {
			x := nums[i] + nums[j] + nums[k]
			if x < target {
				ans += k - j
				j++
			} else {
				k--
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function threeSumSmaller(nums: number[], target: number): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i < n - 2; ++i) {
        let [j, k] = [i + 1, n - 1];
        while (j < k) {
            const x = nums[i] + nums[j] + nums[k];
            if (x < target) {
                ans += k - j;
                ++j;
            } else {
                --k;
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
 * @param {number} target
 * @return {number}
 */
var threeSumSmaller = function (nums, target) {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i < n - 2; ++i) {
        let [j, k] = [i + 1, n - 1];
        while (j < k) {
            const x = nums[i] + nums[j] + nums[k];
            if (x < target) {
                ans += k - j;
                ++j;
            } else {
                --k;
            }
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
