---
comments: true
difficulty: Medium
tags:
    - Stack
    - Graph
    - Array
    - Dynamic Programming
    - Shortest Path
    - Monotonic Stack
---

<!-- problem:start -->

# [2297. Jump Game VIII 🔒](https://leetcode.com/problems/jump-game-viii)

[Tài liệu tiếng Trung](/solution/2200-2299/2297.Jump%20Game%20VIII/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>, được đánh chỉ số từ <strong>0</strong>. Ban đầu, bạn đứng tại chỉ số <code>0</code>. Bạn có thể nhảy từ chỉ số <code>i</code> đến chỉ số <code>j</code> với <code>i &lt; j</code> nếu:</p>

<ul>
	<li><code>nums[i] &lt;= nums[j]</code> và <code>nums[k] &lt; nums[i]</code> với mọi chỉ số <code>k</code> trong khoảng <code>i &lt; k &lt; j</code>, hoặc</li>
	<li><code>nums[i] &gt; nums[j]</code> và <code>nums[k] &gt;= nums[i]</code> với mọi chỉ số <code>k</code> trong khoảng <code>i &lt; k &lt; j</code>.</li>
</ul>

<p>Bạn cũng được cho một mảng số nguyên <code>costs</code> có độ dài <code>n</code>, trong đó <code>costs[i]</code> là chi phí để nhảy <strong>đến</strong> chỉ số <code>i</code>.</p>

<p>Hãy trả về <em>chi phí <strong>nhỏ nhất</strong> để nhảy đến chỉ số </em><code>n - 1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,4,4,1], costs = [3,7,6,4,2]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Bạn bắt đầu tại chỉ số 0.
- Nhảy đến chỉ số 2 với chi phí costs[2] = 6.
- Nhảy đến chỉ số 4 với chi phí costs[4] = 2.
Tổng chi phí là 8. Có thể chứng minh rằng 8 là chi phí nhỏ nhất cần thiết.
Hai đường đi khả dĩ khác là từ chỉ số 0 -&gt; 1 -&gt; 4 và từ chỉ số 0 -&gt; 2 -&gt; 3 -&gt; 4.
Tổng chi phí của chúng lần lượt là 9 và 12.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,2], costs = [1,1,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Bắt đầu tại chỉ số 0.
- Nhảy đến chỉ số 1 với chi phí costs[1] = 1.
- Nhảy đến chỉ số 2 với chi phí costs[2] = 1.
Tổng chi phí là 2. Lưu ý rằng bạn không thể nhảy trực tiếp từ chỉ số 0 đến chỉ số 2 vì nums[0] &lt;= nums[1].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length == costs.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i], costs[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic Stack + Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Từ $i$, ta có thể nhảy đến chỉ số tiếp theo có giá trị $\ge nums[i]$ hoặc chỉ số tiếp theo có giá trị $< nums[i]$, và cần tìm chi phí nhỏ nhất để đi đến cuối. Việc duyệt sang phải từ mọi $i$ sẽ quá chậm. Hai vị trí kế tiếp này chính là những vị trí mà monotonic stack có thể tìm được trong thời gian tuyến tính.
>
> Ta xây dựng $g[i]$ bằng một increasing stack và một non-increasing stack khi duyệt từ phải sang trái, sau đó thực hiện chuyển trạng thái $f[j] = \min(f[j], f[i]+costs[j])$ theo thứ tự chỉ số. Số cạnh chỉ là $O(n)$.

<!-- thinking:end -->

Theo mô tả bài toán, ta cần tìm vị trí kế tiếp $j$ sao cho $\textit{nums}[j]$ lớn hơn hoặc bằng $\textit{nums}[i]$, và vị trí kế tiếp $j$ sao cho $\textit{nums}[j]$ nhỏ hơn $\textit{nums}[i]$. Ta có thể dùng monotonic stack để tìm hai vị trí này trong thời gian $O(n)$, sau đó xây dựng danh sách kề $g$, trong đó $g[i]$ biểu diễn các chỉ số mà chỉ số $i$ có thể nhảy đến.

Tiếp theo, ta dùng dynamic programming để tìm chi phí nhỏ nhất. Gọi $f[i]$ là chi phí nhỏ nhất để nhảy đến chỉ số $i$. Ban đầu, $f[0] = 0$ và các $f[i] = \infty$ còn lại. Ta duyệt các chỉ số $i$ từ nhỏ đến lớn. Với mỗi $i$, ta duyệt từng chỉ số $j$ trong $g[i]$ và thực hiện chuyển trạng thái $f[j] = \min(f[j], f[i] + \textit{costs}[j])$. Đáp án là $f[n - 1]$.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, nums: List[int], costs: List[int]) -> int:
        n = len(nums)
        g = defaultdict(list)
        stk = []
        for i in range(n - 1, -1, -1):
            while stk and nums[stk[-1]] < nums[i]:
                stk.pop()
            if stk:
                g[i].append(stk[-1])
            stk.append(i)

        stk = []
        for i in range(n - 1, -1, -1):
            while stk and nums[stk[-1]] >= nums[i]:
                stk.pop()
            if stk:
                g[i].append(stk[-1])
            stk.append(i)

        f = [inf] * n
        f[0] = 0
        for i in range(n):
            for j in g[i]:
                f[j] = min(f[j], f[i] + costs[j])
        return f[n - 1]
