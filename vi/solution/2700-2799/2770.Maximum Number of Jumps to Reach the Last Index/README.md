---
comments: true
difficulty: Medium
rating: 1533
source: Weekly Contest 353 Q2
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2770. Maximum Number of Jumps to Reach the Last Index](https://leetcode.com/problems/maximum-number-of-jumps-to-reach-the-last-index)

[中文文档](/solution/2700-2799/2770.Maximum%20Number%20of%20Jumps%20to%20Reach%20the%20Last%20Index/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> gồm <code>n</code> phần tử được đánh chỉ số bắt đầu từ <strong>0</strong> và một số nguyên <code>target</code>.</p>

<p>Ban đầu, bạn đứng ở chỉ số <code>0</code>. Trong một bước, bạn có thể nhảy từ chỉ số <code>i</code> đến bất kỳ chỉ số <code>j</code> nào thỏa mãn:</p>

<ul>
	<li><code>0 &lt;= i &lt; j &lt; n</code></li>
	<li><code>-target &lt;= nums[j] - nums[i] &lt;= target</code></li>
</ul>

<p>Trả về <em><strong>số lần nhảy lớn nhất</strong> bạn có thể thực hiện để đến chỉ số</em> <code>n - 1</code>.</p>

<p>Nếu không có cách nào để đến chỉ số <code>n - 1</code>, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,6,4,1,2], target = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Để đi từ chỉ số 0 đến chỉ số n - 1 với số lần nhảy lớn nhất, ta có thể thực hiện chuỗi nhảy sau:
- Nhảy từ chỉ số 0 đến chỉ số 1.
- Nhảy từ chỉ số 1 đến chỉ số 3.
- Nhảy từ chỉ số 3 đến chỉ số 5.
Có thể chứng minh rằng không có chuỗi nhảy nào khác đi từ 0 đến n - 1 với hơn 3 lần nhảy. Do đó, đáp án là 3. </pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,6,4,1,2], target = 3
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Để đi từ chỉ số 0 đến chỉ số n - 1 với số lần nhảy lớn nhất, ta có thể thực hiện chuỗi nhảy sau:
- Nhảy từ chỉ số 0 đến chỉ số 1.
- Nhảy từ chỉ số 1 đến chỉ số 2.
- Nhảy từ chỉ số 2 đến chỉ số 3.
- Nhảy từ chỉ số 3 đến chỉ số 4.
- Nhảy từ chỉ số 4 đến chỉ số 5.
Có thể chứng minh rằng không có chuỗi nhảy nào khác đi từ 0 đến n - 1 với hơn 5 lần nhảy. Do đó, đáp án là 5. </pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,6,4,1,2], target = 0
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Có thể chứng minh rằng không có chuỗi nhảy nào đi từ 0 đến n - 1. Do đó, đáp án là -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length == n &lt;= 1000</code></li>
	<li><code>-10<sup>9</sup>&nbsp;&lt;= nums[i]&nbsp;&lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= target &lt;= 2 * 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Ta nhảy từ chỉ số $0$ đến cuối mảng, mỗi bước yêu cầu hiệu tuyệt đối không vượt quá $target$, đồng thời cần tối đa hóa số lần nhảy. Vì $n\le 1000$, có thể mô hình hóa thành bài toán đường đi, nhưng mục tiêu ở đây là tối đa hóa số lần nhảy.
>
> $dfs(i)$ là số lần nhảy lớn nhất bắt đầu từ $i$: lấy $1+dfs(j)$ trên mọi $j>i$ hợp lệ, trả về $0$ tại cuối mảng, và trả về $-\infty$ khi không thể nhảy. Nếu giá trị đã memoize là số âm, trả về $-1$.

<!-- thinking:end -->

Với mỗi vị trí $i$, ta xét việc nhảy đến vị trí $j$ thỏa mãn $|nums[i] - nums[j]| \leq target$. Khi đó, ta có thể nhảy từ $i$ đến $j$, rồi tiếp tục nhảy từ $j$ đến cuối mảng.

Vì vậy, ta xây dựng hàm $dfs(i)$ biểu diễn số lần nhảy lớn nhất cần thực hiện để đến chỉ số cuối, bắt đầu từ vị trí $i$. Khi đó, đáp án là $dfs(0)$.

Quá trình tính hàm $dfs(i)$ như sau:

