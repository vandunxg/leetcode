---
comments: true
difficulty: Medium
rating: 1793
source: Weekly Contest 390 Q3
tags:
    - Array
    - Hash Table
    - Ordered Set
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3092. Most Frequent IDs](https://leetcode.com/problems/most-frequent-ids)

[Tài liệu tiếng Trung](/solution/3000-3099/3092.Most%20Frequent%20IDs/README.md)

## Mô tả

<!-- description:start -->

<p>Bài toán yêu cầu theo dõi tần suất của các ID trong một collection thay đổi theo thời gian. Cho hai mảng số nguyên <code>nums</code> và <code>freq</code> có cùng độ dài <code>n</code>. Mỗi phần tử trong <code>nums</code> biểu diễn một ID, còn phần tử tương ứng trong <code>freq</code> cho biết cần thêm hoặc xóa bao nhiêu lần ID đó khỏi collection ở mỗi bước.</p>

<ul>
	<li><strong>Thêm ID:</strong> Nếu <code>freq[i]</code> là số dương, nghĩa là thêm <code>freq[i]</code> ID có giá trị <code>nums[i]</code> vào collection ở bước <code>i</code>.</li>
	<li><strong>Xóa ID:</strong> Nếu <code>freq[i]</code> là số âm, nghĩa là xóa <code>-freq[i]</code> ID có giá trị <code>nums[i]</code> khỏi collection ở bước <code>i</code>.</li>
</ul>

<p>Trả về một mảng <code>ans</code> có độ dài <code>n</code>, trong đó <code>ans[i]</code> biểu diễn <strong>số lượng</strong> của <em>ID xuất hiện nhiều nhất</em> trong collection sau bước thứ <code>i<sup>th</sup></code>. Nếu collection rỗng ở một bước nào đó, <code>ans[i]</code> phải bằng 0 tại bước đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,2,1], freq = [3,2,-3,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,3,2,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau bước 0, ta có 3 ID với giá trị 2. Do đó, <code>ans[0] = 3</code>.<br />
Sau bước 1, ta có 3 ID với giá trị 2 và 2 ID với giá trị 3. Do đó, <code>ans[1] = 3</code>.<br />
Sau bước 2, ta có 2 ID với giá trị 3. Do đó, <code>ans[2] = 2</code>.<br />
Sau bước 3, ta có 2 ID với giá trị 3 và 1 ID với giá trị 1. Do đó, <code>ans[3] = 2</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,5,3], freq = [2,-2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,0,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau bước 0, ta có 2 ID với giá trị 5. Do đó, <code>ans[0] = 2</code>.<br />
Sau bước 1, collection không còn ID nào. Do đó, <code>ans[1] = 0</code>.<br />
Sau bước 2, ta có 1 ID với giá trị 3. Do đó, <code>ans[2] = 1</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length == freq.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= freq[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>freq[i] != 0</code></li>
	<li>Dữ liệu đầu vào được tạo<!-- notionvc: a136b55a-f319-4fa6-9247-11be9f3b1db8 --> sao cho số lần xuất hiện của một ID không bao giờ âm ở bất kỳ bước nào.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Priority Queue (Max Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Tần suất của các ID thay đổi trực tuyến và ta cần báo cáo giá trị lớn nhất hiện tại sau mỗi lần cập nhật. Vì $n \le 10^5$, việc duyệt toàn bộ mỗi lần là quá chậm.
>
> Số lượng có thể tăng hoặc giảm, nên heap không thể chỉnh sửa các phần tử cũ. Các số lượng đã lỗi thời được đưa vào một map để xóa trì hoãn và chỉ bị loại bỏ khi chúng nằm ở đỉnh heap.
>
> Một hash map lưu tần suất hiện tại, một hash map khác lưu số lần một số lượng đã bị loại bỏ; ta đẩy số lượng mới vào max-heap rồi dọn dẹp đỉnh heap.

<!-- thinking:end -->

Ta sử dụng một hash table $cnt$ để ghi lại số lần xuất hiện của mỗi ID, một hash table $lazy$ để ghi lại số lần mỗi số lượng cần bị xóa, và một priority queue $pq$ để duy trì số lần xuất hiện lớn nhất.

Với mỗi thao tác $(x, f)$, ta cần cập nhật số lần xuất hiện $cnt[x]$ của $x$, nghĩa là giá trị của $cnt[x]$ trong $lazy$ cần tăng thêm $1$, cho biết số lần cần xóa số lượng này tăng thêm $1$. Sau đó, ta cập nhật giá trị của $cnt[x]$ bằng cách cộng $f$ vào $cnt[x]$. Tiếp theo, ta thêm giá trị mới của $cnt[x]$ vào priority queue $pq$. Sau đó, ta kiểm tra phần tử trên cùng của priority queue $pq$. Nếu số lần cần xóa số lượng tương ứng trong $lazy$ lớn hơn $0$, ta lấy phần tử trên cùng ra. Cuối cùng, ta kiểm tra priority queue có rỗng hay không. Nếu không rỗng, phần tử trên cùng là số lần xuất hiện lớn nhất và ta thêm nó vào mảng kết quả.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mostFrequentIDs(self, nums: List[int], freq: List[int]) -> List[int]:
        cnt = Counter()
        lazy = Counter()
        ans = []
        pq = []
        for x, f in zip(nums, freq):
            lazy[cnt[x]] += 1
            cnt[x] += f
            heappush(pq, -cnt[x])
            while pq and lazy[-pq[0]] > 0:
                lazy[-pq[0]] -= 1
                heappop(pq)
            ans.append(0 if not pq else -pq[0])
        return ans
```

#### Java

```java
class Solution {
    public long[] mostFrequentIDs(int[] nums, int[] freq) {
        Map<Integer, Long> cnt = new HashMap<>();
        Map<Long, Integer> lazy = new HashMap<>();
        int n = nums.length;
        long[] ans = new long[n];
        PriorityQueue<Long> pq = new PriorityQueue<>(Collections.reverseOrder());
        for (int i = 0; i < n; ++i) {
            int x = nums[i], f = freq[i];
            lazy.merge(cnt.getOrDefault(x, 0L), 1, Integer::sum);
            cnt.merge(x, (long) f, Long::sum);
            pq.add(cnt.get(x));
            while (!pq.isEmpty() && lazy.getOrDefault(pq.peek(), 0) > 0) {
                lazy.merge(pq.poll(), -1, Integer::sum);
            }
            ans[i] = pq.isEmpty() ? 0 : pq.peek();
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> mostFrequentIDs(vector<int>& nums, vector<int>& freq) {
        unordered_map<int, long long> cnt;
        unordered_map<long long, int> lazy;
        int n = nums.size();
        vector<long long> ans(n);
        priority_queue<long long> pq;

        for (int i = 0; i < n; ++i) {
            int x = nums[i], f = freq[i];
            lazy[cnt[x]]++;
            cnt[x] += f;
            pq.push(cnt[x]);
            while (!pq.empty() && lazy[pq.top()] > 0) {
                lazy[pq.top()]--;
                pq.pop();
            }
            ans[i] = pq.empty() ? 0 : pq.top();
        }

        return ans;
    }
};
```

#### Go

```go
func mostFrequentIDs(nums []int, freq []int) []int64 {
	n := len(nums)
	cnt := map[int]int{}
	lazy := map[int]int{}
	ans := make([]int64, n)
	pq := hp{}
	heap.Init(&pq)
	for i, x := range nums {
		f := freq[i]
		lazy[cnt[x]]++
		cnt[x] += f
		heap.Push(&pq, cnt[x])
		for pq.Len() > 0 && lazy[pq.IntSlice[0]] > 0 {
			lazy[pq.IntSlice[0]]--
			heap.Pop(&pq)
		}
		if pq.Len() > 0 {
			ans[i] = int64(pq.IntSlice[0])
		}
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
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
