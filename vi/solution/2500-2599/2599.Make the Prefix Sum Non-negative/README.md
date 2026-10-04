---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2599. Make the Prefix Sum Non-negative 🔒](https://leetcode.com/problems/make-the-prefix-sum-non-negative)

[中文文档](/solution/2500-2599/2599.Make%20the%20Prefix%20Sum%20Non-negative/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>. Bạn có thể thực hiện thao tác sau bao nhiêu lần tùy ý:</p>

<ul>
	<li>Chọn một phần tử bất kỳ trong <code>nums</code> và đưa nó vào cuối <code>nums</code>.</li>
</ul>

<p>Mảng tổng tiền tố của <code>nums</code> là một mảng <code>prefix</code> có cùng độ dài với <code>nums</code>, trong đó <code>prefix[i]</code> là tổng của tất cả các số nguyên <code>nums[j]</code> với <code>j</code> nằm trong đoạn đóng <code>[0, i]</code>.</p>

<p>Trả về <em>số thao tác ít nhất để mảng tổng tiền tố không chứa số nguyên âm</em>. Các test case được tạo sao cho luôn có thể làm cho mảng tổng tiền tố không âm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,-5,4]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> không cần thực hiện thao tác nào.
Mảng là [2,3,-5,4]. Mảng tổng tiền tố là [2, 5, 0, 4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,-5,-2,6]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> ta có thể thực hiện một thao tác trên chỉ số 1.
Mảng sau thao tác là [3,-2,6,-5]. Mảng tổng tiền tố là [3, 1, 7, 2].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Priority Queue (Min Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Một phần tử có thể được chuyển xuống cuối; mọi tổng tiền tố phải luôn không âm, và ta muốn số lần di chuyển ít nhất. Không thể liệt kê tất cả các cách di chuyển.
>
> Chỉ các số âm mới có thể kéo một tổng tiền tố xuống dưới 0. Khi tổng đang xét trở thành số âm, ta loại số âm nhỏ nhất đã gặp cho đến lúc đó — cách này khôi phục tổng với ảnh hưởng ít nhất. Một min-heap lưu các số âm đó; mỗi lần lấy một phần tử ra khỏi heap, ta trừ nó khỏi tổng và tăng số lần di chuyển.

<!-- thinking:end -->

Ta dùng biến $s$ để ghi nhận tổng tiền tố của mảng hiện tại.

Duyệt mảng $nums$, cộng phần tử hiện tại $x$ vào tổng tiền tố $s$. Nếu $x$ là số âm, thêm $x$ vào min heap. Nếu lúc này $s$ là số âm, tham lam lấy số âm nhỏ nhất ra khỏi heap và trừ nó khỏi $s$, đồng thời tăng đáp án lên một. Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makePrefSumNonNegative(self, nums: List[int]) -> int:
        h = []
        ans = s = 0
        for x in nums:
            s += x
            if x < 0:
                heappush(h, x)
            while s < 0:
                s -= heappop(h)
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int makePrefSumNonNegative(int[] nums) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        int ans = 0;
        long s = 0;
        for (int x : nums) {
            s += x;
            if (x < 0) {
                pq.offer(x);
            }
            while (s < 0) {
                s -= pq.poll();
                ++ans;
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
    int makePrefSumNonNegative(vector<int>& nums) {
        priority_queue<int, vector<int>, greater<int>> pq;
        int ans = 0;
        long long s = 0;
        for (int& x : nums) {
            s += x;
            if (x < 0) {
                pq.push(x);
            }
            while (s < 0) {
                s -= pq.top();
                pq.pop();
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func makePrefSumNonNegative(nums []int) (ans int) {
	pq := hp{}
	s := 0
	for _, x := range nums {
		s += x
		if x < 0 {
			heap.Push(&pq, x)
		}
		for s < 0 {
			s -= heap.Pop(&pq).(int)
			ans++
		}
	}
	return ans
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] < h.IntSlice[j] }
func (h *hp) Push(v any)        { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
```

#### TypeScript

```ts
function makePrefSumNonNegative(nums: number[]): number {
    const pq = new MinPriorityQueue<number>();
    let ans = 0;
    let s = 0;
    for (const x of nums) {
        s += x;
        if (x < 0) {
            pq.enqueue(x);
        }
        while (s < 0) {
            s -= pq.dequeue();
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