- Nếu $i = n - 1$, ta đã đến chỉ số cuối và không cần nhảy thêm, nên trả về $0$;
- Nếu không, ta liệt kê các vị trí $j$ có thể nhảy đến từ vị trí $i$, rồi tính số lần nhảy lớn nhất cần thực hiện để đến chỉ số cuối khi bắt đầu từ $j$. Khi đó, $dfs(i)$ bằng giá trị lớn nhất của mọi $dfs(j)$ cộng $1$. Nếu không có vị trí $j$ nào có thể nhảy đến từ $i$, thì $dfs(i) = -\infty$.

Để tránh tính toán trùng lặp, ta có thể sử dụng memoization.

Độ phức tạp thời gian là $O(n^2)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumJumps(self, nums: List[int], target: int) -> int:
        @cache
        def dfs(i: int) -> int:
            if i == n - 1:
                return 0
            ans = -inf
            for j in range(i + 1, n):
                if abs(nums[i] - nums[j]) <= target:
                    ans = max(ans, 1 + dfs(j))
            return ans

        n = len(nums)
        ans = dfs(0)
        return -1 if ans < 0 else ans
```

#### Java

```java
class Solution {
    private Integer[] f;
    private int[] nums;
    private int n;
    private int target;

    public int maximumJumps(int[] nums, int target) {
        n = nums.length;
        this.target = target;
        this.nums = nums;
        f = new Integer[n];
        int ans = dfs(0);
        return ans < 0 ? -1 : ans;
    }

    private int dfs(int i) {
        if (i == n - 1) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        int ans = -(1 << 30);
        for (int j = i + 1; j < n; ++j) {
            if (Math.abs(nums[i] - nums[j]) <= target) {
                ans = Math.max(ans, 1 + dfs(j));
            }
        }
        return f[i] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumJumps(vector<int>& nums, int target) {
        int n = nums.size();
        int f[n];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i) -> int {
            if (i == n - 1) {
                return 0;
            }
            if (f[i] != -1) {
                return f[i];
            }
            f[i] = -(1 << 30);
            for (int j = i + 1; j < n; ++j) {
                if (abs(nums[i] - nums[j]) <= target) {
                    f[i] = max(f[i], 1 + dfs(j));
                }
            }
            return f[i];
        };
        int ans = dfs(0);
        return ans < 0 ? -1 : ans;
    }
};
```

#### Go

```go
func maximumJumps(nums []int, target int) int {
	n := len(nums)
	f := make([]int, n)
	for i := range f {
		f[i] = -1
	}
	var dfs func(int) int
	dfs = func(i int) int {
		if i == n-1 {
			return 0
		}
		if f[i] != -1 {
			return f[i]
		}
		f[i] = -(1 << 30)
		for j := i + 1; j < n; j++ {
			if abs(nums[i]-nums[j]) <= target {
				f[i] = max(f[i], 1+dfs(j))
			}
		}
		return f[i]
	}
	ans := dfs(0)
	if ans < 0 {
		return -1
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
function maximumJumps(nums: number[], target: number): number {
    const n = nums.length;
    const f: number[] = Array(n).fill(-1);
    const dfs = (i: number): number => {
        if (i === n - 1) {
            return 0;
        }
        if (f[i] !== -1) {
            return f[i];
        }
        f[i] = -(1 << 30);
        for (let j = i + 1; j < n; ++j) {
            if (Math.abs(nums[i] - nums[j]) <= target) {
                f[i] = Math.max(f[i], 1 + dfs(j));
            }
        }
        return f[i];
    };
    const ans = dfs(0);
    return ans < 0 ? -1 : ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_jumps(nums: Vec<i32>, target: i32) -> i32 {
        let n = nums.len();
        let mut f = vec![-1; n];

        fn dfs(i: usize, nums: &Vec<i32>, target: i32, f: &mut Vec<i32>) -> i32 {
            if i == nums.len() - 1 {
                return 0;
            }
            if f[i] != -1 {
                return f[i];
            }
            f[i] = -(1 << 30);
            for j in i + 1..nums.len() {
                if (nums[i] - nums[j]).abs() <= target {
                    f[i] = f[i].max(1 + dfs(j, nums, target, f));
                }
            }
            f[i]
        }

        let ans = dfs(0, &nums, target, &mut f);
        if ans < 0 { -1 } else { ans }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
