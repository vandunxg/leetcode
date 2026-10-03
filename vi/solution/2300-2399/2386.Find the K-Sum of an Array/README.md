---
comments: true
difficulty: Hard
rating: 2647
source: Weekly Contest 307 Q4
tags:
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2386. Find the K-Sum of an Array](https://leetcode.com/problems/find-the-k-sum-of-an-array)

[中文文档](/solution/2300-2399/2386.Find%20the%20K-Sum%20of%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <strong>dương</strong> <code>k</code>. Bạn có thể chọn bất kỳ <strong>dãy con</strong> nào của mảng và tính tổng tất cả phần tử của nó.</p>

<p>Ta định nghĩa <strong>K-Sum</strong> của mảng là tổng dãy con <strong>lớn thứ</strong> <code>k<sup>th</sup></code> có thể thu được (<strong>không nhất thiết khác nhau</strong>).</p>

<p>Trả về <em>K-Sum của mảng</em>.</p>

<p>Một <strong>dãy con</strong> là một mảng có thể được tạo ra từ một mảng khác bằng cách xóa một số hoặc không xóa phần tử nào mà không thay đổi thứ tự của các phần tử còn lại.</p>

<p><strong>Lưu ý</strong> rằng dãy con rỗng được xem là có tổng bằng <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,-2], k = 5
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Tất cả các tổng dãy con có thể tạo ra, được sắp xếp theo thứ tự giảm dần, là:
6, 4, 4, 2, <u>2</u>, 0, 0, -2.
K-Sum thứ 5 của mảng là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,-2,3,4,-10,12], k = 16
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> K-Sum thứ 16 của mảng là 10.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= min(2000, 2<sup>n</sup>)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàng đợi ưu tiên (Min-Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm tổng dãy con lớn thứ $k$. Giá trị lớn nhất $mx$ là tổng các số dương; mọi tổng khác đều bằng $mx$ trừ đi tổng của một dãy con các giá trị tuyệt đối. Với $n \le 10^5$ và $k \le 2000$, không thể liệt kê $2^n$ trường hợp.
>
> Ta sắp xếp các giá trị tuyệt đối rồi dùng min-heap để mở rộng các tổng dãy con theo thứ tự không giảm: từ $(s,i)$, thêm “$nums[i]$” và “thay $nums[i-1]$ bằng $nums[i]$”. Sau khi lấy ra $k-1$ phần tử, phần tử đầu heap là lượng nhỏ thứ $k$ cần trừ.

<!-- thinking:end -->

Đầu tiên, ta tìm tổng mảng con lớn nhất $mx$, chính là tổng của tất cả các số dương.

Có thể nhận thấy tổng của các mảng con khác có thể được xem là tổng mảng con lớn nhất trừ đi tổng của các phần khác trong mảng con. Vì vậy, ta có thể chuyển bài toán thành tìm tổng mảng con nhỏ thứ $k$.

Ta chỉ cần sắp xếp tất cả các số theo thứ tự tăng dần dựa trên giá trị tuyệt đối, rồi tạo một min-heap để lưu tuple $(s, i)$, trong đó $s$ là tổng hiện tại và $i$ là chỉ số của số tiếp theo được chọn trong mảng con.

Mỗi lần, ta lấy phần tử đầu heap và thêm vào hai trường hợp mới: một là chọn số tiếp theo, hai là chọn số tiếp theo nhưng không chọn số hiện tại.

Vì mảng đã được sắp xếp theo thứ tự tăng dần, phương pháp này có thể duyệt qua tất cả các tổng mảng con theo đúng thứ tự mà không bỏ sót.

Độ phức tạp thời gian là $O(n \times \log n + k \times \log k)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kSum(self, nums: List[int], k: int) -> int:
        mx = 0
        for i, x in enumerate(nums):
            if x > 0:
                mx += x
            else:
                nums[i] = -x
        nums.sort()
        h = [(0, 0)]
        for _ in range(k - 1):
            s, i = heappop(h)
            if i < len(nums):
                heappush(h, (s + nums[i], i + 1))
                if i:
                    heappush(h, (s + nums[i] - nums[i - 1], i + 1))
        return mx - h[0][0]
```

#### Java

```java
class Solution {
    public long kSum(int[] nums, int k) {
        long mx = 0;
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            if (nums[i] > 0) {
                mx += nums[i];
            } else {
                nums[i] *= -1;
            }
        }
        Arrays.sort(nums);
        PriorityQueue<Pair<Long, Integer>> pq
            = new PriorityQueue<>(Comparator.comparing(Pair::getKey));
        pq.offer(new Pair<>(0L, 0));
        while (--k > 0) {
            var p = pq.poll();
            long s = p.getKey();
            int i = p.getValue();
            if (i < n) {
                pq.offer(new Pair<>(s + nums[i], i + 1));
                if (i > 0) {
                    pq.offer(new Pair<>(s + nums[i] - nums[i - 1], i + 1));
                }
            }
        }
        return mx - pq.peek().getKey();
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long kSum(vector<int>& nums, int k) {
        long long mx = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            if (nums[i] > 0) {
                mx += nums[i];
            } else {
                nums[i] *= -1;
            }
        }
        sort(nums.begin(), nums.end());
        using pli = pair<long long, int>;
        priority_queue<pli, vector<pli>, greater<pli>> pq;
        pq.push({0, 0});
        while (--k) {
            auto p = pq.top();
            pq.pop();
            long long s = p.first;
            int i = p.second;
            if (i < n) {
                pq.push({s + nums[i], i + 1});
                if (i) {
                    pq.push({s + nums[i] - nums[i - 1], i + 1});
                }
            }
        }
        return mx - pq.top().first;
    }
};
```

#### Go

```go
func kSum(nums []int, k int) int64 {
	mx := 0
	for i, x := range nums {
		if x > 0 {
			mx += x
		} else {
			nums[i] *= -1
		}
	}
	sort.Ints(nums)
	h := &hp{{0, 0}}
	for k > 1 {
		k--
		p := heap.Pop(h).(pair)
		if p.i < len(nums) {
			heap.Push(h, pair{p.sum + nums[p.i], p.i + 1})
			if p.i > 0 {
				heap.Push(h, pair{p.sum + nums[p.i] - nums[p.i-1], p.i + 1})
			}
		}
	}
	return int64(mx) - int64((*h)[0].sum)
}

type pair struct{ sum, i int }
type hp []pair

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].sum < h[j].sum }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)        { *h = append(*h, v.(pair)) }
func (h *hp) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
