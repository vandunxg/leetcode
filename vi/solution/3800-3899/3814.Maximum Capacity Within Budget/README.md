---
comments: true
difficulty: Medium
rating: 1796
source: Weekly Contest 485 Q2
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [3814. Maximum Capacity Within Budget](https://leetcode.com/problems/maximum-capacity-within-budget)

[中文文档](/solution/3800-3899/3814.Maximum%20Capacity%20Within%20Budget/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>costs</code> và <code>capacity</code>, có cùng độ dài <code>n</code>, trong đó <code>costs[i]</code> là chi phí mua máy thứ <code>i<sup>th</sup></code> và <code>capacity[i]</code> là công suất của máy đó.</p>

<p>Bạn cũng được cho một số nguyên <code>budget</code>.</p>

<p>Bạn có thể chọn <strong>tối đa hai máy phân biệt</strong> sao cho <strong>tổng chi phí</strong> của các máy được chọn <strong>nhỏ hơn nghiêm ngặt</strong> <code>budget</code>.</p>

<p>Trả về <strong>tổng công suất lớn nhất</strong> có thể đạt được của các máy được chọn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">costs = [4,8,5,3], capacity = [1,5,2,7], budget = 8</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn hai máy có <code>costs[0] = 4</code> và <code>costs[3] = 3</code>.</li>
	<li>Tổng chi phí là <code>4 + 3 = 7</code>, nhỏ hơn nghiêm ngặt <code>budget = 8</code>.</li>
	<li>Tổng công suất lớn nhất là <code>capacity[0] + capacity[3] = 1 + 7 = 8</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">costs = [3,5,7,4], capacity = [2,4,3,6], budget = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn một máy có <code>costs[3] = 4</code>.</li>
	<li>Tổng chi phí là 4, nhỏ hơn nghiêm ngặt <code>budget = 7</code>.</li>
	<li>Tổng công suất lớn nhất là <code>capacity[3] = 6</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">costs = [2,2,2], capacity = [3,5,4], budget = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn hai máy có <code>costs[1] = 2</code> và <code>costs[2] = 2</code>.</li>
	<li>Tổng chi phí là <code>2 + 2 = 4</code>, nhỏ hơn nghiêm ngặt <code>budget = 5</code>.</li>
	<li>Tổng công suất lớn nhất là <code>capacity[1] + capacity[2] = 5 + 4 = 9</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == costs.length == capacity.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= costs[i], capacity[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= budget &lt;= 2 * 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Chọn tối đa hai máy có tổng chi phí nhỏ hơn nghiêm ngặt $\textit{budget}$ và tối đa hóa công suất. Với $n \le 10^5$, không thể duyệt tất cả các cặp.
>
> Một máy có chi phí riêng đã lớn hơn hoặc bằng ngân sách thì không thể được mua; ta loại máy đó. Công suất lớn nhất của một máy là giá trị lớn nhất còn lại.
>
> Với hai máy, sắp xếp theo chi phí. Với mỗi máy rẻ hơn $i$, các máy có thể ghép với nó là những máy có chi phí nhỏ hơn $\textit{budget}-\textit{cost}_i$. Cận phải này di chuyển sang trái khi $i$ tăng.
>
> Một ordered set lưu các công suất ở phía phải còn hợp lệ, sau khi loại chính máy $i$, sẽ cho máy ghép tốt nhất. Thu hẹp con trỏ phải giúp các thao tác cập nhật có độ phức tạp logarithm.

<!-- thinking:end -->

Trước hết, ta lọc các máy có chi phí nhỏ hơn ngân sách và sắp xếp chúng theo thứ tự tăng dần của chi phí, lưu vào mảng $\textit{arr}$, trong đó $\textit{arr}[i] = (\textit{costs}[i], \textit{capacity}[i])$. Nếu $\textit{arr}$ rỗng, ta không thể mua máy nào, nên trả về $0$.

Nếu không, ta tìm máy có công suất lớn nhất trong $\textit{arr}$ và khởi tạo đáp án bằng công suất này.

Tiếp theo, ta dùng phương pháp hai con trỏ để duyệt các cặp máy trong $\textit{arr}$, đồng thời dùng ordered set $\textit{remain}$ để duy trì công suất của tất cả các máy hiện đang có thể chọn. Ban đầu, $\textit{remain}$ chứa công suất của tất cả máy trong $\textit{arr}$.

Ta dùng các con trỏ $i$ và $j$ lần lượt trỏ đến đầu và cuối của $\textit{arr}$. Với mỗi $i$, ta xóa $\textit{arr}[i]$ khỏi $\textit{remain}$, sau đó di chuyển con trỏ $j$ cho đến khi $\textit{arr}[i].\textit{cost} + \textit{arr}[j].\textit{cost} < \textit{budget}$. Trong quá trình này, ta xóa khỏi $\textit{remain}$ các máy không thỏa điều kiện. Khi đó, mọi máy trong $\textit{remain}$ đều có thể được mua cùng với $\textit{arr}[i]$. Ta chọn máy có công suất lớn nhất trong $\textit{remain}$, cộng công suất của nó với công suất của $\textit{arr}[i]$, rồi cập nhật đáp án. Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng máy.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxCapacity(self, costs: List[int], capacity: List[int], budget: int) -> int:
        arr = []
        for a, b in zip(costs, capacity):
            if a < budget:
                arr.append((a, b))
        if not arr:
            return 0
        arr.sort()
        remain = SortedList()
        for i, (_, b) in enumerate(arr):
            remain.add((b, i))
        i, j = 0, len(arr) - 1
        ans = remain[-1][0]
        while i < j:
            remain.discard((arr[i][1], i))
            while i < j and arr[i][0] + arr[j][0] >= budget:
                remain.discard((arr[j][1], j))
                j -= 1
            if remain:
                ans = max(ans, arr[i][1] + remain[-1][0])
            i += 1
        return ans
```

#### Java

```java
class Solution {
    public int maxCapacity(int[] costs, int[] capacity, int budget) {
        List<int[]> arr = new ArrayList<>();
        for (int k = 0; k < costs.length; k++) {
            int a = costs[k], b = capacity[k];
            if (a < budget) {
                arr.add(new int[] {a, b});
            }
        }
        if (arr.isEmpty()) {
            return 0;
        }
        arr.sort(Comparator.comparingInt(o -> o[0]));
        TreeSet<int[]> remain = new TreeSet<>((x, y) -> {
            if (x[0] != y[0]) {
                return x[0] - y[0];
            }
            return x[1] - y[1];
        });
        for (int i = 0; i < arr.size(); i++) {
            remain.add(new int[] {arr.get(i)[1], i});
        }
        int i = 0, j = arr.size() - 1;
        int ans = remain.last()[0];
        while (i < j) {
            remain.remove(new int[] {arr.get(i)[1], i});
            while (i < j && arr.get(i)[0] + arr.get(j)[0] >= budget) {
                remain.remove(new int[] {arr.get(j)[1], j});
                j--;
            }
            if (!remain.isEmpty()) {
                ans = Math.max(ans, arr.get(i)[1] + remain.last()[0]);
            }
            i++;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxCapacity(vector<int>& costs, vector<int>& capacity, int budget) {
        vector<pair<int, int>> arr;
        for (int k = 0; k < costs.size(); k++) {
            int a = costs[k], b = capacity[k];
            if (a < budget) {
                arr.emplace_back(a, b);
            }
        }
        if (arr.empty()) {
            return 0;
        }
        sort(arr.begin(), arr.end());
        multiset<pair<int, int>> remain;
        for (int i = 0; i < arr.size(); i++) {
            remain.insert({arr[i].second, i});
        }
        int i = 0, j = arr.size() - 1;
        int ans = prev(remain.end())->first;
        while (i < j) {
            remain.erase(remain.find({arr[i].second, i}));
            while (i < j && arr[i].first + arr[j].first >= budget) {
                remain.erase(remain.find({arr[j].second, j}));
                j--;
            }
            if (!remain.empty()) {
                ans = max(ans, arr[i].second + prev(remain.end())->first);
            }
            i++;
        }
        return ans;
    }
};
```

#### Go

```go
type Node struct {
	b int
	i int
}

type MaxHeap []Node

func (h MaxHeap) Len() int { return len(h) }
func (h MaxHeap) Less(i, j int) bool {
	if h[i].b != h[j].b {
		return h[i].b > h[j].b
	}
	return h[i].i > h[j].i
}
func (h MaxHeap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *MaxHeap) Push(x interface{}) {
	*h = append(*h, x.(Node))
}
func (h *MaxHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[:n-1]
	return x
}

func maxCapacity(costs []int, capacity []int, budget int) int {
	arr := make([][2]int, 0)
	for k := 0; k < len(costs); k++ {
		a, b := costs[k], capacity[k]
		if a < budget {
			arr = append(arr, [2]int{a, b})
		}
	}
	if len(arr) == 0 {
		return 0
	}
	sort.Slice(arr, func(i, j int) bool {
		if arr[i][0] != arr[j][0] {
			return arr[i][0] < arr[j][0]
		}
		return arr[i][1] < arr[j][1]
	})
	alive := make([]bool, len(arr))
	h := &MaxHeap{}
	for i := 0; i < len(arr); i++ {
		alive[i] = true
		heap.Push(h, Node{arr[i][1], i})
	}
	i, j := 0, len(arr)-1
	for h.Len() > 0 && !alive[(*h)[0].i] {
		heap.Pop(h)
	}
	ans := (*h)[0].b
	for i < j {
		alive[i] = false
		for i < j && arr[i][0]+arr[j][0] >= budget {
			alive[j] = false
			j--
		}
		for h.Len() > 0 && !alive[(*h)[0].i] {
			heap.Pop(h)
		}
		if h.Len() > 0 {
			if arr[i][1]+(*h)[0].b > ans {
				ans = arr[i][1] + (*h)[0].b
			}
		}
		i++
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
