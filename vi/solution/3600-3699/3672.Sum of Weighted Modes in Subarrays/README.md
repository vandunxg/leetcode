---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Counting
    - Ordered Set
    - Sliding Window
---

<!-- problem:start -->

# [3672. Sum of Weighted Modes in Subarrays 🔒](https://leetcode.com/problems/sum-of-weighted-modes-in-subarrays)

[中文文档](/solution/3600-3699/3672.Sum%20of%20Weighted%20Modes%20in%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Với mỗi <strong>mảng con</strong> có độ dài <code>k</code>:</p>

<ul>
	<li><strong>Mode</strong> được định nghĩa là phần tử có <strong>tần suất xuất hiện cao nhất</strong>. Nếu có nhiều phần tử cùng là mode, chọn phần tử <strong>nhỏ nhất</strong>.</li>
	<li><strong>Trọng số</strong> được định nghĩa là <code>mode * frequency(mode)</code>.</li>
</ul>

<p>Hãy trả về <strong>tổng</strong> trọng số của tất cả <strong>mảng con</strong> có độ dài <code>k</code>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li><strong>Mảng con</strong> là một dãy phần tử liên tiếp <strong>không rỗng</strong> trong một mảng.</li>
	<li><strong>Tần suất</strong> của một phần tử <code>x</code> là số lần phần tử đó xuất hiện trong mảng.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2,3], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con có độ dài <code>k = 3</code> là:</p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Mảng con</th>
			<th style="border: 1px solid black;">Tần suất</th>
			<th style="border: 1px solid black;">Mode</th>
			<th style="border: 1px solid black;">Mode<br />
			​​​​​​​Tần suất</th>
			<th style="border: 1px solid black;">Trọng số</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">[1, 2, 2]</td>
			<td style="border: 1px solid black;">1: 1, 2: 2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2 &times; 2 = 4</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">[2, 2, 3]</td>
			<td style="border: 1px solid black;">2: 2, 3: 1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2 &times; 2 = 4</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, tổng trọng số là <code>4 + 4 = 8</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,2], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con có độ dài <code>k = 2</code> là:</p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Mảng con</th>
			<th style="border: 1px solid black;">Tần suất</th>
			<th style="border: 1px solid black;">Mode</th>
			<th style="border: 1px solid black;">Mode<br />
			Tần suất</th>
			<th style="border: 1px solid black;">Trọng số</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">[1, 2]</td>
			<td style="border: 1px solid black;">1: 1, 2: 1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1 &times; 1 = 1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">[2, 1]</td>
			<td style="border: 1px solid black;">2: 1, 1: 1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1 &times; 1 = 1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">[1, 2]</td>
			<td style="border: 1px solid black;">1: 1, 2: 1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1 &times; 1 = 1</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, tổng trọng số là <code>1 + 1 + 1 = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,3,4,3], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con có độ dài <code>k = 3</code> là:</p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Mảng con</th>
			<th style="border: 1px solid black;">Tần suất</th>
			<th style="border: 1px solid black;">Mode</th>
			<th style="border: 1px solid black;">Mode<br />
			Tần suất</th>
			<th style="border: 1px solid black;">Trọng số</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">[4, 3, 4]</td>
			<td style="border: 1px solid black;">4: 2, 3: 1</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2 &times; 4 = 8</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">[3, 4, 3]</td>
			<td style="border: 1px solid black;">3: 2, 4: 1</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2 &times; 3 = 6</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, tổng trọng số là <code>8 + 6 = 14</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Map + Priority Queue + Sliding Window + Lazy Deletion

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi cửa sổ có độ dài $k$ đóng góp $\textit{mode}\times\textit{freq}$, trong đó nếu hòa thì ưu tiên mode nhỏ hơn. Nếu quét lại từng cửa sổ, độ phức tạp sẽ là $O(nk)$.
>
> Một frequency map kết hợp với heap có khóa $(-\textit{freq},\textit{val})$ sẽ cho ta mode. Lazy deletion loại bỏ phần tử ở đỉnh heap khi tần suất của nó không còn khớp với map.
>
> Khi trượt cửa sổ, ta push cả phần tử đi vào và phần tử đi ra. $\textit{get\_mode}$ trả về tích khi phần tử ở đỉnh đã nhất quán. Mỗi chỉ số chỉ gây ra một số thao tác heap không đổi.

<!-- thinking:end -->

Ta dùng một hash map $\textit{cnt}$ để ghi nhận tần suất của mỗi số trong cửa sổ hiện tại. Ta dùng một priority queue $\textit{pq}$ để lưu tần suất và giá trị của mỗi số trong cửa sổ hiện tại, ưu tiên tần suất cao hơn, và nếu tần suất bằng nhau thì ưu tiên số nhỏ hơn.

Ta thiết kế hàm $\textit{get_mode()}$ để lấy mode và tần suất của nó trong cửa sổ hiện tại. Cụ thể, ta liên tục lấy phần tử ở đỉnh priority queue ra cho đến khi tần suất của nó khớp với tần suất được ghi trong hash map; khi đó, phần tử ở đỉnh chính là mode và tần suất của nó trong cửa sổ hiện tại.

