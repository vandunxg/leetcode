---
comments: true
difficulty: Medium
rating: 1399
source: Biweekly Contest 125 Q2
tags:
    - Array
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3066. Minimum Operations to Exceed Threshold Value II](https://leetcode.com/problems/minimum-operations-to-exceed-threshold-value-ii)

[中文文档](/solution/3000-3099/3066.Minimum%20Operations%20to%20Exceed%20Threshold%20Value%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>0-indexed</strong> <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Ta được phép thực hiện một số thao tác trên <code>nums</code>. Trong một thao tác, ta có thể:</p>

<ul>
    <li>Chọn hai số nguyên <strong>nhỏ nhất</strong> <code>x</code> và <code>y</code> từ <code>nums</code>.</li>
    <li>Xóa <code>x</code> và <code>y</code> khỏi <code>nums</code>.</li>
    <li>Chèn <code>(min(x, y) * 2 + max(x, y))</code> vào bất kỳ vị trí nào trong mảng.</li>
</ul>

<p><strong>Lưu ý</strong> rằng chỉ có thể thực hiện thao tác trên khi <code>nums</code> chứa <strong>ít nhất</strong> hai phần tử.</p>

<p>Trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để mọi phần tử của mảng đều <strong>lớn hơn hoặc bằng</strong> <code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,11,10,1,3], k = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ol>
    <li>Trong thao tác đầu tiên, ta xóa các phần tử 1 và 2, sau đó thêm <code>1 * 2 + 2</code> vào <code>nums</code>. Khi đó <code>nums</code> trở thành <code>[4, 11, 10, 3]</code>.</li>
    <li>Trong thao tác thứ hai, ta xóa các phần tử 3 và 4, sau đó thêm <code>3 * 2 + 4</code> vào <code>nums</code>. Khi đó <code>nums</code> trở thành <code>[10, 11, 10]</code>.</li>
</ol>

<p>Lúc này, mọi phần tử của nums đều lớn hơn hoặc bằng 10, nên ta có thể dừng lại.&nbsp;</p>

<p>Có thể chứng minh rằng 2 là số thao tác nhỏ nhất cần thực hiện để mọi phần tử của mảng đều lớn hơn hoặc bằng 10.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,2,4,9], k = 20</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ol>
    <li>Sau một thao tác, <code>nums</code> trở thành <code>[2, 4, 9, 3]</code>.&nbsp;</li>
    <li>Sau hai thao tác, <code>nums</code> trở thành <code>[7, 4, 9]</code>.&nbsp;</li>
    <li>Sau ba thao tác, <code>nums</code> trở thành <code>[15, 9]</code>.&nbsp;</li>
    <li>Sau bốn thao tác, <code>nums</code> trở thành <code>[33]</code>.</li>
</ol>

<p>Lúc này, mọi phần tử của <code>nums</code> đều lớn hơn 20, nên ta có thể dừng lại.&nbsp;</p>

<p>Có thể chứng minh rằng 4 là số thao tác nhỏ nhất cần thực hiện để mọi phần tử của mảng đều lớn hơn hoặc bằng 20.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= nums.length &lt;= 2 * 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
    <li>Đầu vào được tạo sao cho luôn tồn tại đáp án. Nghĩa là, sau khi thực hiện một số thao tác, mọi phần tử của mảng đều lớn hơn hoặc bằng <code>k</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Priority Queue (Min Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước thay hai phần tử nhỏ nhất $x \le y$ bằng $2x+y$ cho đến khi phần tử nhỏ nhất lớn hơn hoặc bằng $k$. Vì $n \le 2 \times 10^5$, việc quét tuyến tính để tìm các phần tử nhỏ nhất là quá chậm.
>
> Thao tác luôn sử dụng hai phần tử nhỏ nhất hiện tại, nên ta dùng min-heap để duy trì chúng.
>
> Sau khi tạo heap, ta lấy ra hai phần tử, thêm $2x+y$, rồi dừng khi phần tử trên cùng lớn hơn hoặc bằng $k$.

<!-- thinking:end -->

Ta có thể sử dụng priority queue (min-heap) để mô phỏng quá trình này.

Cụ thể, trước tiên ta thêm các phần tử trong mảng vào priority queue `pq`. Sau đó, ta liên tục lấy ra hai phần tử nhỏ nhất `x` và `y` từ priority queue, rồi đưa `min(x, y) * 2 + max(x, y)` trở lại priority queue. Sau mỗi thao tác, ta tăng số thao tác lên một. Ta dừng khi số phần tử trong queue nhỏ hơn 2 hoặc phần tử nhỏ nhất trong queue lớn hơn hoặc bằng `k`.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], k: int) -> int:
        heapify(nums)
        ans = 0
        while len(nums) > 1 and nums[0] < k:
            x, y = heappop(nums), heappop(nums)
            heappush(nums, x * 2 + y)
            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums, int k) {
        PriorityQueue<Long> pq = new PriorityQueue<>();
        for (int x : nums) {
            pq.offer((long) x);
        }
        int ans = 0;
        for (; pq.size() > 1 && pq.peek() < k; ++ans) {
            long x = pq.poll(), y = pq.poll();
            pq.offer(x * 2 + y);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums, int k) {
        using ll = long long;
        priority_queue<ll, vector<ll>, greater<ll>> pq;
        for (int x : nums) {
            pq.push(x);
        }
        int ans = 0;
        for (; pq.size() > 1 && pq.top() < k; ++ans) {
            ll x = pq.top();
            pq.pop();
            ll y = pq.top();
            pq.pop();
            pq.push(x * 2 + y);
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int, k int) (ans int) {
    pq := &hp{nums}
    heap.Init(pq)
    for ; pq.Len() > 1 && pq.IntSlice[0] < k; ans++ {
        x, y := heap.Pop(pq).(int), heap.Pop(pq).(int)
        heap.Push(pq, x*2+y)
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
function minOperations(nums: number[], k: number): number {
    const pq = new MinPriorityQueue<number>();
    for (const x of nums) {
        pq.enqueue(x);
    }
    let ans = 0;
    for (; pq.size() > 1 && pq.front() < k; ++ans) {
        const x = pq.dequeue();
        const y = pq.dequeue();
        pq.enqueue(x * 2 + y);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::BinaryHeap;

impl Solution {
    pub fn min_operations(nums: Vec<i32>, k: i32) -> i32 {
        let mut pq = BinaryHeap::new();

        for &x in &nums {
            pq.push(-(x as i64));
        }

        let mut ans = 0;

        while pq.len() > 1 && -pq.peek().unwrap() < k as i64 {
            let x = -pq.pop().unwrap();
            let y = -pq.pop().unwrap();
            pq.push(-(x * 2 + y));
            ans += 1;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
