---
comments: true
difficulty: Hard
rating: 1945
source: Weekly Contest 232 Q4
tags:
    - Stack
    - Array
    - Two Pointers
    - Binary Search
    - Cartesian Tree
    - Monotonic Stack
---

<!-- problem:start -->

# [1793. Maximum Score of a Good Subarray](https://leetcode.com/problems/maximum-score-of-a-good-subarray)

[中文文档](/solution/1700-1799/1793.Maximum%20Score%20of%20a%20Good%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> <strong>(đánh chỉ số từ 0)</strong> và số nguyên <code>k</code>.</p>

<p><strong>Điểm số</strong> của mảng con <code>(i, j)</code> được định nghĩa là <code>min(nums[i], nums[i+1], ..., nums[j]) * (j - i + 1)</code>. Mảng con <strong>tốt</strong> là mảng con thỏa mãn <code>i &lt;= k &lt;= j</code>.</p>

<p>Trả về <em><strong>điểm số</strong> lớn nhất có thể của một mảng con <strong>tốt</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,4,3,7,4,5], k = 3
<strong>Output:</strong> 15
<strong>Giải thích:</strong> Mảng con tối ưu là (1, 5), có điểm min(4,3,7,4,5) * (5-1+1) = 3 * 5 = 15.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [5,5,4,5,4,1,1,1], k = 0
<strong>Output:</strong> 20
<strong>Giải thích:</strong> Mảng con tối ưu là (0, 4), có điểm min(5,5,4,5,4) * (4-0+1) = 4 * 5 = 20.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= k &lt; nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ngăn xếp đơn điệu

<!-- thinking:start -->

> **Tư duy**
>
> Mảng con tốt phải chứa chỉ số $k$; điểm số bằng độ dài nhân với giá trị nhỏ nhất. Với $n\le 10^5$, không thể thử mọi cửa sổ chứa $k$.
>
> Xét từng chỉ số là vị trí của giá trị nhỏ nhất: monotonic stack tìm giá trị nhỏ hơn nghiêm ngặt đầu tiên ở hai phía. Nếu đoạn đó vẫn chứa $k$, cập nhật bằng $v\times\textit{width}$.

<!-- thinking:end -->

Ta có thể xét mỗi phần tử $nums[i]$ trong $nums$ là giá trị nhỏ nhất của mảng con, rồi dùng monotonic stack để tìm vị trí đầu tiên $left[i]$ bên trái có giá trị nhỏ hơn $nums[i]$ và vị trí đầu tiên $right[i]$ bên phải có giá trị nhỏ hơn hoặc bằng $nums[i]$. Khi đó, điểm số của mảng con có $nums[i]$ là giá trị nhỏ nhất là $nums[i] \times (right[i] - left[i] - 1)$.

Lưu ý rằng chỉ được cập nhật đáp án khi hai biên $left[i]$ và $right[i]$ thỏa mãn $left[i]+1 \leq k \leq right[i]-1$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumScore(self, nums: List[int], k: int) -> int:
        n = len(nums)
        left = [-1] * n
        right = [n] * n
        stk = []
        for i, v in enumerate(nums):
            while stk and nums[stk[-1]] >= v:
                stk.pop()
            if stk:
                left[i] = stk[-1]
            stk.append(i)
        stk = []
        for i in range(n - 1, -1, -1):
            v = nums[i]
            while stk and nums[stk[-1]] > v:
                stk.pop()
            if stk:
                right[i] = stk[-1]
            stk.append(i)
        ans = 0
        for i, v in enumerate(nums):
            if left[i] + 1 <= k <= right[i] - 1:
                ans = max(ans, v * (right[i] - left[i] - 1))
        return ans