Ta dùng biến $\textit{ans}$ để ghi nhận tổng trọng số của tất cả các cửa sổ. Ban đầu, ta thêm $k$ số đầu tiên của mảng vào hash map và priority queue, sau đó gọi $\textit{get_mode()}$ để lấy mode và tần suất của cửa sổ đầu tiên, rồi cộng trọng số của nó vào $\textit{ans}$.

Tiếp theo, bắt đầu từ số thứ $k$, ta thêm từng số vào hash map và priority queue, đồng thời giảm tần suất của số ngoài cùng bên trái cửa sổ trong hash map. Sau đó, ta gọi $\textit{get_mode()}$ để lấy mode và tần suất của cửa sổ hiện tại, rồi cộng trọng số của nó vào $\textit{ans}$.

Cuối cùng, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n \log k)$, và độ phức tạp không gian là $O(k)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def modeWeight(self, nums: List[int], k: int) -> int:
        pq = []
        cnt = defaultdict(int)
        for x in nums[:k]:
            cnt[x] += 1
            heappush(pq, (-cnt[x], x))

        def get_mode() -> int:
            while -pq[0][0] != cnt[pq[0][1]]:
                heappop(pq)
            freq, val = -pq[0][0], pq[0][1]
            return freq * val

        ans = 0
        ans += get_mode()

        for i in range(k, len(nums)):
            x, y = nums[i], nums[i - k]
            cnt[x] += 1
            cnt[y] -= 1
            heappush(pq, (-cnt[x], x))
            heappush(pq, (-cnt[y], y))

            ans += get_mode()

        return ans
```

#### Java

```java
class Solution {
    public long modeWeight(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        PriorityQueue<int[]> pq = new PriorityQueue<>(
            (a, b) -> a[0] != b[0] ? Integer.compare(a[0], b[0]) : Integer.compare(a[1], b[1]));

        for (int i = 0; i < k; i++) {
            int x = nums[i];
            cnt.merge(x, 1, Integer::sum);
            pq.offer(new int[] {-cnt.get(x), x});
        }

        long ans = 0;

        Supplier<Long> getMode = () -> {
            while (true) {
                int[] top = pq.peek();
                int val = top[1];
                int freq = -top[0];
                if (cnt.getOrDefault(val, 0) == freq) {
                    return 1L * freq * val;
                }
                pq.poll();
            }
        };

        ans += getMode.get();

        for (int i = k; i < nums.length; i++) {
            int x = nums[i], y = nums[i - k];
            cnt.merge(x, 1, Integer::sum);
            pq.offer(new int[] {-cnt.get(x), x});
            cnt.merge(y, -1, Integer::sum);
            pq.offer(new int[] {-cnt.get(y), y});
            ans += getMode.get();
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long modeWeight(vector<int>& nums, int k) {
        unordered_map<int, int> cnt;
        priority_queue<pair<int, int>> pq; // {freq, -val}

        for (int i = 0; i < k; i++) {
            int x = nums[i];
            cnt[x]++;
            pq.push({cnt[x], -x});
        }

        auto get_mode = [&]() {
            while (true) {
                auto [freq, negVal] = pq.top();
                int val = -negVal;
                if (cnt[val] == freq) {
                    return 1LL * freq * val;
                }
                pq.pop();
            }
        };

        long long ans = 0;
        ans += get_mode();

        for (int i = k; i < nums.size(); i++) {
            int x = nums[i], y = nums[i - k];
            cnt[x]++;
            cnt[y]--;
            pq.push({cnt[x], -x});
            pq.push({cnt[y], -y});
            ans += get_mode();
        }

        return ans;
    }
};
```

#### Go

```go
func modeWeight(nums []int, k int) int64 {
	cnt := make(map[int]int)
	pq := &MaxHeap{}
	heap.Init(pq)

	for i := 0; i < k; i++ {
		x := nums[i]
		cnt[x]++
		heap.Push(pq, pair{cnt[x], x})
	}

	getMode := func() int64 {
		for {
			top := (*pq)[0]
			if cnt[top.val] == top.freq {
				return int64(top.freq) * int64(top.val)
			}
			heap.Pop(pq)
		}
	}

	var ans int64
	ans += getMode()

	for i := k; i < len(nums); i++ {
		x, y := nums[i], nums[i-k]
		cnt[x]++
		cnt[y]--
		heap.Push(pq, pair{cnt[x], x})
		heap.Push(pq, pair{cnt[y], y})
		ans += getMode()
	}

	return ans
}

type pair struct {
	freq int
	val  int
}

type MaxHeap []pair

func (h MaxHeap) Len() int { return len(h) }
func (h MaxHeap) Less(i, j int) bool {
	if h[i].freq != h[j].freq {
		return h[i].freq > h[j].freq
	}
	return h[i].val < h[j].val
}
func (h MaxHeap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *MaxHeap) Push(x any) {
	*h = append(*h, x.(pair))
}
func (h *MaxHeap) Pop() any {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[:n-1]
	return x
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
