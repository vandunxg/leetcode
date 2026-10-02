---
comments: true
difficulty: Hard
rating: 2533
source: Weekly Contest 217 Q4
tags:
    - Greedy
    - Array
    - Ordered Set
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1675. Minimize Deviation in Array](https://leetcode.com/problems/minimize-deviation-in-array)

[中文文档](/solution/1600-1699/1675.Minimize%20Deviation%20in%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>nums</code> gồm <code>n</code> số nguyên dương.</p>

<p>Bạn có thể thực hiện hai loại thao tác trên bất kỳ phần tử nào của mảng với số lần tùy ý:</p>

<ul>
	<li>Nếu phần tử là số <strong>chẵn</strong>, <strong>chia</strong> nó cho <code>2</code>.

    <ul>
      <li>Ví dụ, nếu mảng là <code>[1,2,3,4]</code>, bạn có thể thực hiện thao tác này trên phần tử cuối, khi đó mảng trở thành <code>[1,2,3,<u>2</u>].</code></li>
    </ul>
    </li>
    <li>Nếu phần tử là số <strong>lẻ</strong>, <strong>nhân</strong> nó với <code>2</code>.
    <ul>
      <li>Ví dụ, nếu mảng là <code>[1,2,3,4]</code>, bạn có thể thực hiện thao tác này trên phần tử đầu, khi đó mảng trở thành <code>[<u>2</u>,2,3,4].</code></li>
    </ul>
    </li>

</ul>

<p><strong>Độ lệch</strong> của mảng là <strong>hiệu lớn nhất</strong> giữa hai phần tử bất kỳ trong mảng.</p>

<p>Hãy trả về <em><strong>độ lệch nhỏ nhất</strong> mà mảng có thể đạt được sau một số thao tác.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3,4]
<strong>Output:</strong> 1
<strong>Giải thích:</strong> Có thể biến đổi mảng thành [1,2,3,<u>2</u>], rồi thành [<u>2</u>,2,3,2], khi đó độ lệch là 3 - 2 = 1.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [4,1,5,20,3]
<strong>Output:</strong> 3
<strong>Giải thích:</strong> Sau hai thao tác, có thể biến đổi mảng thành [4,<u>2</u>,5,<u>5</u>,3], khi đó độ lệch là 5 - 2 = 3.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [2,10,8]
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>2 &lt;= n &lt;= 5 * 10<sup><span style="font-size: 10.8333px;">4</span></sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Priority Queue

<!-- thinking:start -->

> **Tư duy**
>
> Một giá trị lẻ có thể được nhân đôi một lần; một giá trị chẵn có thể được chia đôi nhiều lần. Ta muốn $\max-\min$ nhỏ nhất. Vì $n$ có thể bằng $5\times 10^4$, không thể liệt kê mọi giá trị có thể đạt được của từng phần tử.
>
> Trước hết nhân đôi mọi số lẻ để chỉ còn thao tác chia đôi. Max-heap lưu giá trị lớn nhất hiện tại và ta theo dõi giá trị nhỏ nhất toàn cục; liên tục chia đôi phần tử đầu heap rồi cập nhật độ lệch cho đến khi phần tử đầu là số lẻ và không thể giảm thêm.

<!-- thinking:end -->

Trực giác cho thấy để đạt độ lệch nhỏ nhất, ta cần giảm giá trị lớn nhất và tăng giá trị nhỏ nhất của mảng.

Vì có hai thao tác: nhân số lẻ với $2$ và chia số chẵn cho $2$, bài toán phức tạp hơn. Ta có thể nhân đôi tất cả số lẻ để biến chúng thành số chẵn, tương đương với việc chỉ còn một thao tác chia. Thao tác chia chỉ có thể làm giảm một giá trị, và giảm giá trị lớn nhất mới có thể giúp kết quả tốt hơn.

Vì vậy, ta dùng priority queue (max heap) để duy trì giá trị lớn nhất. Mỗi lần lấy phần tử đầu heap ra để chia tiếp cho $2$, đưa giá trị mới vào heap, rồi cập nhật giá trị nhỏ nhất và độ lệch nhỏ nhất giữa đầu heap với giá trị nhỏ nhất.

Khi phần tử đầu heap là số lẻ, ta dừng thao tác.

Độ phức tạp thời gian là $O(n\log n \times \log m)$, trong đó $n$ là độ dài mảng `nums` và $m$ là phần tử lớn nhất. Mỗi phần tử được chia cho $2$ nhiều nhất $O(\log m)$ lần, nên tổng số lần chia cho $2$ là $O(n\log m)$. Mỗi lần lấy và thêm phần tử vào heap tốn $O(\log n)$, do đó tổng độ phức tạp là $O(n\log n \times \log m)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumDeviation(self, nums: List[int]) -> int:
        h = []
        mi = inf
        for v in nums:
            if v & 1:
                v <<= 1
            h.append(-v)
            mi = min(mi, v)
        heapify(h)
        ans = -h[0] - mi
        while h[0] % 2 == 0:
            x = heappop(h) // 2
            heappush(h, x)
            mi = min(mi, -x)
            ans = min(ans, -h[0] - mi)
        return ans
```

#### Java

```java
class Solution {
    public int minimumDeviation(int[] nums) {
        PriorityQueue<Integer> q = new PriorityQueue<>((a, b) -> b - a);
        int mi = Integer.MAX_VALUE;
        for (int v : nums) {
            if (v % 2 == 1) {
                v <<= 1;
            }
            q.offer(v);
            mi = Math.min(mi, v);
        }
        int ans = q.peek() - mi;
        while (q.peek() % 2 == 0) {
            int x = q.poll() / 2;
            q.offer(x);
            mi = Math.min(mi, x);
            ans = Math.min(ans, q.peek() - mi);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumDeviation(vector<int>& nums) {
        int mi = INT_MAX;
        priority_queue<int> pq;
        for (int v : nums) {
            if (v & 1) v <<= 1;
            pq.push(v);
            mi = min(mi, v);
        }
        int ans = pq.top() - mi;
        while (pq.top() % 2 == 0) {
            int x = pq.top() >> 1;
            pq.pop();
            pq.push(x);
            mi = min(mi, x);
            ans = min(ans, pq.top() - mi);
        }
        return ans;
    }
};
```

#### Go

```go
func minimumDeviation(nums []int) int {
	q := hp{}
	mi := math.MaxInt32
	for _, v := range nums {
		if v%2 == 1 {
			v <<= 1
		}
		heap.Push(&q, v)
		mi = min(mi, v)
	}
	ans := q.IntSlice[0] - mi
	for q.IntSlice[0]%2 == 0 {
		x := heap.Pop(&q).(int) >> 1
		heap.Push(&q, x)
		mi = min(mi, x)
		ans = min(ans, q.IntSlice[0]-mi)
	}
	return ans
}

type hp struct{ sort.IntSlice }

func (h *hp) Push(v any) { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
func (h *hp) Less(i, j int) bool { return h.IntSlice[i] > h.IntSlice[j] }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