```

#### Java

```java
class Solution {
    public int maximumScore(int[] nums, int k) {
        int n = nums.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, -1);
        Arrays.fill(right, n);
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            int v = nums[i];
            while (!stk.isEmpty() && nums[stk.peek()] >= v) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                left[i] = stk.peek();
            }
            stk.push(i);
        }
        stk.clear();
        for (int i = n - 1; i >= 0; --i) {
            int v = nums[i];
            while (!stk.isEmpty() && nums[stk.peek()] > v) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                right[i] = stk.peek();
            }
            stk.push(i);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (left[i] + 1 <= k && k <= right[i] - 1) {
                ans = Math.max(ans, nums[i] * (right[i] - left[i] - 1));
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
    int maximumScore(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> left(n, -1);
        vector<int> right(n, n);
        stack<int> stk;
        for (int i = 0; i < n; ++i) {
            int v = nums[i];
            while (!stk.empty() && nums[stk.top()] >= v) {
                stk.pop();
            }
            if (!stk.empty()) {
                left[i] = stk.top();
            }
            stk.push(i);
        }
        stk = stack<int>();
        for (int i = n - 1; i >= 0; --i) {
            int v = nums[i];
            while (!stk.empty() && nums[stk.top()] > v) {
                stk.pop();
            }
            if (!stk.empty()) {
                right[i] = stk.top();
            }
            stk.push(i);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (left[i] + 1 <= k && k <= right[i] - 1) {
                ans = max(ans, nums[i] * (right[i] - left[i] - 1));
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maximumScore(nums []int, k int) (ans int) {
	n := len(nums)
	left := make([]int, n)
	right := make([]int, n)
	for i := range left {
		left[i] = -1
		right[i] = n
	}
	stk := []int{}
	for i, v := range nums {
		for len(stk) > 0 && nums[stk[len(stk)-1]] >= v {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			left[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	stk = []int{}
	for i := n - 1; i >= 0; i-- {
		v := nums[i]
		for len(stk) > 0 && nums[stk[len(stk)-1]] > v {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			right[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	for i, v := range nums {
		if left[i]+1 <= k && k <= right[i]-1 {
			ans = max(ans, v*(right[i]-left[i]-1))
		}
	}
	return
}
```

#### TypeScript

```ts
function maximumScore(nums: number[], k: number): number {
    const n = nums.length;
    const left: number[] = Array(n).fill(-1);
    const right: number[] = Array(n).fill(n);
    const stk: number[] = [];
    for (let i = 0; i < n; ++i) {
        while (stk.length && nums[stk.at(-1)] >= nums[i]) {
            stk.pop();
        }
        if (stk.length) {
            left[i] = stk.at(-1);
        }
        stk.push(i);
    }
    stk.length = 0;
    for (let i = n - 1; ~i; --i) {
        while (stk.length && nums[stk.at(-1)] > nums[i]) {
            stk.pop();
        }
        if (stk.length) {
            right[i] = stk.at(-1);
        }
        stk.push(i);
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        if (left[i] + 1 <= k && k <= right[i] - 1) {
            ans = Math.max(ans, nums[i] * (right[i] - left[i] - 1));
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hai con trỏ

<!-- thinking:start -->

> **Thinking**
>
> Ngăn xếp sử dụng bộ nhớ phụ tuyến tính. Hai con trỏ bắt đầu từ $k$ có thể mở rộng cửa sổ, luôn mở rộng về phía phần tử kề lớn hơn để giá trị nhỏ nhất hiện tại giảm chậm nhất, đồng thời cập nhật $\textit{min}\times\textit{len}$. Không gian phụ là $O(1)$.

<!-- thinking:end -->

Ta có thể khởi tạo hai con trỏ tại chỉ số trung tâm `k` và mở rộng ra hai phía trái, phải.
Bằng cách duy trì giá trị nhỏ nhất trong cửa sổ hiện tại, ta có thể tìm được điểm số lớn nhất trong thời gian tuyến tính chặt.

**Các bước thuật toán:**

1. Khởi tạo con trỏ trái `i = k`, con trỏ phải `j = k` và giá trị nhỏ nhất của cửa sổ `min_num = nums[k]`. Đặt điểm số lớn nhất ban đầu là `max_score = nums[k]`.

2. Mở rộng các con trỏ khi `i > 0` hoặc `j < len(nums) - 1`:
    - **Hướng**: Nếu biên trái không thể mở rộng (`i == 0`), di chuyển con trỏ phải `j++`. Nếu biên phải không thể mở rộng (`j == len(nums) - 1`), di chuyển con trỏ trái `i--`.
    - Nếu cả hai phía đều có thể mở rộng, so sánh `nums[i - 1]` và `nums[j + 1]`, rồi mở rộng về phía có giá trị lớn hơn (tức là nếu `nums[i - 1] >= nums[j + 1]`, giảm `i`; ngược lại, tăng `j`).

3. **Cập nhật trạng thái**: sau mỗi lần di chuyển con trỏ, cập nhật giá trị nhỏ nhất của cửa sổ hiện tại: `min_num = min(min_num, nums[i] or nums[j])`.

4. **Tính điểm**: độ dài của mảng con tốt hiện tại là `j + 1 - i`, và điểm số là `score = min_num * (j + 1 - i)`. Cập nhật điểm số lớn nhất toàn cục: `max_score = max(max_score, score)`.

5. Trả về `max_score` khi vòng lặp for kết thúc.

**Phân tích độ phức tạp:**

- **Độ phức tạp thời gian**: $O(n)$, trong đó $n$ là độ dài mảng `nums`. Mỗi phần tử được duyệt nhiều nhất một lần.
- **Độ phức tạp không gian**: $O(1)$, vì chỉ cần một lượng không gian phụ hằng số cho hai con trỏ, giá trị nhỏ nhất của cửa sổ và điểm số lớn nhất toàn cục.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumScore(self, nums: list[int], k: int) -> int:
        max_score = nums[k]  # Base case.
        min_num = nums[k]

        left_idx, right_idx = k, k

        while 0 < left_idx or right_idx < len(nums) - 1:
            if left_idx == 0:  # Can only go right.
                right_idx += 1
                min_num = min(min_num, nums[right_idx])

            elif right_idx == len(nums) - 1:  # Can only go left.
                left_idx -= 1
                min_num = min(min_num, nums[left_idx])

            else:  # Can go bidirectional.
                if nums[left_idx - 1] >= nums[right_idx + 1]:
                    left_idx -= 1
                    min_num = min(min_num, nums[left_idx])

                else:
                    right_idx += 1
                    min_num = min(min_num, nums[right_idx])

            score = min_num * (right_idx + 1 - left_idx)
            max_score = max(max_score, score)

        return max_score
```

#### C++

```cpp
class Solution {
public:
    int maximumScore(vector<int>& nums, int k) {
        int maxScore = nums[k], minNum = nums[k]; // Base case.

        int leftIdx = k, rightIdx = k;

        while (0 < leftIdx || rightIdx < nums.size() - 1) {
            if (leftIdx == 0) { // Can only go right.
                rightIdx++;
                minNum = min(minNum, nums[rightIdx]);
            }

            else if (rightIdx == nums.size() - 1) { // Can only go left.
                leftIdx--;
                minNum = min(minNum, nums[leftIdx]);
            }

            else { // Can go bidirectional.
                if (nums[leftIdx - 1] >= nums[rightIdx + 1]) {
                    leftIdx--;
                    minNum = min(minNum, nums[leftIdx]);
                }

                else {
                    rightIdx++;
                    minNum = min(minNum, nums[rightIdx]);
                }
            }

            int score = minNum * (rightIdx + 1 - leftIdx);
            maxScore = max(maxScore, score);
        }

        return maxScore;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
