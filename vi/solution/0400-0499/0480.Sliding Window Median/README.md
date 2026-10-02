---
comments: true
difficulty: Hard
tags:
    - Array
    - Hash Table
    - Treap
    - Sliding Window
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [480. Sliding Window Median](https://leetcode.com/problems/sliding-window-median)

[中文文档](/solution/0400-0499/0480.Sliding%20Window%20Median/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Median</strong> là giá trị nằm giữa trong một danh sách số nguyên đã sắp xếp. Nếu danh sách có số phần tử chẵn thì không có giá trị chính giữa; khi đó median là trung bình cộng của hai giá trị ở giữa.</p>

<ul>
	<li>Ví dụ, nếu <code>arr = [2,<u>3</u>,4]</code> thì median là <code>3</code>.</li>
	<li>Nếu <code>arr = [1,<u>2,3</u>,4]</code> thì median là <code>(2 + 3) / 2 = 2.5</code>.</li>
</ul>

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>. Một sliding window kích thước <code>k</code> di chuyển từ đầu trái sang đầu phải của mảng. Bạn chỉ nhìn thấy <code>k</code> số trong window. Mỗi lần, window dịch sang phải một vị trí.</p>

<p>Hãy trả về <em>mảng median của từng window trong mảng ban đầu</em>. Đáp án sai lệch không quá <code>10<sup>-5</sup></code> so với giá trị thực vẫn được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,-1,-3,5,3,6,7], k = 3
<strong>Đầu ra:</strong> [1.00000,-1.00000,-1.00000,3.00000,5.00000,6.00000]
<strong>Giải thích:</strong> 
Vị trí window                  Trung vị
---------------                -----
[<strong>1  3  -1</strong>] -3  5  3  6  7        1
 1 [<strong>3  -1  -3</strong>] 5  3  6  7       -1
 1  3 [<strong>-1  -3  5</strong>] 3  6  7       -1
 1  3  -1 [<strong>-3  5  3</strong>] 6  7        3
 1  3  -1  -3 [<strong>5  3  6</strong>] 7        5
 1  3  -1  -3  5 [<strong>3  6  7</strong>]       6
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,2,3,1,4,2], k = 3
<strong>Đầu ra:</strong> [2.00000,3.00000,3.00000,3.00000,2.00000,3.00000,2.00000]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-2<sup>31</sup> &lt;= nums[i] &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai Priority Queue (Min-Heap và Max-Heap) + Xóa trì hoãn

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm median của từng window. Sắp xếp từng window tốn $O(nk\log k)$. Heap giúp lấy median nhưng không hỗ trợ xóa tùy ý giá trị sắp rời window.
>
> Dùng hai heap: max-heap lưu nửa nhỏ hơn, min-heap lưu nửa lớn hơn, cùng một map để ghi nhận các phần tử cần xóa trì hoãn. Sau khi thêm hoặc xóa, chỉ pop các phần tử đã bị xóa ở đầu heap, rồi cân bằng lại kích thước các phần tử còn hiệu lực.
>
> Xóa trì hoãn biến thao tác xóa tùy ý thành thao tác pop ở đầu heap. Kích thước phần tử còn hiệu lực được đếm riêng để hai phần tử đầu vẫn là các ứng viên median.

<!-- thinking:end -->

Có thể dùng hai priority queue (min-heap và max-heap) để duy trì các phần tử trong window hiện tại. Một priority queue lưu nửa nhỏ hơn, priority queue còn lại lưu nửa lớn hơn. Khi đó, median của window hiện tại là một trong hai phần tử đầu heap hoặc trung bình của chúng.

Thiết kế class $\textit{MedianFinder}$ để duy trì các phần tử trong window hiện tại. Class này gồm các method sau:

- `add_num(num)`: Thêm $\textit{num}$ vào window hiện tại.
- `find_median()`: Trả về median của các phần tử trong window hiện tại.
- `remove_num(num)`: Xóa $\textit{num}$ khỏi window hiện tại.
- `prune(pq)`: Nếu phần tử đầu heap có trong dictionary xóa trì hoãn $\textit{delayed}$, pop phần tử đó khỏi heap và giảm bộ đếm xóa trì hoãn. Nếu bộ đếm về 0, xóa phần tử khỏi dictionary.
- `rebalance()`: Nếu nửa nhỏ có nhiều hơn nửa lớn $2$ phần tử, chuyển phần tử đầu của nửa lớn sang nửa nhỏ. Nếu nửa nhỏ có ít phần tử hơn nửa lớn, cũng chuyển phần tử đầu của nửa lớn sang nửa nhỏ.

