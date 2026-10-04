---
comments: true
difficulty: Medium
rating: 1785
source: Weekly Contest 342 Q3
tags:
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [2653. Sliding Subarray Beauty](https://leetcode.com/problems/sliding-subarray-beauty)

[中文文档](/solution/2600-2699/2653.Sliding%20Subarray%20Beauty/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> gồm <code>n</code> số nguyên, hãy tìm <strong>beauty</strong> của mỗi mảng con có kích thước <code>k</code>.</p>

<p><strong>Beauty</strong> của một mảng con là <code>x<sup>th</sup></code><strong> số nguyên nhỏ nhất </strong> trong mảng con nếu đó là <strong>số âm</strong>, hoặc <code>0</code> nếu mảng con có ít hơn <code>x</code> số nguyên âm.</p>

<p>Hãy trả về <em>một mảng số nguyên gồm </em><code>n - k + 1</code> <em>phần tử, biểu thị </em><strong>beauty</strong><em> của các mảng con <strong>theo thứ tự</strong> bắt đầu từ chỉ số đầu tiên trong mảng.</em></p>

<ul>
	<li>
	<p>Mảng con là một dãy phần tử liên tiếp <strong>không rỗng</strong> trong một mảng.</p>
	</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,-1,-3,-2,3], k = 3, x = 2
<strong>Đầu ra:</strong> [-1,-2,-2]
<strong>Giải thích:</strong> Có 3 mảng con có kích thước k = 3.
Mảng con đầu tiên là <code>[1, -1, -3]</code> và số nguyên âm nhỏ thứ 2<sup>nd</sup> là -1.&nbsp;
Mảng con thứ hai là <code>[-1, -3, -2]</code> và số nguyên âm nhỏ thứ 2<sup>nd</sup> là -2.&nbsp;
Mảng con thứ ba là <code>[-3, -2, 3]&nbsp;</code> và số nguyên âm nhỏ thứ 2<sup>nd</sup> là -2.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,-2,-3,-4,-5], k = 2, x = 2
<strong>Đầu ra:</strong> [-1,-2,-3,-4]
<strong>Giải thích:</strong> Có 4 mảng con có kích thước k = 2.
Với <code>[-1, -2]</code>, số nguyên âm nhỏ thứ 2<sup>nd</sup> là -1.
Với <code>[-2, -3]</code>, số nguyên âm nhỏ thứ 2<sup>nd</sup> là -2.
Với <code>[-3, -4]</code>, số nguyên âm nhỏ thứ 2<sup>nd</sup> là -3.
Với <code>[-4, -5]</code>, số nguyên âm nhỏ thứ 2<sup>nd</sup> là -4.&nbsp;</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-3,1,2,-3,0,-3], k = 2, x = 1
<strong>Đầu ra:</strong> [-3,0,-3,-3,-3]
<strong>Giải thích:</strong> Có 5 mảng con có kích thước k = 2<strong>.</strong>
Với <code>[-3, 1]</code>, số nguyên âm nhỏ thứ 1<sup>st</sup> là -3.
Với <code>[1, 2]</code>, không có số nguyên âm nào nên beauty là 0.
Với <code>[2, -3]</code>, số nguyên âm nhỏ thứ 1<sup>st</sup> là -3.
Với <code>[-3, 0]</code>, số nguyên âm nhỏ thứ 1<sup>st</sup> là -3.
Với <code>[0, -3]</code>, số nguyên âm nhỏ thứ 1<sup>st</sup> là -3.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length&nbsp;</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
	<li><code>1 &lt;= x &lt;= k&nbsp;</code></li>
	<li><code>-50&nbsp;&lt;= nums[i] &lt;= 50&nbsp;</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Một cửa sổ trả về giá trị nhỏ thứ $x$ nếu giá trị đó âm, ngược lại trả về $0$. Các giá trị nằm trong $[-50,50]$, nên không cần sắp xếp từng cửa sổ. Với $n \le 10^5$, $O(nk\log k)$ là quá lớn.
