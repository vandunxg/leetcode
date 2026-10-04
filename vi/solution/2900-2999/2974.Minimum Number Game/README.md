---
comments: true
difficulty: Easy
rating: 1184
source: Weekly Contest 377 Q1
tags:
    - Array
    - Sorting
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2974. Minimum Number Game](https://leetcode.com/problems/minimum-number-game)

[中文文档](/solution/2900-2999/2974.Minimum%20Number%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> có độ dài <strong>chẵn</strong>, đồng thời có một mảng rỗng <code>arr</code>. Alice và Bob quyết định chơi một trò chơi, trong đó mỗi vòng Alice và Bob thực hiện một thao tác. Luật chơi như sau:</p>

<ul>
	<li>Trong mỗi vòng, trước tiên Alice sẽ loại bỏ phần tử <strong>nhỏ nhất</strong> khỏi <code>nums</code>, sau đó Bob cũng làm như vậy.</li>
	<li>Tiếp theo, Bob sẽ thêm phần tử đã loại bỏ vào mảng <code>arr</code> trước, rồi Alice cũng làm như vậy.</li>
	<li>Trò chơi tiếp tục cho đến khi <code>nums</code> trở thành mảng rỗng.</li>
</ul>

<p>Trả về <em>mảng kết quả</em> <code>arr</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,4,2,3]
<strong>Đầu ra:</strong> [3,2,5,4]
<strong>Giải thích:</strong> Trong vòng đầu tiên, Alice loại bỏ 2 trước, sau đó Bob loại bỏ 3. Tiếp theo, Bob thêm 3 vào arr trước, rồi Alice thêm 2. Vì vậy arr = [3,2].
Ở đầu vòng thứ hai, nums = [5,4]. Lúc này, Alice loại bỏ 4 trước, sau đó Bob loại bỏ 5. Tiếp theo, cả hai lần lượt thêm phần tử vào arr, kết quả là [3,2,5,4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,5]
<strong>Đầu ra:</strong> [5,2]
<strong>Giải thích:</strong> Trong vòng đầu tiên, Alice loại bỏ 2 trước, sau đó Bob loại bỏ 5. Tiếp theo, Bob thêm 5 vào arr trước, rồi Alice thêm 2. Vì vậy arr = [5,2].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>nums.length % 2 == 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng + Priority Queue (Min Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi vòng lấy ra hai phần tử nhỏ nhất hiện tại nhưng thêm phần tử lớn hơn trước. Điều đó tương đương với việc lấy ra hai phần tử từ heap rồi đảo thứ tự. Vì $n \le 100$ và là số chẵn, heap mô phỏng đúng theo đề bài.

<!-- thinking:end -->

Ta có thể lần lượt đưa các phần tử của mảng $\textit{nums}$ vào một min heap. Mỗi lần, ta lấy ra hai phần tử $a$ và $b$ từ min heap, sau đó lần lượt đưa $b$ và $a$ vào mảng kết quả cho đến khi min heap rỗng.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberGame(self, nums: List[int]) -> List[int]:
        heapify(nums)
        ans = []
        while nums:
            a, b = heappop(nums), heappop(nums)
            ans.append(b)
            ans.append(a)
        return ans
```

#### Java

```java
class Solution {
    public int[] numberGame(int[] nums) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int x : nums) {
            pq.offer(x);
        }
        int[] ans = new int[nums.length];
        int i = 0;
        while (!pq.isEmpty()) {
            int a = pq.poll();
            ans[i++] = pq.poll();
            ans[i++] = a;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> numberGame(vector<int>& nums) {
        priority_queue<int, vector<int>, greater<int>> pq;
        for (int x : nums) {
            pq.push(x);
        }
        vector<int> ans;
        while (pq.size()) {
            int a = pq.top();
            pq.pop();
            int b = pq.top();
            pq.pop();
            ans.push_back(b);
            ans.push_back(a);
        }
        return ans;
    }
};
```

#### Go

```go
func numberGame(nums []int) (ans []int) {
	pq := &hp{nums}
	heap.Init(pq)
	for pq.Len() > 0 {
		a := heap.Pop(pq).(int)
		b := heap.Pop(pq).(int)
		ans = append(ans, b)
		ans = append(ans, a)
	}
	return
}

type hp struct{ sort.IntSlice }

func (h *hp) Less(i, j int) bool { return h.IntSlice[i] < h.IntSlice[j] }
func (h *hp) Pop() interface{} {
	old := h.IntSlice
	n := len(old)
	x := old[n-1]
	h.IntSlice = old[0 : n-1]
	return x
}
func (h *hp) Push(x interface{}) {
	h.IntSlice = append(h.IntSlice, x.(int))
}
```

#### TypeScript

```ts
function numberGame(nums: number[]): number[] {
    const pq = new MinPriorityQueue<number>();
    for (const x of nums) {
        pq.enqueue(x);
    }
    const ans: number[] = [];
    while (pq.size()) {
        const a = pq.dequeue();
        const b = pq.dequeue();
        ans.push(b, a);
    }
    return ans;
}
```

#### Rust

```rust
use std::cmp::Reverse;
use std::collections::BinaryHeap;

impl Solution {
    pub fn number_game(nums: Vec<i32>) -> Vec<i32> {
        let mut pq = BinaryHeap::new();

        for &x in &nums {
            pq.push(Reverse(x));
        }

        let mut ans = Vec::new();

        while let Some(Reverse(a)) = pq.pop() {
            if let Some(Reverse(b)) = pq.pop() {
                ans.push(b);
                ans.push(a);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Hoán đổi

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 duy trì một heap, nhưng sau khi sắp xếp thì dãy các phần tử nhỏ nhất toàn cục đã được xác định: mỗi cặp liền kề $(a_{2i},a_{2i+1})$ chính là một vòng. Hoán đổi hai phần tử tại chỗ; không cần cấu trúc động. Sắp xếp vẫn là thao tác chi phối độ phức tạp.

<!-- thinking:end -->

Ta có thể sắp xếp mảng $\textit{nums}$, sau đó duyệt qua mảng và hoán đổi các phần tử liền kề ở mỗi lần cho đến khi duyệt hết mảng, rồi trả về mảng đã hoán đổi.

Độ phức tạp thời gian là $O(n \log n)$, độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberGame(self, nums: List[int]) -> List[int]:
        nums.sort()
        for i in range(0, len(nums), 2):
            nums[i], nums[i + 1] = nums[i + 1], nums[i]
        return nums
```

#### Java

```java
class Solution {
    public int[] numberGame(int[] nums) {
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i += 2) {
            int t = nums[i];
            nums[i] = nums[i + 1];
            nums[i + 1] = t;
        }
        return nums;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> numberGame(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        for (int i = 0; i < n; i += 2) {
            swap(nums[i], nums[i + 1]);
        }
        return nums;
    }
};
```

#### Go

```go
func numberGame(nums []int) []int {
	sort.Ints(nums)
	for i := 0; i < len(nums); i += 2 {
		nums[i], nums[i+1] = nums[i+1], nums[i]
	}
	return nums
}
```

#### TypeScript

```ts
function numberGame(nums: number[]): number[] {
    nums.sort((a, b) => a - b);
    for (let i = 0; i < nums.length; i += 2) {
        [nums[i], nums[i + 1]] = [nums[i + 1], nums[i]];
    }
    return nums;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_game(nums: Vec<i32>) -> Vec<i32> {
        let mut nums = nums;
        nums.sort_unstable();
        for i in (0..nums.len()).step_by(2) {
            nums.swap(i, i + 1);
        }
        nums
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