Trong method `add_num(num)`, trước tiên xét thêm $\textit{num}$ vào nửa nhỏ. Nếu $\textit{num}$ lớn hơn phần tử đầu của nửa lớn thì thêm vào nửa lớn. Sau đó gọi `rebalance()` để đảm bảo chênh lệch kích thước giữa hai priority queue không quá $1$.

Trong method `remove_num(num)`, tăng bộ đếm xóa trì hoãn của $\textit{num}$. Sau đó so sánh $\textit{num}$ với phần tử đầu của nửa nhỏ. Nếu $\textit{num}$ nhỏ hơn hoặc bằng phần tử này, cập nhật kích thước nửa nhỏ rồi gọi `prune()` để đảm bảo phần tử đầu nửa nhỏ không còn nằm trong dictionary xóa trì hoãn. Nếu không, cập nhật kích thước nửa lớn và gọi `prune()` để đảm bảo tương tự với phần tử đầu nửa lớn.

Trong method `find_median()`, nếu window hiện tại có số phần tử lẻ thì trả về phần tử đầu của nửa nhỏ; nếu chẵn thì trả về trung bình của phần tử đầu ở nửa nhỏ và nửa lớn.

Trong method `prune(pq)`, nếu phần tử đầu heap nằm trong dictionary xóa trì hoãn thì pop phần tử đó khỏi heap và giảm bộ đếm tương ứng. Nếu bộ đếm về 0, xóa phần tử khỏi dictionary.

Trong method `rebalance()`, nếu nửa nhỏ có nhiều hơn nửa lớn $2$ phần tử thì chuyển phần tử đầu của nửa lớn sang nửa nhỏ. Nếu nửa nhỏ có ít phần tử hơn nửa lớn thì cũng chuyển phần tử đầu của nửa lớn sang nửa nhỏ.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class MedianFinder:
    def __init__(self, k: int):
        self.k = k
        self.small = []
        self.large = []
        self.delayed = defaultdict(int)
        self.small_size = 0
        self.large_size = 0

    def add_num(self, num: int):
        if not self.small or num <= -self.small[0]:
            heappush(self.small, -num)
            self.small_size += 1
        else:
            heappush(self.large, num)
            self.large_size += 1
        self.rebalance()

    def find_median(self) -> float:
        return -self.small[0] if self.k & 1 else (-self.small[0] + self.large[0]) / 2

    def remove_num(self, num: int):
        self.delayed[num] += 1
        if num <= -self.small[0]:
            self.small_size -= 1
            if num == -self.small[0]:
                self.prune(self.small)
        else:
            self.large_size -= 1
            if num == self.large[0]:
                self.prune(self.large)
        self.rebalance()

    def prune(self, pq: List[int]):
        sign = -1 if pq is self.small else 1
        while pq and sign * pq[0] in self.delayed:
            self.delayed[sign * pq[0]] -= 1
            if self.delayed[sign * pq[0]] == 0:
                self.delayed.pop(sign * pq[0])
            heappop(pq)

    def rebalance(self):
        if self.small_size > self.large_size + 1:
            heappush(self.large, -heappop(self.small))
            self.small_size -= 1
            self.large_size += 1
            self.prune(self.small)
        elif self.small_size < self.large_size:
            heappush(self.small, -heappop(self.large))
            self.large_size -= 1
            self.small_size += 1
            self.prune(self.large)


class Solution:
    def medianSlidingWindow(self, nums: List[int], k: int) -> List[float]:
        finder = MedianFinder(k)
        for x in nums[:k]:
            finder.add_num(x)
        ans = [finder.find_median()]
        for i in range(k, len(nums)):
            finder.add_num(nums[i])
            finder.remove_num(nums[i - k])
            ans.append(finder.find_median())
        return ans
```

#### Java

```java
class MedianFinder {
    private PriorityQueue<Integer> small = new PriorityQueue<>(Comparator.reverseOrder());
    private PriorityQueue<Integer> large = new PriorityQueue<>();
    private Map<Integer, Integer> delayed = new HashMap<>();
    private int smallSize;
    private int largeSize;
    private int k;

    public MedianFinder(int k) {
        this.k = k;
    }

    public void addNum(int num) {
        if (small.isEmpty() || num <= small.peek()) {
            small.offer(num);
            ++smallSize;
        } else {
            large.offer(num);
            ++largeSize;
        }
        rebalance();
    }

    public double findMedian() {
        return (k & 1) == 1 ? small.peek() : ((double) small.peek() + large.peek()) / 2;
    }