>
> Ta dùng một mảng đếm có kích thước $101$ để theo dõi cửa sổ. Duyệt từ phía các giá trị âm cho đến khi tổng tiền tố đạt $x$ sẽ tìm được phần tử theo thứ tự; nếu không có giá trị âm nào thì trả về $0$.

<!-- thinking:end -->

Ta nhận thấy rằng miền giá trị của các phần tử trong mảng $nums$ là $[-50,50]$. Do đó, ta có thể sử dụng một mảng có độ dài $101$, ký hiệu là $cnt$, để đếm số lần xuất hiện của mỗi số trong $[-50,50]$. Vì có các số âm, ta có thể cộng $50$ vào mỗi số để đưa tất cả về không âm, từ đó sử dụng mảng $cnt$ để đếm số lần xuất hiện của từng số.

Tiếp theo, ta duyệt qua mảng $nums$, duy trì một cửa sổ trượt có độ dài $k$. Số lần xuất hiện của tất cả phần tử trong cửa sổ được ghi lại trong mảng $cnt$. Sau đó, ta duyệt qua mảng $cnt$ để tìm số nhỏ thứ $x$, đây chính là giá trị beauty của cửa sổ trượt hiện tại. Nếu không có số nhỏ thứ $x$, giá trị beauty sẽ là $0$.

Độ phức tạp thời gian là $O(n \times 50)$, và độ phức tạp không gian là $O(100)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getSubarrayBeauty(self, nums: List[int], k: int, x: int) -> List[int]:
        def f(x: int) -> int:
            s = 0
            for i in range(50):
                s += cnt[i]
                if s >= x:
                    return i - 50
            return 0

        cnt = [0] * 101
        for v in nums[:k]:
            cnt[v + 50] += 1
        ans = [f(x)]
        for i in range(k, len(nums)):
            cnt[nums[i] + 50] += 1
            cnt[nums[i - k] + 50] -= 1
            ans.append(f(x))
        return ans
```

#### Java

```java
class Solution {
    public int[] getSubarrayBeauty(int[] nums, int k, int x) {
        int n = nums.length;
        int[] cnt = new int[101];
        for (int i = 0; i < k; ++i) {
            ++cnt[nums[i] + 50];
        }
        int[] ans = new int[n - k + 1];
        ans[0] = f(cnt, x);
        for (int i = k, j = 1; i < n; ++i) {
            ++cnt[nums[i] + 50];
            --cnt[nums[i - k] + 50];
            ans[j++] = f(cnt, x);
        }
        return ans;
    }

