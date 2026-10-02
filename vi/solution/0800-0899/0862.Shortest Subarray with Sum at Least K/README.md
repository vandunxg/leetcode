---
comments: true
difficulty: Hard
tags:
    - Queue
    - Array
    - Binary Search
    - Prefix Sum
    - Sliding Window
    - Monotonic Queue
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [862. Shortest Subarray with Sum at Least K](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k)

[中文文档](/solution/0800-0899/0862.Shortest%20Subarray%20with%20Sum%20at%20Least%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>, hãy trả về <em>độ dài của <strong>mảng con</strong> không rỗng ngắn nhất trong </em><code>nums</code><em> có tổng ít nhất bằng </em><code>k</code>. Nếu không có <strong>mảng con</strong> nào như vậy, trả về <code>-1</code>.</p>

<p><strong>Mảng con</strong> là một đoạn <strong>liên tiếp</strong> của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [1], k = 1
<strong>Đầu ra:</strong> 1
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [1,2], k = 4
<strong>Đầu ra:</strong> -1
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [2,-1,2], k = 3
<strong>Đầu ra:</strong> 3
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm mảng con ngắn nhất có tổng ít nhất bằng $k$. Số âm khiến sliding window không còn hiệu quả, còn $n\le 10^5$ không cho phép liệt kê với độ phức tạp bậc hai. Với prefix sum, ta cần tìm $j-i$ nhỏ nhất sao cho $s[j]-s[i]\ge k$.
>
> Duy trì deque các chỉ số sao cho prefix sum tăng đơn điệu: pop phần tử đầu khi nó đã tạo tổng ít nhất $k$; loại phần tử cuối nếu prefix sum tại đó lớn hơn hoặc bằng prefix sum hiện tại, vì prefix sum nhỏ hơn ở vị trí muộn hơn là điểm bắt đầu tốt hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestSubarray(self, nums: List[int], k: int) -> int:
        s = list(accumulate(nums, initial=0))
        q = deque()
        ans = inf
        for i, v in enumerate(s):
            while q and v - s[q[0]] >= k:
                ans = min(ans, i - q.popleft())
            while q and s[q[-1]] >= v:
                q.pop()
            q.append(i)
        return -1 if ans == inf else ans
```

#### Java

```java
class Solution {
    public int shortestSubarray(int[] nums, int k) {
        int n = nums.length;
        long[] s = new long[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        Deque<Integer> q = new ArrayDeque<>();
        int ans = n + 1;
        for (int i = 0; i <= n; ++i) {
            while (!q.isEmpty() && s[i] - s[q.peek()] >= k) {
                ans = Math.min(ans, i - q.poll());
            }
            while (!q.isEmpty() && s[q.peekLast()] >= s[i]) {
                q.pollLast();
            }
            q.offer(i);
        }
        return ans > n ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int shortestSubarray(vector<int>& nums, int k) {
        int n = nums.size();
        vector<long> s(n + 1);
        for (int i = 0; i < n; ++i) s[i + 1] = s[i] + nums[i];
        deque<int> q;
        int ans = n + 1;
        for (int i = 0; i <= n; ++i) {
            while (!q.empty() && s[i] - s[q.front()] >= k) {
                ans = min(ans, i - q.front());
                q.pop_front();
            }
            while (!q.empty() && s[q.back()] >= s[i]) q.pop_back();
            q.push_back(i);
        }
        return ans > n ? -1 : ans;
    }
};
```

#### Go

```go
func shortestSubarray(nums []int, k int) int {
	n := len(nums)
	s := make([]int, n+1)
	for i, x := range nums {
		s[i+1] = s[i] + x
	}
	q := []int{}
	ans := n + 1
	for i, v := range s {
		for len(q) > 0 && v-s[q[0]] >= k {
			ans = min(ans, i-q[0])
			q = q[1:]
		}
		for len(q) > 0 && s[q[len(q)-1]] >= v {
			q = q[:len(q)-1]
		}
		q = append(q, i)
	}
	if ans > n {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function shortestSubarray(nums: number[], k: number): number {
    const [n, MAX] = [nums.length, Number.POSITIVE_INFINITY];
    const s = Array(n + 1).fill(0);
    const q: number[] = [];
    let ans = MAX;

    for (let i = 0; i < n; i++) {
        s[i + 1] = s[i] + nums[i];
    }

    for (let i = 0; i < n + 1; i++) {
        while (q.length && s[i] - s[q[0]] >= k) {
            ans = Math.min(ans, i - q.shift()!);
        }

        while (q.length && s[i] <= s[q.at(-1)!]) {
            q.pop();
        }

        q.push(i);
    }

    return ans === MAX ? -1 : ans;
}
```

#### JavaScript

```js
function shortestSubarray(nums, k) {
    const [n, MAX] = [nums.length, Number.POSITIVE_INFINITY];
    const s = Array(n + 1).fill(0);
    const q = [];
    let ans = MAX;

    for (let i = 0; i < n; i++) {
        s[i + 1] = s[i] + nums[i];
    }

    for (let i = 0; i < n + 1; i++) {
        while (q.length && s[i] - s[q[0]] >= k) {
            ans = Math.min(ans, i - q.shift());
        }

        while (q.length && s[i] <= s[q.at(-1)]) {
            q.pop();
        }

        q.push(i);
    }

    return ans === MAX ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