    public void removeNum(int num) {
        delayed.merge(num, 1, Integer::sum);
        if (num <= small.peek()) {
            --smallSize;
            if (num == small.peek()) {
                prune(small);
            }
        } else {
            --largeSize;
            if (num == large.peek()) {
                prune(large);
            }
        }
        rebalance();
    }

    private void prune(PriorityQueue<Integer> pq) {
        while (!pq.isEmpty() && delayed.containsKey(pq.peek())) {
            if (delayed.merge(pq.peek(), -1, Integer::sum) == 0) {
                delayed.remove(pq.peek());
            }
            pq.poll();
        }
    }

    private void rebalance() {
        if (smallSize > largeSize + 1) {
            large.offer(small.poll());
            --smallSize;
            ++largeSize;
            prune(small);
        } else if (smallSize < largeSize) {
            small.offer(large.poll());
            --largeSize;
            ++smallSize;
            prune(large);
        }
    }
}

class Solution {
    public double[] medianSlidingWindow(int[] nums, int k) {
        MedianFinder finder = new MedianFinder(k);
        for (int i = 0; i < k; ++i) {
            finder.addNum(nums[i]);
        }
        int n = nums.length;
        double[] ans = new double[n - k + 1];
        ans[0] = finder.findMedian();
        for (int i = k; i < n; ++i) {
            finder.addNum(nums[i]);
            finder.removeNum(nums[i - k]);
            ans[i - k + 1] = finder.findMedian();
        }
        return ans;
    }
}
```

#### C++

```cpp
class MedianFinder {
public:
    MedianFinder(int k) {
        this->k = k;
    }

    void addNum(int num) {
        if (small.empty() || num <= small.top()) {
            small.push(num);
            ++smallSize;
        } else {
            large.push(num);
            ++largeSize;
        }
        reblance();
    }

    void removeNum(int num) {
        ++delayed[num];
        if (num <= small.top()) {
            --smallSize;
            if (num == small.top()) {
                prune(small);
            }
        } else {
            --largeSize;
            if (num == large.top()) {
                prune(large);
            }
        }
        reblance();
    }

    double findMedian() {
        return k & 1 ? small.top() : ((double) small.top() + large.top()) / 2.0;
    }

private:
    priority_queue<int> small;
    priority_queue<int, vector<int>, greater<int>> large;
    unordered_map<int, int> delayed;
    int smallSize = 0;
    int largeSize = 0;
    int k;

    template <typename T>
    void prune(T& pq) {
        while (!pq.empty() && delayed[pq.top()]) {
            if (--delayed[pq.top()] == 0) {
                delayed.erase(pq.top());
            }
            pq.pop();
        }
    }

    void reblance() {
        if (smallSize > largeSize + 1) {
            large.push(small.top());
            small.pop();
            --smallSize;
            ++largeSize;
            prune(small);
        } else if (smallSize < largeSize) {
            small.push(large.top());
            large.pop();
            ++smallSize;
            --largeSize;
            prune(large);
        }
    }
};

class Solution {
public:
    vector<double> medianSlidingWindow(vector<int>& nums, int k) {
        MedianFinder finder(k);
        for (int i = 0; i < k; ++i) {
            finder.addNum(nums[i]);
        }
        vector<double> ans = {finder.findMedian()};
        for (int i = k; i < nums.size(); ++i) {
            finder.addNum(nums[i]);
            finder.removeNum(nums[i - k]);
            ans.push_back(finder.findMedian());
        }
        return ans;
    }
};
```

#### Go

```go
type MedianFinder struct {
	small                hp
	large                hp
	delayed              map[int]int
	smallSize, largeSize int
	k                    int
}

func Constructor(k int) MedianFinder {
	return MedianFinder{hp{}, hp{}, map[int]int{}, 0, 0, k}
}

func (this *MedianFinder) AddNum(num int) {
	if this.small.Len() == 0 || num <= -this.small.IntSlice[0] {
		heap.Push(&this.small, -num)
		this.smallSize++
	} else {
		heap.Push(&this.large, num)
		this.largeSize++
	}
	this.rebalance()
}

func (this *MedianFinder) FindMedian() float64 {
	if this.k&1 == 1 {
		return float64(-this.small.IntSlice[0])
	}
	return float64(-this.small.IntSlice[0]+this.large.IntSlice[0]) / 2
}

func (this *MedianFinder) removeNum(num int) {
	this.delayed[num]++
	if num <= -this.small.IntSlice[0] {
		this.smallSize--
		if num == -this.small.IntSlice[0] {
			this.prune(&this.small)
		}
	} else {
		this.largeSize--
		if num == this.large.IntSlice[0] {
			this.prune(&this.large)
		}
	}
	this.rebalance()
}