    private int f(int[] cnt, int x) {
        int s = 0;
        for (int i = 0; i < 50; ++i) {
            s += cnt[i];
            if (s >= x) {
                return i - 50;
            }
        }
        return 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> getSubarrayBeauty(vector<int>& nums, int k, int x) {
        int n = nums.size();
        int cnt[101]{};
        for (int i = 0; i < k; ++i) {
            ++cnt[nums[i] + 50];
        }
        vector<int> ans(n - k + 1);
        auto f = [&](int x) {
            int s = 0;
            for (int i = 0; i < 50; ++i) {
                s += cnt[i];
                if (s >= x) {
                    return i - 50;
                }
            }
            return 0;
        };
        ans[0] = f(x);
        for (int i = k, j = 1; i < n; ++i) {
            ++cnt[nums[i] + 50];
            --cnt[nums[i - k] + 50];
            ans[j++] = f(x);
        }
        return ans;
    }
};
```

#### Go

```go
func getSubarrayBeauty(nums []int, k int, x int) []int {
	n := len(nums)
	cnt := [101]int{}
	for _, x := range nums[:k] {
		cnt[x+50]++
	}
	ans := make([]int, n-k+1)
	f := func(x int) int {
		s := 0
		for i := 0; i < 50; i++ {
			s += cnt[i]
			if s >= x {
				return i - 50
			}
		}
		return 0
	}
	ans[0] = f(x)
	for i, j := k, 1; i < n; i, j = i+1, j+1 {
		cnt[nums[i]+50]++
		cnt[nums[i-k]+50]--
		ans[j] = f(x)
	}
	return ans
}
```

#### TypeScript

```ts
function getSubarrayBeauty(nums: number[], k: number, x: number): number[] {
    const n = nums.length;
    const cnt: number[] = new Array(101).fill(0);
    for (let i = 0; i < k; ++i) {
        ++cnt[nums[i] + 50];
    }
    const ans: number[] = new Array(n - k + 1);
    const f = (x: number): number => {
        let s = 0;
        for (let i = 0; i < 50; ++i) {
            s += cnt[i];
            if (s >= x) {
                return i - 50;
            }
        }
        return 0;
    };
    ans[0] = f(x);
    for (let i = k, j = 1; i < n; ++i, ++j) {
        cnt[nums[i] + 50]++;
        cnt[nums[i - k] + 50]--;
        ans[j] = f(x);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hàng đợi ưu tiên kép (Min-Max Heap) + Xóa trì hoãn

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 cần miền giá trị rất nhỏ. Hai heap giữ $x$ giá trị nhỏ nhất ở một phía, để phần tử đầu heap là ứng viên cho beauty; các giá trị rời khỏi cửa sổ được xử lý bằng cách xóa trì hoãn.
>
> Khi đó, ta vẫn tìm được phần tử nhỏ thứ $x$ trong $O(n\log k)$ ngay cả khi miền giá trị lớn hơn; ở đây, cách này thay thế cho mảng đếm.

<!-- thinking:end -->

Ta có thể sử dụng hai hàng đợi ưu tiên (min-max heap) để duy trì các phần tử trong cửa sổ hiện tại. Một hàng đợi ưu tiên lưu $x$ phần tử nhỏ hơn trong cửa sổ hiện tại, hàng đợi còn lại lưu $k - x$ phần tử lớn hơn trong cửa sổ hiện tại. Ta cũng cần một từ điển xóa trì hoãn `delayed` để ghi lại những phần tử trong cửa sổ hiện tại cần được xóa.

Ta thiết kế một lớp `MedianFinder` để duy trì các phần tử trong cửa sổ hiện tại. Lớp này gồm các phương thức sau:

- `add_num(num)`: Thêm `num` vào cửa sổ hiện tại.
- `find()`: Trả về giá trị beauty của cửa sổ hiện tại.
- `remove_num(num)`: Xóa `num` khỏi cửa sổ hiện tại.
- `prune(pq)`: Nếu phần tử đầu heap nằm trong từ điển xóa trì hoãn `delayed`, lấy nó ra khỏi đầu heap và giảm một đơn vị số lần xóa trì hoãn của nó. Nếu số lần xóa trì hoãn của phần tử bằng không, xóa phần tử đó khỏi từ điển xóa trì hoãn.
- `rebalance()`: Cân bằng kích thước của hai hàng đợi ưu tiên.

Trong phương thức `add_num(num)`, trước tiên ta xét việc thêm `num` vào hàng đợi nhỏ hơn. Nếu số lượng phần tử nhỏ hơn $x$ hoặc `num` nhỏ hơn hoặc bằng phần tử đầu của hàng đợi nhỏ hơn, ta thêm `num` vào hàng đợi nhỏ hơn; nếu không, ta thêm `num` vào hàng đợi lớn hơn. Sau đó, ta gọi phương thức `rebalance()` để đảm bảo số phần tử trong hàng đợi nhỏ hơn không vượt quá $x$.

Trong phương thức `remove_num(num)`, ta tăng số lần xóa trì hoãn của `num` lên một. Sau đó, ta so sánh `num` với phần tử đầu của hàng đợi nhỏ hơn. Nếu `num` nhỏ hơn hoặc bằng phần tử đầu của hàng đợi nhỏ hơn, ta cập nhật kích thước của hàng đợi nhỏ hơn và gọi phương thức `prune()` để đảm bảo phần tử đầu của hàng đợi nhỏ hơn không nằm trong từ điển xóa trì hoãn. Nếu không, ta cập nhật kích thước của hàng đợi lớn hơn và gọi phương thức `prune()` để đảm bảo phần tử đầu của hàng đợi lớn hơn không nằm trong từ điển xóa trì hoãn.

Trong phương thức `find()`, nếu kích thước của hàng đợi nhỏ hơn bằng $x$, trả về phần tử đầu của hàng đợi nhỏ hơn; nếu không, trả về $0$.

Trong phương thức `prune(pq)`, nếu phần tử đầu heap nằm trong từ điển xóa trì hoãn, lấy nó ra khỏi đầu heap và giảm một đơn vị số lần xóa trì hoãn của nó. Nếu số lần xóa trì hoãn của phần tử bằng không, xóa phần tử đó khỏi từ điển xóa trì hoãn.

Trong phương thức `rebalance()`, nếu kích thước của hàng đợi nhỏ hơn lớn hơn $x$, thêm phần tử đầu của hàng đợi nhỏ hơn vào hàng đợi lớn hơn và gọi phương thức `prune()` để đảm bảo phần tử đầu của hàng đợi nhỏ hơn không nằm trong từ điển xóa trì hoãn. Nếu kích thước của hàng đợi nhỏ hơn nhỏ hơn $x$ và kích thước của hàng đợi lớn hơn lớn hơn $0$, thêm phần tử đầu của hàng đợi lớn hơn vào hàng đợi nhỏ hơn và gọi phương thức `prune()` để đảm bảo phần tử đầu của hàng đợi lớn hơn không nằm trong từ điển xóa trì hoãn.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng `nums`.

Bài toán tương tự:

- [480. Sliding Window Median](https://github.com/doocs/leetcode/blob/main/solution/0400-0499/0480.Sliding%20Window%20Median/README.md)

<!-- tabs:start -->

#### Python3

```python
class MedianFinder:
    def __init__(self, x: int):
        self.x = x
        self.small = []
        self.large = []
        self.delayed = defaultdict(int)
        self.small_size = 0
        self.large_size = 0

    def add_num(self, num: int):
        if self.small_size < self.x or num <= -self.small[0]:
            heappush(self.small, -num)
            self.small_size += 1
        else:
            heappush(self.large, num)
            self.large_size += 1
        self.rebalance()

    def find(self) -> float:
        return -self.small[0] if self.small_size == self.x else 0

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
        if self.small_size > self.x:
            heappush(self.large, -heappop(self.small))
            self.small_size -= 1
            self.large_size += 1
            self.prune(self.small)
        elif self.small_size < self.x and self.large_size > 0:
            heappush(self.small, -heappop(self.large))
            self.large_size -= 1
            self.small_size += 1
            self.prune(self.large)


class Solution:
    def getSubarrayBeauty(self, nums: List[int], k: int, x: int) -> List[int]:
        finder = MedianFinder(x)
        for i in range(k):
            if nums[i] < 0:
                finder.add_num(nums[i])
        ans = [finder.find()]
        for i in range(k, len(nums)):
            if nums[i] < 0:
                finder.add_num(nums[i])
            if nums[i - k] < 0:
                finder.remove_num(nums[i - k])
            ans.append(finder.find())
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
    private int x;

    public MedianFinder(int x) {
        this.x = x;
    }

    public void addNum(int num) {
        if (smallSize < x || num <= small.peek()) {
            small.offer(num);
            ++smallSize;
        } else {
            large.offer(num);
            ++largeSize;
        }
        rebalance();
    }

    public int find() {
        return smallSize == x ? small.peek() : 0;
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
        if (smallSize > x) {
            large.offer(small.poll());
            --smallSize;
            ++largeSize;
            prune(small);
        } else if (smallSize < x && largeSize > 0) {
            small.offer(large.poll());
            --largeSize;
            ++smallSize;
            prune(large);
        }
    }
}

class Solution {
    public int[] getSubarrayBeauty(int[] nums, int k, int x) {
        MedianFinder finder = new MedianFinder(x);
        for (int i = 0; i < k; ++i) {
            if (nums[i] < 0) {
                finder.addNum(nums[i]);
            }
        }
        int n = nums.length;
        int[] ans = new int[n - k + 1];
        ans[0] = finder.find();
        for (int i = k; i < n; ++i) {
            if (nums[i] < 0) {
                finder.addNum(nums[i]);
            }
            if (nums[i - k] < 0) {
                finder.removeNum(nums[i - k]);
            }
            ans[i - k + 1] = finder.find();
        }
        return ans;
    }
}
```

#### C++

```cpp
class MedianFinder {
public:
    MedianFinder(int x) {
        this->x = x;
    }

    void addNum(int num) {
        if (smallSize < x || num <= small.top()) {
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

    int find() {
        return smallSize == x ? small.top() : 0;
    }

private:
    priority_queue<int> small;
    priority_queue<int, vector<int>, greater<int>> large;
    unordered_map<int, int> delayed;
    int smallSize = 0;
    int largeSize = 0;
    int x;

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
        if (smallSize > x) {
            large.push(small.top());
            small.pop();
            --smallSize;
            ++largeSize;
            prune(small);
        } else if (smallSize < x && largeSize > 0) {
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
    vector<int> getSubarrayBeauty(vector<int>& nums, int k, int x) {
        MedianFinder finder(x);
        for (int i = 0; i < k; ++i) {
            if (nums[i] < 0) {
                finder.addNum(nums[i]);
            }
        }
        int n = nums.size();
        vector<int> ans;
        ans.push_back(finder.find());
        for (int i = k; i < n; ++i) {
            if (nums[i] < 0) {
                finder.addNum(nums[i]);
            }
            if (nums[i - k] < 0) {
                finder.removeNum(nums[i - k]);
            }
            ans.push_back(finder.find());
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
	x                    int
}

func Constructor(x int) MedianFinder {
	return MedianFinder{hp{}, hp{}, map[int]int{}, 0, 0, x}
}

func (this *MedianFinder) AddNum(num int) {
	if this.smallSize < this.x || num <= -this.small.IntSlice[0] {
		heap.Push(&this.small, -num)
		this.smallSize++
	} else {
		heap.Push(&this.large, num)
		this.largeSize++
	}
	this.rebalance()
}

func (this *MedianFinder) Find() int {
	if this.smallSize == this.x {
		return -this.small.IntSlice[0]
	}
	return 0
}

func (this *MedianFinder) RemoveNum(num int) {
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
	if this.smallSize > this.x {
		heap.Push(&this.large, -heap.Pop(&this.small).(int))
		this.smallSize--
		this.largeSize++
		this.prune(&this.small)
	} else if this.smallSize < this.x && this.largeSize > 0 {
		heap.Push(&this.small, -heap.Pop(&this.large).(int))
		this.smallSize++
		this.largeSize--
		this.prune(&this.large)
	}
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

func getSubarrayBeauty(nums []int, k int, x int) []int {
	finder := Constructor(x)
	for _, num := range nums[:k] {
		if num < 0 {
			finder.AddNum(num)
		}
	}
	ans := []int{finder.Find()}
	for i := k; i < len(nums); i++ {
		if nums[i] < 0 {
			finder.AddNum(nums[i])
		}
		if nums[i-k] < 0 {
			finder.RemoveNum(nums[i-k])
		}
		ans = append(ans, finder.Find())
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
