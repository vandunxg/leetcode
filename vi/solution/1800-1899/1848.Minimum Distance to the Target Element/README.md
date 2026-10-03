---
comments: true
difficulty: Easy
rating: 1216
source: Weekly Contest 239 Q1
tags:
    - Array
---

<!-- problem:start -->

# [1848. Minimum Distance to the Target Element](https://leetcode.com/problems/minimum-distance-to-the-target-element)

[中文文档](/solution/1800-1899/1848.Minimum%20Distance%20to%20the%20Target%20Element/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> <strong>(đánh chỉ số từ 0)</strong> và hai số nguyên <code>target</code> và <code>start</code>, hãy tìm một chỉ số <code>i</code> sao cho <code>nums[i] == target</code> và <code>abs(i - start)</code> được <strong>tối thiểu hóa</strong>. Lưu ý rằng&nbsp;<code>abs(x)</code>&nbsp;là giá trị tuyệt đối của <code>x</code>.</p>

<p>Trả về <code>abs(i - start)</code>.</p>

<p><strong>Đảm bảo</strong> <code>target</code> tồn tại trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5], target = 5, start = 3
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> nums[4] = 5 là giá trị duy nhất bằng target, nên đáp án là abs(4 - 3) = 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1], target = 1, start = 0
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> nums[0] = 1 là giá trị duy nhất bằng target, nên đáp án là abs(0 - 0) = 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,1,1,1,1,1,1,1], target = 1, start = 0
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mọi giá trị của nums đều bằng 1, nhưng nums[0] tối thiểu hóa abs(i - start), có giá trị abs(0 - 0) = 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= start &lt; nums.length</code></li>
	<li><code>target</code> nằm trong <code>nums</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Đảm bảo target xuất hiện; ta cần chỉ số gần $\textit{start}$ nhất. Không cần cấu trúc dữ liệu phụ.
>
> Duyệt mọi chỉ số có giá trị bằng $\textit{target}$ và lấy giá trị nhỏ nhất của $|i-\textit{start}|$.

<!-- thinking:end -->

Duyệt mảng, tìm tất cả chỉ số có giá trị bằng $target$, sau đó tính $|i - start|$ và lấy giá trị nhỏ nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getMinDistance(self, nums: List[int], target: int, start: int) -> int:
        return min(abs(i - start) for i, x in enumerate(nums) if x == target)
```

#### Java

```java
class Solution {
    public int getMinDistance(int[] nums, int target, int start) {
        int n = nums.length;
        int ans = n;
        for (int i = 0; i < n; ++i) {
            if (nums[i] == target) {
                ans = Math.min(ans, Math.abs(i - start));
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
    int getMinDistance(vector<int>& nums, int target, int start) {
        int n = nums.size();
        int ans = n;
        for (int i = 0; i < n; ++i) {
            if (nums[i] == target) {
                ans = min(ans, abs(i - start));
            }
        }
        return ans;
    }
};
```

#### Go

```go
func getMinDistance(nums []int, target int, start int) int {
	ans := 1 << 30
	for i, x := range nums {
		if t := abs(i - start); x == target && t < ans {
			ans = t
		}
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function getMinDistance(nums: number[], target: number, start: number): number {
    let ans = Infinity;
    for (let i = 0; i < nums.length; ++i) {
        if (nums[i] === target) {
            ans = Math.min(ans, Math.abs(i - start));
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn get_min_distance(nums: Vec<i32>, target: i32, start: i32) -> i32 {
        nums.iter()
            .enumerate()
            .filter(|&(_, &x)| x == target)
            .map(|(i, _)| ((i as i32) - start).abs())
            .min()
            .unwrap_or_default()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