func (this *MedianFinder) prune(pq *hp) {
	sign := 1
	if pq == &this.small {
		sign = -1
	}
	for pq.Len() > 0 && this.delayed[sign*pq.IntSlice[0]] > 0 {
		this.delayed[sign*pq.IntSlice[0]]--
		if this.delayed[sign*pq.IntSlice[0]] == 0 {
			delete(this.delayed, sign*pq.IntSlice[0])
		}
		heap.Pop(pq)
	}
}

func (this *MedianFinder) rebalance() {
	if this.smallSize > this.largeSize+1 {
		heap.Push(&this.large, -heap.Pop(&this.small).(int))
		this.smallSize--
		this.largeSize++
		this.prune(&this.small)
	} else if this.smallSize < this.largeSize {
		heap.Push(&this.small, -heap.Pop(&this.large).(int))
		this.smallSize++
		this.largeSize--
		this.prune(&this.large)
	}
}

func medianSlidingWindow(nums []int, k int) []float64 {
	finder := Constructor(k)
	for _, num := range nums[:k] {
		finder.AddNum(num)
	}
	ans := []float64{finder.FindMedian()}
	for i := k; i < len(nums); i++ {
		finder.AddNum(nums[i])
		finder.removeNum(nums[i-k])
		ans = append(ans, finder.FindMedian())
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

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Cài đặt heap với xóa trì hoãn khá dài. Ordered set hỗ trợ thêm và xóa trong $O(\log k)$. Set trái lưu nửa nhỏ hơn, set phải lưu nửa lớn hơn: thêm phần tử vào set phải, chuyển phần tử nhỏ nhất của nó sang set trái rồi cân bằng lại. Khi window đủ phần tử, lấy các giá trị ở hai đầu để tính median và xóa giá trị sắp rời window.
>
> Nếu có sẵn kiểu ordered set thì code sẽ ngắn hơn.

<!-- thinking:end -->

Có thể dùng hai ordered set để duy trì các phần tử trong window hiện tại. Ordered set $l$ lưu nửa nhỏ hơn, còn ordered set $r$ lưu nửa lớn hơn.

Duyệt mảng $\textit{nums}$. Với mỗi phần tử $x$, thêm nó vào ordered set $r$, rồi chuyển phần tử nhỏ nhất trong $r$ sang $l$. Nếu kích thước $l$ lớn hơn kích thước $r$ quá $1$, chuyển phần tử lớn nhất trong $l$ sang $r$.

Nếu window hiện tại có tổng cộng $k$ phần tử và kích thước lẻ, giá trị lớn nhất trong ordered set $l$ là median. Nếu kích thước chẵn, median là trung bình của giá trị lớn nhất trong $l$ và giá trị nhỏ nhất trong $r$. Sau đó xóa phần tử ngoài cùng bên trái khỏi window rồi tiếp tục duyệt mảng.

Độ phức tạp thời gian là $O(n \log k)$ và độ phức tạp không gian là $O(k)$, trong đó $n$ là độ dài mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def medianSlidingWindow(self, nums: List[int], k: int) -> List[float]:
        l = SortedList()
        r = SortedList()
        ans = []
        for i, x in enumerate(nums):
            r.add(x)
            l.add(r.pop(0))
            while len(l) - len(r) > 1:
                r.add(l.pop())
            j = i - k + 1
            if j >= 0:
                ans.append(l[-1] if k & 1 else (l[-1] + r[0]) / 2)
                if nums[j] in l:
                    l.remove(nums[j])
                else:
                    r.remove(nums[j])
        return ans
```

#### Java

```java
class Solution {
    public double[] medianSlidingWindow(int[] nums, int k) {
        TreeMap<Integer, Integer> l = new TreeMap<>();
        TreeMap<Integer, Integer> r = new TreeMap<>();
        int n = nums.length;
        double[] ans = new double[n - k + 1];
        int lSize = 0, rSize = 0;
        for (int i = 0; i < n; ++i) {
            r.merge(nums[i], 1, Integer::sum);
            int x = r.firstKey();
            if (r.merge(x, -1, Integer::sum) == 0) {
                r.remove(x);
            }
            l.merge(x, 1, Integer::sum);
            ++lSize;
            while (lSize - rSize > 1) {
                x = l.lastKey();
                if (l.merge(x, -1, Integer::sum) == 0) {
                    l.remove(x);
                }
                r.merge(x, 1, Integer::sum);
                --lSize;
                ++rSize;
            }
            int j = i - k + 1;
            if (j >= 0) {
                ans[j] = k % 2 == 1 ? l.lastKey() : ((double) l.lastKey() + r.firstKey()) / 2;
                if (l.containsKey(nums[j])) {
                    if (l.merge(nums[j], -1, Integer::sum) == 0) {
                        l.remove(nums[j]);
                    }
                    --lSize;
                } else {
                    if (r.merge(nums[j], -1, Integer::sum) == 0) {
                        r.remove(nums[j]);
                    }
                    --rSize;
                }
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
    vector<double> medianSlidingWindow(vector<int>& nums, int k) {
        multiset<int> l, r;
        int n = nums.size();
        vector<double> ans;
        for (int i = 0; i < n; ++i) {
            r.insert(nums[i]);
            l.insert(*r.begin());
            r.erase(r.begin());
            while (l.size() - r.size() > 1) {
                r.insert(*l.rbegin());
                l.erase(prev(l.end()));
            }
            int j = i - k + 1;
            if (j >= 0) {
                ans.push_back(k % 2 ? *l.rbegin() : ((double) *l.rbegin() + *r.begin()) / 2);
                auto it = l.find(nums[j]);
                if (it != l.end()) {
                    l.erase(it);
                } else {
                    r.erase(r.find(nums[j]));
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func medianSlidingWindow(nums []int, k int) (ans []float64) {
	l := redblacktree.New[int, int]()
	r := redblacktree.New[int, int]()
	merge := func(st *redblacktree.Tree[int, int], x, v int) {
		c, _ := st.Get(x)
		if c+v == 0 {
			st.Remove(x)
		} else {
			st.Put(x, c+v)
		}
	}
	lSize, rSize := 0, 0
	for i, x := range nums {
		merge(r, x, 1)
		x = r.Left().Key
		merge(r, x, -1)
		merge(l, x, 1)
		lSize++
		for lSize-rSize > 1 {
			x = l.Right().Key
			merge(l, x, -1)
			merge(r, x, 1)
			lSize--
			rSize++
		}
		if j := i - k + 1; j >= 0 {
			if k%2 == 1 {
				ans = append(ans, float64(l.Right().Key))
			} else {
				ans = append(ans, float64(l.Right().Key+r.Left().Key)/2)
			}
			if x = nums[j]; x <= l.Right().Key {
				merge(l, x, -1)
				lSize--
			} else {
				merge(r, x, -1)
				rSize--
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function medianSlidingWindow(nums: number[], k: number): number[] {
    const l = new TreapMultiSet<number>((a, b) => a - b);
    const r = new TreapMultiSet<number>((a, b) => a - b);
    const n = nums.length;
    const ans: number[] = [];
    for (let i = 0; i < n; ++i) {
        r.add(nums[i]);
        l.add(r.shift()!);
        while (l.size - r.size > 1) {
            r.add(l.pop()!);
        }
        const j = i - k + 1;
        if (j >= 0) {
            ans[j] = k % 2 ? l.last()! : (l.last()! + r.first()!) / 2;
            if (nums[j] <= l.last()!) {
                l.delete(nums[j]);
            } else {
                r.delete(nums[j]);
            }
        }
    }
    return ans;
}

type CompareFunction<T, R extends 'number' | 'boolean'> = (
    a: T,
    b: T,
) => R extends 'number' ? number : boolean;

interface ITreapMultiSet<T> extends Iterable<T> {
    add: (...value: T[]) => this;
    has: (value: T) => boolean;
    delete: (value: T) => void;

    bisectLeft: (value: T) => number;
    bisectRight: (value: T) => number;

    indexOf: (value: T) => number;
    lastIndexOf: (value: T) => number;

    at: (index: number) => T | undefined;
    first: () => T | undefined;
    last: () => T | undefined;

    lower: (value: T) => T | undefined;
    higher: (value: T) => T | undefined;
    floor: (value: T) => T | undefined;
    ceil: (value: T) => T | undefined;

    shift: () => T | undefined;
    pop: (index?: number) => T | undefined;

    count: (value: T) => number;

    keys: () => IterableIterator<T>;
    values: () => IterableIterator<T>;
    rvalues: () => IterableIterator<T>;
    entries: () => IterableIterator<[number, T]>;

    readonly size: number;
}

class TreapNode<T = number> {
    value: T;
    count: number;
    size: number;
    priority: number;
    left: TreapNode<T> | null;
    right: TreapNode<T> | null;

    constructor(value: T) {
        this.value = value;
        this.count = 1;
        this.size = 1;
        this.priority = Math.random();
        this.left = null;
        this.right = null;
    }

    static getSize(node: TreapNode<any> | null): number {
        return node?.size ?? 0;
    }

    static getFac(node: TreapNode<any> | null): number {
        return node?.priority ?? 0;
    }

    pushUp(): void {
        let tmp = this.count;
        tmp += TreapNode.getSize(this.left);
        tmp += TreapNode.getSize(this.right);
        this.size = tmp;
    }

    rotateRight(): TreapNode<T> {
        // eslint-disable-next-line @typescript-eslint/no-this-alias
        let node: TreapNode<T> = this;
        const left = node.left;
        node.left = left?.right ?? null;
        left && (left.right = node);
        left && (node = left);
        node.right?.pushUp();
        node.pushUp();
        return node;
    }

    rotateLeft(): TreapNode<T> {
        // eslint-disable-next-line @typescript-eslint/no-this-alias
        let node: TreapNode<T> = this;
        const right = node.right;
        node.right = right?.left ?? null;
        right && (right.left = node);
        right && (node = right);
        node.left?.pushUp();
        node.pushUp();
        return node;
    }
}

class TreapMultiSet<T = number> implements ITreapMultiSet<T> {
    private readonly root: TreapNode<T>;
    private readonly compareFn: CompareFunction<T, 'number'>;
    private readonly leftBound: T;
    private readonly rightBound: T;

    constructor(compareFn?: CompareFunction<T, 'number'>);
    constructor(compareFn: CompareFunction<T, 'number'>, leftBound: T, rightBound: T);
    constructor(
        compareFn: CompareFunction<T, any> = (a: any, b: any) => a - b,
        leftBound: any = -Infinity,
        rightBound: any = Infinity,
    ) {
        this.root = new TreapNode<T>(rightBound);
        this.root.priority = Infinity;
        this.root.left = new TreapNode<T>(leftBound);
        this.root.left.priority = -Infinity;
        this.root.pushUp();

        this.leftBound = leftBound;
        this.rightBound = rightBound;
        this.compareFn = compareFn;
    }

    get size(): number {
        return this.root.size - 2;
    }

    get height(): number {
        const getHeight = (node: TreapNode<T> | null): number => {
            if (node == null) return 0;
            return 1 + Math.max(getHeight(node.left), getHeight(node.right));
        };

        return getHeight(this.root);
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Returns true if value is a member.
     */
    has(value: T): boolean {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): boolean => {
            if (node == null) return false;
            if (compare(node.value, value) === 0) return true;
            if (compare(node.value, value) < 0) return dfs(node.right, value);
            return dfs(node.left, value);
        };

        return dfs(this.root, value);
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Add value to sorted set.
     */
    add(...values: T[]): this {
        const compare = this.compareFn;
        const dfs = (
            node: TreapNode<T> | null,
            value: T,
            parent: TreapNode<T>,
            direction: 'left' | 'right',
        ): void => {
            if (node == null) return;
            if (compare(node.value, value) === 0) {
                node.count++;
                node.pushUp();
            } else if (compare(node.value, value) > 0) {
                if (node.left) {
                    dfs(node.left, value, node, 'left');
                } else {
                    node.left = new TreapNode(value);
                    node.pushUp();
                }

                if (TreapNode.getFac(node.left) > node.priority) {
                    parent[direction] = node.rotateRight();
                }
            } else if (compare(node.value, value) < 0) {
                if (node.right) {
                    dfs(node.right, value, node, 'right');
                } else {
                    node.right = new TreapNode(value);
                    node.pushUp();
                }

                if (TreapNode.getFac(node.right) > node.priority) {
                    parent[direction] = node.rotateLeft();
                }
            }
            parent.pushUp();
        };

        values.forEach(value => dfs(this.root.left, value, this.root, 'left'));
        return this;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Remove value from sorted set if it is a member.
     * If value is not a member, do nothing.
     */
    delete(value: T): void {
        const compare = this.compareFn;
        const dfs = (
            node: TreapNode<T> | null,
            value: T,
            parent: TreapNode<T>,
            direction: 'left' | 'right',
        ): void => {
            if (node == null) return;

            if (compare(node.value, value) === 0) {
                if (node.count > 1) {
                    node.count--;
                    node?.pushUp();
                } else if (node.left == null && node.right == null) {
                    parent[direction] = null;
                } else {
                    // 旋到根节点
                    if (
                        node.right == null ||
                        TreapNode.getFac(node.left) > TreapNode.getFac(node.right)
                    ) {
                        parent[direction] = node.rotateRight();
                        dfs(parent[direction]?.right ?? null, value, parent[direction]!, 'right');
                    } else {
                        parent[direction] = node.rotateLeft();
                        dfs(parent[direction]?.left ?? null, value, parent[direction]!, 'left');
                    }
                }
            } else if (compare(node.value, value) > 0) {
                dfs(node.left, value, node, 'left');
            } else if (compare(node.value, value) < 0) {
                dfs(node.right, value, node, 'right');
            }

            parent?.pushUp();
        };

        dfs(this.root.left, value, this.root, 'left');
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Returns an index to insert value in the sorted set.
     * If the value is already present, the insertion point will be before (to the left of) any existing values.
     */
    bisectLeft(value: T): number {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): number => {
            if (node == null) return 0;

            if (compare(node.value, value) === 0) {
                return TreapNode.getSize(node.left);
            } else if (compare(node.value, value) > 0) {
                return dfs(node.left, value);
            } else if (compare(node.value, value) < 0) {
                return dfs(node.right, value) + TreapNode.getSize(node.left) + node.count;
            }

            return 0;
        };

        return dfs(this.root, value) - 1;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Returns an index to insert value in the sorted set.
     * If the value is already present, the insertion point will be before (to the right of) any existing values.
     */
    bisectRight(value: T): number {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): number => {
            if (node == null) return 0;

            if (compare(node.value, value) === 0) {
                return TreapNode.getSize(node.left) + node.count;
            } else if (compare(node.value, value) > 0) {
                return dfs(node.left, value);
            } else if (compare(node.value, value) < 0) {
                return dfs(node.right, value) + TreapNode.getSize(node.left) + node.count;
            }

            return 0;
        };
        return dfs(this.root, value) - 1;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Returns the index of the first occurrence of a value in the set, or -1 if it is not present.
     */
    indexOf(value: T): number {
        const compare = this.compareFn;
        let isExist = false;

        const dfs = (node: TreapNode<T> | null, value: T): number => {
            if (node == null) return 0;

            if (compare(node.value, value) === 0) {
                isExist = true;
                return TreapNode.getSize(node.left);
            } else if (compare(node.value, value) > 0) {
                return dfs(node.left, value);
            } else if (compare(node.value, value) < 0) {
                return dfs(node.right, value) + TreapNode.getSize(node.left) + node.count;
            }

            return 0;
        };
        const res = dfs(this.root, value) - 1;
        return isExist ? res : -1;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Returns the index of the last occurrence of a value in the set, or -1 if it is not present.
     */
    lastIndexOf(value: T): number {
        const compare = this.compareFn;
        let isExist = false;

        const dfs = (node: TreapNode<T> | null, value: T): number => {
            if (node == null) return 0;

            if (compare(node.value, value) === 0) {
                isExist = true;
                return TreapNode.getSize(node.left) + node.count - 1;
            } else if (compare(node.value, value) > 0) {
                return dfs(node.left, value);
            } else if (compare(node.value, value) < 0) {
                return dfs(node.right, value) + TreapNode.getSize(node.left) + node.count;
            }

            return 0;
        };

        const res = dfs(this.root, value) - 1;
        return isExist ? res : -1;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Returns the item located at the specified index.
     * @param index The zero-based index of the desired code unit. A negative index will count back from the last item.
     */
    at(index: number): T | undefined {
        if (index < 0) index += this.size;
        if (index < 0 || index >= this.size) return undefined;

        const dfs = (node: TreapNode<T> | null, rank: number): T | undefined => {
            if (node == null) return undefined;

            if (TreapNode.getSize(node.left) >= rank) {
                return dfs(node.left, rank);
            } else if (TreapNode.getSize(node.left) + node.count >= rank) {
                return node.value;
            } else {
                return dfs(node.right, rank - TreapNode.getSize(node.left) - node.count);
            }
        };

        const res = dfs(this.root, index + 2);
        return ([this.leftBound, this.rightBound] as any[]).includes(res) ? undefined : res;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Find and return the element less than `val`, return `undefined` if no such element found.
     */
    lower(value: T): T | undefined {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): T | undefined => {
            if (node == null) return undefined;
            if (compare(node.value, value) >= 0) return dfs(node.left, value);

            const tmp = dfs(node.right, value);
            if (tmp == null || compare(node.value, tmp) > 0) {
                return node.value;
            } else {
                return tmp;
            }
        };

        const res = dfs(this.root, value) as any;
        return res === this.leftBound ? undefined : res;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Find and return the element greater than `val`, return `undefined` if no such element found.
     */
    higher(value: T): T | undefined {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): T | undefined => {
            if (node == null) return undefined;
            if (compare(node.value, value) <= 0) return dfs(node.right, value);

            const tmp = dfs(node.left, value);

            if (tmp == null || compare(node.value, tmp) < 0) {
                return node.value;
            } else {
                return tmp;
            }
        };

        const res = dfs(this.root, value) as any;
        return res === this.rightBound ? undefined : res;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Find and return the element less than or equal to `val`, return `undefined` if no such element found.
     */
    floor(value: T): T | undefined {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): T | undefined => {
            if (node == null) return undefined;
            if (compare(node.value, value) === 0) return node.value;
            if (compare(node.value, value) >= 0) return dfs(node.left, value);

            const tmp = dfs(node.right, value);
            if (tmp == null || compare(node.value, tmp) > 0) {
                return node.value;
            } else {
                return tmp;
            }
        };

        const res = dfs(this.root, value) as any;
        return res === this.leftBound ? undefined : res;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Find and return the element greater than or equal to `val`, return `undefined` if no such element found.
     */
    ceil(value: T): T | undefined {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): T | undefined => {
            if (node == null) return undefined;
            if (compare(node.value, value) === 0) return node.value;
            if (compare(node.value, value) <= 0) return dfs(node.right, value);

            const tmp = dfs(node.left, value);

            if (tmp == null || compare(node.value, tmp) < 0) {
                return node.value;
            } else {
                return tmp;
            }
        };

        const res = dfs(this.root, value) as any;
        return res === this.rightBound ? undefined : res;
    }

    /**
     * @complexity `O(logn)`
     * @description
     * Returns the last element from set.
     * If the set is empty, undefined is returned.
     */
    first(): T | undefined {
        const iter = this.inOrder();
        iter.next();
        const res = iter.next().value;
        return res === this.rightBound ? undefined : res;
    }

    /**
     * @complexity `O(logn)`
     * @description
     * Returns the last element from set.
     * If the set is empty, undefined is returned .
     */
    last(): T | undefined {
        const iter = this.reverseInOrder();
        iter.next();
        const res = iter.next().value;
        return res === this.leftBound ? undefined : res;
    }

    /**
     * @complexity `O(logn)`
     * @description
     * Removes the first element from an set and returns it.
     * If the set is empty, undefined is returned and the set is not modified.
     */
    shift(): T | undefined {
        const first = this.first();
        if (first === undefined) return undefined;
        this.delete(first);
        return first;
    }

    /**
     * @complexity `O(logn)`
     * @description
     * Removes the last element from an set and returns it.
     * If the set is empty, undefined is returned and the set is not modified.
     */
    pop(index?: number): T | undefined {
        if (index == null) {
            const last = this.last();
            if (last === undefined) return undefined;
            this.delete(last);
            return last;
        }

        const toDelete = this.at(index);
        if (toDelete == null) return;
        this.delete(toDelete);
        return toDelete;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description
     * Returns number of occurrences of value in the sorted set.
     */
    count(value: T): number {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): number => {
            if (node == null) return 0;
            if (compare(node.value, value) === 0) return node.count;
            if (compare(node.value, value) < 0) return dfs(node.right, value);
            return dfs(node.left, value);
        };

        return dfs(this.root, value);
    }

    *[Symbol.iterator](): Generator<T, any, any> {
        yield* this.values();
    }

    /**
     * @description
     * Returns an iterable of keys in the set.
     */
    *keys(): Generator<T, any, any> {
        yield* this.values();
    }

    /**
     * @description
     * Returns an iterable of values in the set.
     */
    *values(): Generator<T, any, any> {
        const iter = this.inOrder();
        iter.next();
        const steps = this.size;
        for (let _ = 0; _ < steps; _++) {
            yield iter.next().value;
        }
    }

    /**
     * @description
     * Returns a generator for reversed order traversing the set.
     */
    *rvalues(): Generator<T, any, any> {
        const iter = this.reverseInOrder();
        iter.next();
        const steps = this.size;
        for (let _ = 0; _ < steps; _++) {
            yield iter.next().value;
        }
    }

    /**
     * @description
     * Returns an iterable of key, value pairs for every entry in the set.
     */
    *entries(): IterableIterator<[number, T]> {
        const iter = this.inOrder();
        iter.next();
        const steps = this.size;
        for (let i = 0; i < steps; i++) {
            yield [i, iter.next().value];
        }
    }

    private *inOrder(root: TreapNode<T> | null = this.root): Generator<T, any, any> {
        if (root == null) return;
        yield* this.inOrder(root.left);
        const count = root.count;
        for (let _ = 0; _ < count; _++) {
            yield root.value;
        }
        yield* this.inOrder(root.right);
    }

    private *reverseInOrder(root: TreapNode<T> | null = this.root): Generator<T, any, any> {
        if (root == null) return;
        yield* this.reverseInOrder(root.right);
        const count = root.count;
        for (let _ = 0; _ < count; _++) {
            yield root.value;
        }
        yield* this.reverseInOrder(root.left);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
