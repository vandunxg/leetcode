---
comments: true
difficulty: Medium
rating: 1749
source: Biweekly Contest 113 Q2
tags:
    - Greedy
    - Array
    - Hash Table
    - Two Pointers
    - Binary Search
    - Counting
---

<!-- problem:start -->

# [2856. Minimum Array Length After Pair Removals](https://leetcode.com/problems/minimum-array-length-after-pair-removals)

[中文文档](/solution/2800-2899/2856.Minimum%20Array%20Length%20After%20Pair%20Removals/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>num</code> được sắp xếp theo thứ tự không giảm.</p>

<p>Bạn có thể thực hiện thao tác sau một số lần bất kỳ:</p>

<ul>
	<li>Chọn <strong>hai</strong> chỉ số <code>i</code> và <code>j</code>, sao cho <code>nums[i] &lt; nums[j]</code>.</li>
	<li>Sau đó, xóa các phần tử tại hai chỉ số <code>i</code> và <code>j</code> khỏi <code>nums</code>. Các phần tử còn lại vẫn giữ nguyên thứ tự ban đầu, và mảng được đánh lại chỉ số.</li>
</ul>

<p>Trả về độ dài <strong>nhỏ nhất</strong> của <code>nums</code> sau khi thực hiện thao tác trên không hoặc nhiều lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2856.Minimum%20Array%20Length%20After%20Pair%20Removals/images/tcase1.gif" style="width: 160px; height: 70px;" /></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,2,2,3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2856.Minimum%20Array%20Length%20After%20Pair%20Removals/images/tcase2.gif" style="width: 240px; height: 70px;" /></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1000000000,1000000000]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì hai số bằng nhau nên không thể xóa chúng.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,4,4,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2856.Minimum%20Array%20Length%20After%20Pair%20Removals/images/tcase3.gif" style="width: 210px; height: 70px;" /></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>nums</code> được sắp xếp theo thứ tự <strong>không giảm</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Priority Queue (Max Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác xóa hai giá trị khác nhau. Độ dài còn lại phụ thuộc vào giá trị xuất hiện nhiều nhất: nếu số lần xuất hiện của nó vượt quá một nửa độ dài mảng, nó sẽ còn lại sau khi ghép cặp tất cả các phần tử khác. Max-heap liên tục giảm hai tần suất lớn nhất để thực hiện việc ghép cặp này.

<!-- thinking:end -->

Ta dùng một hash table $cnt$ để đếm số lần xuất hiện của mỗi phần tử trong mảng $nums$, sau đó thêm từng giá trị trong $cnt$ vào một priority queue (max heap) $pq$. Mỗi lần, ta lấy hai phần tử $x$ và $y$ ra khỏi $pq$ rồi giảm giá trị của chúng đi một đơn vị. Nếu giá trị sau khi giảm vẫn lớn hơn $0$, ta thêm giá trị đó trở lại $pq$. Mỗi lần lấy hai phần tử ra khỏi $pq$ tương đương với việc xóa một cặp số khỏi mảng, nên độ dài mảng giảm đi $2$. Khi kích thước của $pq$ nhỏ hơn $2$, ta dừng thao tác xóa.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minLengthAfterRemovals(self, nums: List[int]) -> int:
        cnt = Counter(nums)
        pq = [-x for x in cnt.values()]
        heapify(pq)
        ans = len(nums)
        while len(pq) > 1:
            x, y = -heappop(pq), -heappop(pq)
            x -= 1
            y -= 1
            if x > 0:
                heappush(pq, -x)
            if y > 0:
                heappush(pq, -y)
            ans -= 2
        return ans
```

#### Java

```java
class Solution {
    public int minLengthAfterRemovals(List<Integer> nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            cnt.merge(x, 1, Integer::sum);
        }
        PriorityQueue<Integer> pq = new PriorityQueue<>(Comparator.reverseOrder());
        for (int x : cnt.values()) {
            pq.offer(x);
        }
        int ans = nums.size();
        while (pq.size() > 1) {
            int x = pq.poll();
            int y = pq.poll();
            x--;
            y--;
            if (x > 0) {
                pq.offer(x);
            }
            if (y > 0) {
                pq.offer(y);
            }
            ans -= 2;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minLengthAfterRemovals(vector<int>& nums) {
        unordered_map<int, int> cnt;
        for (int x : nums) {
            ++cnt[x];
        }
        priority_queue<int> pq;
        for (auto& [_, v] : cnt) {
            pq.push(v);
        }
        int ans = nums.size();
        while (pq.size() > 1) {
            int x = pq.top();
            pq.pop();
            int y = pq.top();
            pq.pop();
            x--;
            y--;
            if (x > 0) {
                pq.push(x);
            }
            if (y > 0) {
                pq.push(y);
            }
            ans -= 2;
        }
        return ans;
    }
};
```

#### Go

```go
func minLengthAfterRemovals(nums []int) int {
	cnt := map[int]int{}
	for _, x := range nums {
		cnt[x]++
	}
	h := &hp{}
	for _, x := range cnt {
		h.push(x)
	}
	ans := len(nums)
	for h.Len() > 1 {
		x, y := h.pop(), h.pop()
		if x > 1 {
			h.push(x - 1)
		}
		if y > 1 {
			h.push(y - 1)
		}
		ans -= 2
	}
	return ans
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] > h.IntSlice[j] }
func (h *hp) Push(v any)        { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
func (h *hp) push(v int) { heap.Push(h, v) }
func (h *hp) pop() int   { return heap.Pop(h).(int) }
```

#### TypeScript

```ts
function minLengthAfterRemovals(nums: number[]): number {
    const cnt: Map<number, number> = new Map();
    for (const x of nums) {
        cnt.set(x, (cnt.get(x) ?? 0) + 1);
    }
    const pq = new MaxPriorityQueue<number>();
    for (const [_, v] of cnt) {
        pq.enqueue(v);
    }
    let ans = nums.length;
    while (pq.size() > 1) {
        let x = pq.dequeue();
        let y = pq.dequeue();
        if (--x > 0) {
            pq.enqueue(x);
        }
        if (--y > 0) {
            pq.enqueue(y);
        }
        ans -= 2;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