```

#### Java

```java
class Solution {
    public long minCost(int[] nums, int[] costs) {
        int n = nums.length;
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && nums[stk.peek()] < nums[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                g[i].add(stk.peek());
            }
            stk.push(i);
        }
        stk.clear();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && nums[stk.peek()] >= nums[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                g[i].add(stk.peek());
            }
            stk.push(i);
        }
        long[] f = new long[n];
        Arrays.fill(f, 1L << 60);
        f[0] = 0;
        for (int i = 0; i < n; ++i) {
            for (int j : g[i]) {
                f[j] = Math.min(f[j], f[i] + costs[j]);
            }
        }
        return f[n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minCost(vector<int>& nums, vector<int>& costs) {
        int n = nums.size();
        vector<int> g[n];
        stack<int> stk;
        for (int i = n - 1; ~i; --i) {
            while (!stk.empty() && nums[stk.top()] < nums[i]) {
                stk.pop();
            }
            if (!stk.empty()) {
                g[i].push_back(stk.top());
            }
            stk.push(i);
        }
        stk = stack<int>();
        for (int i = n - 1; ~i; --i) {
            while (!stk.empty() && nums[stk.top()] >= nums[i]) {
                stk.pop();
            }
            if (!stk.empty()) {
                g[i].push_back(stk.top());
            }
            stk.push(i);
        }
        vector<long long> f(n, 1e18);
        f[0] = 0;
        for (int i = 0; i < n; ++i) {
            for (int j : g[i]) {
                f[j] = min(f[j], f[i] + costs[j]);
            }
        }
        return f[n - 1];
    }
};
```

#### Go

```go
func minCost(nums []int, costs []int) int64 {
	n := len(nums)
	g := make([][]int, n)
	stk := []int{}
	for i := n - 1; i >= 0; i-- {
		for len(stk) > 0 && nums[stk[len(stk)-1]] < nums[i] {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			g[i] = append(g[i], stk[len(stk)-1])
		}
		stk = append(stk, i)
	}
	stk = []int{}
	for i := n - 1; i >= 0; i-- {
		for len(stk) > 0 && nums[stk[len(stk)-1]] >= nums[i] {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			g[i] = append(g[i], stk[len(stk)-1])
		}
		stk = append(stk, i)
	}
	f := make([]int64, n)
	for i := 1; i < n; i++ {
		f[i] = math.MaxInt64
	}
	for i := 0; i < n; i++ {
		for _, j := range g[i] {
			f[j] = min(f[j], f[i]+int64(costs[j]))
		}
	}
	return f[n-1]
}
```

#### TypeScript

```ts
function minCost(nums: number[], costs: number[]): number {
    const n = nums.length;
    const g: number[][] = Array.from({ length: n }, () => []);
    const stk: number[] = [];
    for (let i = n - 1; i >= 0; --i) {
        while (stk.length && nums[stk[stk.length - 1]] < nums[i]) {
            stk.pop();
        }
        if (stk.length) {
            g[i].push(stk[stk.length - 1]);
        }
        stk.push(i);
    }
    stk.length = 0;
    for (let i = n - 1; i >= 0; --i) {
        while (stk.length && nums[stk[stk.length - 1]] >= nums[i]) {
            stk.pop();
        }
        if (stk.length) {
            g[i].push(stk[stk.length - 1]);
        }
        stk.push(i);
    }
    const f: number[] = Array.from({ length: n }, () => Infinity);
    f[0] = 0;
    for (let i = 0; i < n; ++i) {
        for (const j of g[i]) {
            f[j] = Math.min(f[j], f[i] + costs[j]);
        }
    }
    return f[n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
