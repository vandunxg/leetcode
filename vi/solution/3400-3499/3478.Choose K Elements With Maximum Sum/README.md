---
comments: true
difficulty: Medium
rating: 1753
source: Weekly Contest 440 Q2
tags:
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3478. Choose K Elements With Maximum Sum](https://leetcode.com/problems/choose-k-elements-with-maximum-sum)

[中文文档](/solution/3400-3499/3478.Choose%20K%20Elements%20With%20Maximum%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code> có cùng độ dài <code>n</code>, cùng với một số nguyên dương <code>k</code>.</p>

<p>Với mỗi chỉ số <code>i</code> từ <code>0</code> đến <code>n - 1</code>, thực hiện các bước sau:</p>

<ul>
	<li>Tìm <strong>tất cả</strong> các chỉ số <code>j</code> sao cho <code>nums1[j]</code> nhỏ hơn <code>nums1[i]</code>.</li>
	<li>Chọn <strong>tối đa</strong> <code>k</code> giá trị <code>nums2[j]</code> tại các chỉ số đó để <strong>tối đa hóa</strong> tổng.</li>
</ul>

<p>Trả về mảng <code>answer</code> có kích thước <code>n</code>, trong đó <code>answer[i]</code> là kết quả tương ứng với chỉ số <code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [4,2,1,5,3], nums2 = [10,20,30,40,50], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[80,30,0,80,50]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>i = 0</code>: Chọn 2 giá trị lớn nhất từ <code>nums2</code> tại các chỉ số <code>[1, 2, 4]</code> sao cho <code>nums1[j] &lt; nums1[0]</code>, được kết quả <code>50 + 30 = 80</code>.</li>
	<li>Với <code>i = 1</code>: Chọn 2 giá trị lớn nhất từ <code>nums2</code> tại chỉ số <code>[2]</code> sao cho <code>nums1[j] &lt; nums1[1]</code>, được kết quả 30.</li>
	<li>Với <code>i = 2</code>: Không có chỉ số nào thỏa mãn <code>nums1[j] &lt; nums1[2]</code>, nên kết quả là 0.</li>
	<li>Với <code>i = 3</code>: Chọn 2 giá trị lớn nhất từ <code>nums2</code> tại các chỉ số <code>[0, 1, 2, 4]</code> sao cho <code>nums1[j] &lt; nums1[3]</code>, được kết quả <code>50 + 30 = 80</code>.</li>
	<li>Với <code>i = 4</code>: Chọn 2 giá trị lớn nhất từ <code>nums2</code> tại các chỉ số <code>[1, 2]</code> sao cho <code>nums1[j] &lt; nums1[4]</code>, được kết quả <code>30 + 20 = 50</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [2,2,2,2], nums2 = [3,1,2,3], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,0,0,0]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì tất cả các phần tử trong <code>nums1</code> đều bằng nhau, không có chỉ số nào thỏa mãn điều kiện <code>nums1[j] &lt; nums1[i]</code> với mọi <code>i</code>, nên tất cả vị trí đều có kết quả là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length == nums2.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Priority Queue (Min-Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi $i$, ta chọn tối đa $k$ giá trị $\textit{nums2}[j]$ trong các chỉ số thỏa mãn $\textit{nums1}[j]<\textit{nums1}[i]$. Vì $n\le 10^5$, không thể lọc lại từ đầu cho từng $i$.
>
> Sau khi sắp xếp theo $\textit{nums1}$, tập các chỉ số $j$ hợp lệ chỉ tăng dần, nên ta có thể dùng một min-heap có kích thước $k$ để duy trì tổng của $k$ giá trị lớn nhất hiện tại.
>
> Con trỏ $j$ đưa các giá trị $\textit{nums2}$ có khóa $\textit{nums1}$ nhỏ hơn vào heap; khi số phần tử vượt quá k, ta loại phần tử nhỏ nhất khỏi heap. Tổng các phần tử trong heap chính là đáp án tại $i$.

<!-- thinking:end -->

Ta có thể chuyển mảng $\textit{nums1}$ thành mảng $\textit{arr}$, trong đó mỗi phần tử là một tuple $(x, i)$, biểu diễn giá trị $x$ tại chỉ số $i$ trong $\textit{nums1}$. Sau đó, ta sắp xếp mảng $\textit{arr}$ theo thứ tự tăng dần của $x$.

Ta sử dụng một min-heap $\textit{pq}$ để duy trì các phần tử từ mảng $\textit{nums2}$. Ban đầu, $\textit{pq}$ rỗng. Ta dùng biến $\textit{s}$ để ghi nhận tổng các phần tử trong $\textit{pq}$. Ngoài ra, ta dùng một con trỏ $j$ để duy trì vị trí hiện tại trong mảng $\textit{arr}$ cần được thêm vào $\textit{pq}$.

Ta duyệt mảng $\textit{arr}$. Với phần tử thứ $h$, $(x, i)$, ta thêm vào $\textit{pq}$ tất cả các phần tử $\textit{nums2}[\textit{arr}[j][1]]$ thỏa mãn $j < h$ và $\textit{arr}[j][0] < x$, đồng thời cộng các phần tử này vào $\textit{s}$. Nếu kích thước của $\textit{pq}$ vượt quá $k$, ta lấy phần tử nhỏ nhất khỏi $\textit{pq}$ và trừ nó khỏi $\textit{s}$. Sau đó, ta cập nhật giá trị của $\textit{ans}[i]$ thành $\textit{s}$.

Sau khi duyệt xong, ta trả về mảng đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n \log n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaxSum(self, nums1: List[int], nums2: List[int], k: int) -> List[int]:
        arr = [(x, i) for i, x in enumerate(nums1)]
        arr.sort()
        pq = []
        s = j = 0
        n = len(arr)
        ans = [0] * n
        for h, (x, i) in enumerate(arr):
            while j < h and arr[j][0] < x:
                y = nums2[arr[j][1]]
                heappush(pq, y)
                s += y
                if len(pq) > k:
                    s -= heappop(pq)
                j += 1
            ans[i] = s
        return ans
```

#### Java

```java
class Solution {
    public long[] findMaxSum(int[] nums1, int[] nums2, int k) {
        int n = nums1.length;
        int[][] arr = new int[n][0];
        for (int i = 0; i < n; ++i) {
            arr[i] = new int[] {nums1[i], i};
        }
        Arrays.sort(arr, (a, b) -> a[0] - b[0]);
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        long s = 0;
        long[] ans = new long[n];
        int j = 0;
        for (int h = 0; h < n; ++h) {
            int x = arr[h][0], i = arr[h][1];
            while (j < h && arr[j][0] < x) {
                int y = nums2[arr[j][1]];
                pq.offer(y);
                s += y;
                if (pq.size() > k) {
                    s -= pq.poll();
                }
                ++j;
            }
            ans[i] = s;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> findMaxSum(vector<int>& nums1, vector<int>& nums2, int k) {
        int n = nums1.size();
        vector<pair<int, int>> arr(n);
        for (int i = 0; i < n; ++i) {
            arr[i] = {nums1[i], i};
        }
        ranges::sort(arr);
        priority_queue<int, vector<int>, greater<int>> pq;
        long long s = 0;
        int j = 0;
        vector<long long> ans(n);
        for (int h = 0; h < n; ++h) {
            auto [x, i] = arr[h];
            while (j < h && arr[j].first < x) {
                int y = nums2[arr[j].second];
                pq.push(y);
                s += y;
                if (pq.size() > k) {
                    s -= pq.top();
                    pq.pop();
                }
                ++j;
            }
            ans[i] = s;
        }
        return ans;
    }
};
```

#### Go

```go
func findMaxSum(nums1 []int, nums2 []int, k int) []int64 {
	n := len(nums1)
	arr := make([][2]int, n)
	for i, x := range nums1 {
		arr[i] = [2]int{x, i}
	}
	ans := make([]int64, n)
	sort.Slice(arr, func(i, j int) bool { return arr[i][0] < arr[j][0] })
	pq := hp{}
	var s int64
	j := 0
	for h, e := range arr {
		x, i := e[0], e[1]
		for j < h && arr[j][0] < x {
			y := nums2[arr[j][1]]
			heap.Push(&pq, y)
			s += int64(y)
			if pq.Len() > k {
				s -= int64(heap.Pop(&pq).(int))
			}
			j++
		}
		ans[i] = s
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
function findMaxSum(nums1: number[], nums2: number[], k: number): number[] {
    const n = nums1.length;
    const arr = nums1.map((x, i) => [x, i]).sort((a, b) => a[0] - b[0]);
    const pq = new MinPriorityQueue<number>();
    let [s, j] = [0, 0];
    const ans: number[] = Array(k).fill(0);
    for (let h = 0; h < n; ++h) {
        const [x, i] = arr[h];
        while (j < h && arr[j][0] < x) {
            const y = nums2[arr[j++][1]];
            pq.enqueue(y);
            s += y;
            if (pq.size() > k) {
                s -= pq.dequeue();
            }
        }
        ans[i] = s;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
