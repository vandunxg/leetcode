---
comments: true
difficulty: Hard
rating: 2508
source: Weekly Contest 412 Q3
tags:
    - Array
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3266. Final Array State After K Multiplication Operations II](https://leetcode.com/problems/final-array-state-after-k-multiplication-operations-ii)

[中文文档](/solution/3200-3299/3266.Final%20Array%20State%20After%20K%20Multiplication%20Operations%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, một số nguyên <code>k</code> và một số nguyên <code>multiplier</code>.</p>

<p>Bạn cần thực hiện <code>k</code> thao tác trên <code>nums</code>. Trong mỗi thao tác:</p>

<ul>
	<li>Tìm giá trị <strong>nhỏ nhất</strong> <code>x</code> trong <code>nums</code>. Nếu giá trị nhỏ nhất xuất hiện nhiều lần, chọn phần tử xuất hiện <strong>đầu tiên</strong>.</li>
	<li>Thay giá trị nhỏ nhất được chọn <code>x</code> bằng <code>x * multiplier</code>.</li>
</ul>

<p>Sau <code>k</code> thao tác, áp dụng <strong>phép modulo</strong> <code>10<sup>9</sup> + 7</code> cho mọi giá trị trong <code>nums</code>.</p>

<p>Trả về một mảng số nguyên biểu diễn <em>trạng thái cuối cùng</em> của <code>nums</code> sau khi thực hiện tất cả <code>k</code> thao tác và áp dụng phép modulo.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,3,5,6], k = 5, multiplier = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[8,4,6,5,6]</span></p>

<p><strong>Giải thích:</strong></p>

<table>
	<tbody>
		<tr>
			<th>Thao tác</th>
			<th>Kết quả</th>
		</tr>
		<tr>
			<td>Sau thao tác 1</td>
			<td>[2, 2, 3, 5, 6]</td>
		</tr>
		<tr>
			<td>Sau thao tác 2</td>
			<td>[4, 2, 3, 5, 6]</td>
		</tr>
		<tr>
			<td>Sau thao tác 3</td>
			<td>[4, 4, 3, 5, 6]</td>
		</tr>
		<tr>
			<td>Sau thao tác 4</td>
			<td>[4, 4, 6, 5, 6]</td>
		</tr>
		<tr>
			<td>Sau thao tác 5</td>
			<td>[8, 4, 6, 5, 6]</td>
		</tr>
		<tr>
			<td>Sau khi áp dụng phép modulo</td>
			<td>[8, 4, 6, 5, 6]</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [100000,2000], k = 2, multiplier = 1000000</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[999999307,999999993]</span></p>

<p><strong>Giải thích:</strong></p>

<table>
	<tbody>
		<tr>
			<th>Thao tác</th>
			<th>Kết quả</th>
		</tr>
		<tr>
			<td>Sau thao tác 1</td>
			<td>[100000, 2000000000]</td>
		</tr>
		<tr>
			<td>Sau thao tác 2</td>
			<td>[100000000000, 2000000000]</td>
		</tr>
		<tr>
			<td>Sau khi áp dụng phép modulo</td>
			<td>[999999307, 999999993]</td>
		</tr>
	</tbody>
</table>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= multiplier &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Priority Queue (Min-Heap) + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Quy tắc giống bài I, nhưng $k\le 10^9$, nên chúng ta không thể mô phỏng từng bước. Khi mọi giá trị đều ít nhất bằng giá trị lớn nhất ban đầu $m$, mỗi phép nhân sẽ biến giá trị nhỏ nhất hiện tại thành một giá trị lớn nhất mới, và cứ sau $n$ thao tác thì mỗi chỉ số được xử lý đúng một lần.
>
> Một heap sẽ nhân các giá trị vẫn còn nhỏ hơn $m$ cho đến khi chúng bắt kịp $m$ hoặc dùng hết $k$. Phần $k$ còn lại được chia thành $k//n$ và $k\% n$, rồi áp dụng bằng fast pow và phép modulo. Nếu $\textit{multiplier}=1$, trả về ngay.

<!-- thinking:end -->

Gọi độ dài của mảng $\textit{nums}$ là $n$, và giá trị lớn nhất là $m$.

Trước tiên, chúng ta dùng một priority queue (min-heap) để mô phỏng các thao tác cho đến khi hoàn thành $k$ thao tác hoặc tất cả phần tử trong heap đều lớn hơn hoặc bằng m.

Lúc này, mọi phần tử trong mảng đều nhỏ hơn $m \times \textit{multiplier}$. Vì $1 \leq m \leq 10^9$ và $1 \leq \textit{multiplier} \leq 10^6$, nên $m \times \textit{multiplier} \leq 10^{15}$, nằm trong phạm vi của số nguyên 64-bit.

Tiếp theo, mỗi thao tác sẽ biến phần tử nhỏ nhất trong mảng thành phần tử lớn nhất. Do đó, sau mỗi $n$ thao tác liên tiếp, mỗi phần tử trong mảng sẽ trải qua đúng một phép nhân.

Vì vậy, sau phần mô phỏng, với $k$ thao tác còn lại, $k \bmod n$ phần tử nhỏ nhất trong mảng sẽ trải qua $\lfloor k / n \rfloor + 1$ phép nhân, còn các phần tử khác sẽ trải qua $\lfloor k / n \rfloor$ phép nhân.

Cuối cùng, chúng ta nhân mỗi phần tử trong mảng với số lần nhân tương ứng và lấy kết quả modulo $10^9 + 7$. Có thể tính phép này bằng lũy thừa nhanh.

Độ phức tạp thời gian là $O(n \times \log n \times \log M + n \times \log k)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$, còn $M$ là giá trị lớn nhất trong mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getFinalState(self, nums: List[int], k: int, multiplier: int) -> List[int]:
        if multiplier == 1:
            return nums
        pq = [(x, i) for i, x in enumerate(nums)]
        heapify(pq)
        m = max(nums)
        while k and pq[0][0] < m:
            x, i = heappop(pq)
            heappush(pq, (x * multiplier, i))
            k -= 1
        n = len(nums)
        mod = 10**9 + 7
        pq.sort()
        for i, (x, j) in enumerate(pq):
            nums[j] = x * pow(multiplier, k // n + int(i < k % n), mod) % mod
        return nums
```

#### Java

```java
class Solution {
    public int[] getFinalState(int[] nums, int k, int multiplier) {
        if (multiplier == 1) {
            return nums;
        }
        PriorityQueue<long[]> pq = new PriorityQueue<>(
            (a, b) -> a[0] == b[0] ? Long.compare(a[1], b[1]) : Long.compare(a[0], b[0]));
        int n = nums.length;
        int m = Arrays.stream(nums).max().getAsInt();
        for (int i = 0; i < n; ++i) {
            pq.offer(new long[] {nums[i], i});
        }
        for (; k > 0 && pq.peek()[0] < m; --k) {
            long[] p = pq.poll();
            p[0] *= multiplier;
            pq.offer(p);
        }
        final int mod = (int) 1e9 + 7;
        for (int i = 0; i < n; ++i) {
            long[] p = pq.poll();
            long x = p[0];
            int j = (int) p[1];
            nums[j] = (int) ((x % mod) * qpow(multiplier, k / n + (i < k % n ? 1 : 0), mod) % mod);
        }
        return nums;
    }

    private int qpow(long a, long n, long mod) {
        long ans = 1 % mod;
        for (; n > 0; n >>= 1) {
            if ((n & 1) == 1) {
                ans = ans * a % mod;
            }
            a = a * a % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> getFinalState(vector<int>& nums, int k, int multiplier) {
        if (multiplier == 1) {
            return nums;
        }

        using ll = long long;
        using pli = pair<ll, int>;
        auto cmp = [](const pli& a, const pli& b) {
            if (a.first == b.first) {
                return a.second > b.second;
            }
            return a.first > b.first;
        };
        priority_queue<pli, vector<pli>, decltype(cmp)> pq(cmp);

        int n = nums.size();
        int m = *max_element(nums.begin(), nums.end());

        for (int i = 0; i < n; ++i) {
            pq.emplace(nums[i], i);
        }

        while (k > 0 && pq.top().first < m) {
            auto p = pq.top();
            pq.pop();
            p.first *= multiplier;
            pq.emplace(p);
            --k;
        }

        auto qpow = [&](ll a, ll n, ll mod) {
            ll ans = 1 % mod;
            a = a % mod;
            while (n > 0) {
                if (n & 1) {
                    ans = ans * a % mod;
                }
                a = a * a % mod;
                n >>= 1;
            }
            return ans;
        };

        const int mod = 1e9 + 7;
        for (int i = 0; i < n; ++i) {
            auto p = pq.top();
            pq.pop();
            long long x = p.first;
            int j = p.second;
            nums[j] = static_cast<int>((x % mod) * qpow(multiplier, k / n + (i < k % n ? 1 : 0), mod) % mod);
        }

        return nums;
    }
};
```

#### Go

```go
func getFinalState(nums []int, k int, multiplier int) []int {
	if multiplier == 1 {
		return nums
	}
	n := len(nums)
	pq := make(hp, n)
	for i, x := range nums {
		pq[i] = pair{x, i}
	}
	heap.Init(&pq)
	m := slices.Max(nums)
	for ; k > 0 && pq[0].x < m; k-- {
		x := pq[0]
		heap.Pop(&pq)
		x.x *= multiplier
		heap.Push(&pq, x)
	}
	const mod int = 1e9 + 7

	for i := range nums {
		p := heap.Pop(&pq).(pair)
		x, j := p.x, p.i
		power := k / n
		if i < k%n {
			power++
		}
		nums[j] = (x % mod) * qpow(multiplier, power, mod) % mod
	}
	return nums
}

func qpow(a, n, mod int) int {
	ans := 1 % mod
	a = a % mod
	for n > 0 {
		if n&1 == 1 {
			ans = (ans * a) % mod
		}
		a = (a * a) % mod
		n >>= 1
	}
	return int(ans)
}

type pair struct{ x, i int }
type hp []pair

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].x < h[j].x || h[i].x == h[j].x && h[i].i < h[j].i }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(x any)        { *h = append(*h, x.(pair)) }
func (h *hp) Pop() (x any)      { a := *h; x = a[len(a)-1]; *h = a[:len(a)-1]; return x }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
