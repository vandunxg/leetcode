---
comments: true
difficulty: Medium
rating: 1665
source: Biweekly Contest 100 Q3
tags:
    - Array
    - Hash Table
    - Sorting
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2593. Find Score of an Array After Marking All Elements](https://leetcode.com/problems/find-score-of-an-array-after-marking-all-elements)

[中文文档](/solution/2500-2599/2593.Find%20Score%20of%20an%20Array%20After%20Marking%20All%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm các số nguyên dương.</p>

<p>Bắt đầu với <code>score = 0</code>, hãy thực hiện thuật toán sau:</p>

<ul>
	<li>Chọn số nguyên nhỏ nhất trong mảng chưa được đánh dấu. Nếu có nhiều số bằng nhau, chọn số có chỉ số nhỏ nhất.</li>
	<li>Cộng giá trị của số nguyên được chọn vào <code>score</code>.</li>
	<li>Đánh dấu <strong>phần tử được chọn và hai phần tử kề bên nếu chúng tồn tại</strong>.</li>
	<li>Lặp lại cho đến khi tất cả phần tử trong mảng đều được đánh dấu.</li>
</ul>

<p>Trả về <em>điểm số nhận được sau khi thực hiện thuật toán trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,3,4,5,2]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Các phần tử được đánh dấu như sau:
- 1 là phần tử nhỏ nhất chưa được đánh dấu, nên ta đánh dấu nó cùng hai phần tử kề bên: [<u>2</u>,<u>1</u>,<u>3</u>,4,5,2].
- 2 là phần tử nhỏ nhất chưa được đánh dấu, nên ta đánh dấu nó cùng phần tử kề bên bên trái: [<u>2</u>,<u>1</u>,<u>3</u>,4,<u>5</u>,<u>2</u>].
- 4 là phần tử chưa được đánh dấu duy nhất còn lại, nên ta đánh dấu nó: [<u>2</u>,<u>1</u>,<u>3</u>,<u>4</u>,<u>5</u>,<u>2</u>].
Điểm số là 1 + 2 + 4 = 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,5,1,3,2]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Các phần tử được đánh dấu như sau:
- 1 là phần tử nhỏ nhất chưa được đánh dấu, nên ta đánh dấu nó cùng hai phần tử kề bên: [2,3,<u>5</u>,<u>1</u>,<u>3</u>,2].
- 2 là phần tử nhỏ nhất chưa được đánh dấu. Vì có hai phần tử như vậy, ta chọn phần tử ngoài cùng bên trái, tức phần tử ở chỉ số 0, và đánh dấu nó cùng phần tử kề bên bên phải: [<u>2</u>,<u>3</u>,<u>5</u>,<u>1</u>,<u>3</u>,2].
- 2 là phần tử chưa được đánh dấu duy nhất còn lại, nên ta đánh dấu nó: [<u>2</u>,<u>3</u>,<u>5</u>,<u>1</u>,<u>3</u>,<u>2</u>].
Điểm số là 1 + 2 + 2 = 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàng đợi ưu tiên (Min Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Liên tục cộng phần tử nhỏ nhất chưa được đánh dấu (nếu bằng nhau thì chọn phần tử bên trái hơn) vào điểm số, rồi đánh dấu nó cùng các phần tử kề bên. Nếu mỗi lần đều quét để tìm phần tử nhỏ nhất thì độ phức tạp là bậc hai.
>
> Min-heap chứa các cặp $(value,index)$ sẽ lấy ra phần tử nhỏ nhất toàn cục tiếp theo chưa được đánh dấu. Sau khi đánh dấu vùng lân cận, loại bỏ các phần tử ở đỉnh heap đã được đánh dấu. Mỗi phần tử được đưa vào heap đúng một lần.

<!-- thinking:end -->

Ta sử dụng một hàng đợi ưu tiên để lưu các phần tử chưa được đánh dấu trong mảng. Mỗi phần tử trong hàng đợi là một tuple $(x, i)$, trong đó $x$ và $i$ lần lượt là giá trị và chỉ số của phần tử trong mảng. Mảng $vis$ được dùng để ghi lại phần tử nào trong mảng đã được đánh dấu.

Mỗi lần lấy phần tử nhỏ nhất $(x, i)$ ra khỏi hàng đợi, ta cộng $x$ vào đáp án, sau đó đánh dấu phần tử ở vị trí $i$ cùng các phần tử kề bên trái và phải của vị trí $i$, tức là các phần tử ở vị trí $i-1$ và $i+1$. Tiếp theo, ta kiểm tra phần tử ở đỉnh heap đã được đánh dấu hay chưa. Nếu đã được đánh dấu, ta liên tục lấy phần tử ở đỉnh heap ra cho đến khi phần tử ở đỉnh chưa được đánh dấu hoặc heap rỗng.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findScore(self, nums: List[int]) -> int:
        n = len(nums)
        vis = [False] * n
        q = [(x, i) for i, x in enumerate(nums)]
        heapify(q)
        ans = 0
        while q:
            x, i = heappop(q)
            ans += x
            vis[i] = True
            for j in (i - 1, i + 1):
                if 0 <= j < n:
                    vis[j] = True
            while q and vis[q[0][1]]:
                heappop(q)
        return ans
```

#### Java

```java
class Solution {
    public long findScore(int[] nums) {
        int n = nums.length;
        boolean[] vis = new boolean[n];
        PriorityQueue<int[]> q
            = new PriorityQueue<>((a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        for (int i = 0; i < n; ++i) {
            q.offer(new int[] {nums[i], i});
        }
        long ans = 0;
        while (!q.isEmpty()) {
            var p = q.poll();
            ans += p[0];
            vis[p[1]] = true;
            for (int j : List.of(p[1] - 1, p[1] + 1)) {
                if (j >= 0 && j < n) {
                    vis[j] = true;
                }
            }
            while (!q.isEmpty() && vis[q.peek()[1]]) {
                q.poll();
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
    long long findScore(vector<int>& nums) {
        int n = nums.size();
        vector<bool> vis(n);
        using pii = pair<int, int>;
        priority_queue<pii, vector<pii>, greater<pii>> q;
        for (int i = 0; i < n; ++i) {
            q.emplace(nums[i], i);
        }
        long long ans = 0;
        while (!q.empty()) {
            auto [x, i] = q.top();
            q.pop();
            ans += x;
            vis[i] = true;
            if (i + 1 < n) {
                vis[i + 1] = true;
            }
            if (i - 1 >= 0) {
                vis[i - 1] = true;
            }
            while (!q.empty() && vis[q.top().second]) {
                q.pop();
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findScore(nums []int) (ans int64) {
	h := hp{}
	for i, x := range nums {
		heap.Push(&h, pair{x, i})
	}
	n := len(nums)
	vis := make([]bool, n)
	for len(h) > 0 {
		p := heap.Pop(&h).(pair)
		x, i := p.x, p.i
		ans += int64(x)
		vis[i] = true
		for _, j := range []int{i - 1, i + 1} {
			if j >= 0 && j < n {
				vis[j] = true
			}
		}
		for len(h) > 0 && vis[h[0].i] {
			heap.Pop(&h)
		}
	}
	return
}

type pair struct{ x, i int }
type hp []pair

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].x < h[j].x || (h[i].x == h[j].x && h[i].i < h[j].i) }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)        { *h = append(*h, v.(pair)) }
func (h *hp) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

#### TypeScript

```ts
interface pair {
    x: number;
    i: number;
}

function findScore(nums: number[]): number {
    const q = new PriorityQueue({
        compare: (a: pair, b: pair) => (a.x === b.x ? a.i - b.i : a.x - b.x),
    });
    const n = nums.length;
    const vis: boolean[] = new Array(n).fill(false);
    for (let i = 0; i < n; ++i) {
        q.enqueue({ x: nums[i], i });
    }
    let ans = 0;
    while (!q.isEmpty()) {
        const { x, i } = q.dequeue()!;
        if (vis[i]) {
            continue;
        }
        ans += x;
        vis[i] = true;
        if (i - 1 >= 0) {
            vis[i - 1] = true;
        }
        if (i + 1 < n) {
            vis[i + 1] = true;
        }
        while (!q.isEmpty() && vis[q.front()!.i]) {
            q.dequeue();
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tìm phần tử nhỏ nhất tiếp theo một cách động. Thứ tự này chính là thứ tự tĩnh của $(value,index)$, nên ta chỉ cần sắp xếp các chỉ số một lần rồi bỏ qua những chỉ số đã bị phần tử kề bên đánh dấu. Không cần loại bỏ các phần tử trong heap.

<!-- thinking:end -->

Ta có thể tạo một mảng chỉ số $idx$ với $idx[i]=i$, sau đó sắp xếp mảng chỉ số $idx$ theo giá trị các phần tử trong mảng $nums$. Nếu giá trị các phần tử bằng nhau, ta sắp xếp theo giá trị chỉ số.

Tiếp theo, tạo một mảng $vis$ có độ dài $n+2$, trong đó $vis[i]=false$ cho biết phần tử trong mảng đã được đánh dấu hay chưa.

Ta duyệt mảng chỉ số $idx$. Với mỗi chỉ số $i$ trong mảng, nếu $vis[i+1]$ là $false$, tức là phần tử ở vị trí $i$ chưa được đánh dấu, ta cộng $nums[i]$ vào đáp án, sau đó đánh dấu phần tử ở vị trí $i$ cùng các phần tử kề bên trái và phải của vị trí $i$, tức là các phần tử ở vị trí $i-1$ và $i+1$. Tiếp tục duyệt mảng chỉ số $idx$ cho đến hết.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findScore(self, nums: List[int]) -> int:
        n = len(nums)
        vis = [False] * (n + 2)
        idx = sorted(range(n), key=lambda i: (nums[i], i))
        ans = 0
        for i in idx:
            if not vis[i + 1]:
                ans += nums[i]
                vis[i] = vis[i + 2] = True
        return ans
```

#### Java

```java
class Solution {
    public long findScore(int[] nums) {
        int n = nums.length;
        boolean[] vis = new boolean[n + 2];
        Integer[] idx = new Integer[n];
        Arrays.setAll(idx, i -> i);
        Arrays.sort(idx, (i, j) -> nums[i] - nums[j]);
        long ans = 0;
        for (int i : idx) {
            if (!vis[i + 1]) {
                ans += nums[i];
                vis[i] = true;
                vis[i + 2] = true;
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
    long long findScore(vector<int>& nums) {
        int n = nums.size();
        vector<int> idx(n);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) {
            return nums[i] < nums[j] || (nums[i] == nums[j] && i < j);
        });
        long long ans = 0;
        vector<bool> vis(n + 2);
        for (int i : idx) {
            if (!vis[i + 1]) {
                ans += nums[i];
                vis[i] = vis[i + 2] = true;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findScore(nums []int) (ans int64) {
	n := len(nums)
	idx := make([]int, n)
	for i := range idx {
		idx[i] = i
	}
	sort.Slice(idx, func(i, j int) bool {
		i, j = idx[i], idx[j]
		return nums[i] < nums[j] || (nums[i] == nums[j] && i < j)
	})
	vis := make([]bool, n+2)
	for _, i := range idx {
		if !vis[i+1] {
			ans += int64(nums[i])
			vis[i], vis[i+2] = true, true
		}
	}
	return
}
```

#### TypeScript

```ts
function findScore(nums: number[]): number {
    const n = nums.length;
    const idx: number[] = new Array(n);
    for (let i = 0; i < n; ++i) {
        idx[i] = i;
    }
    idx.sort((i, j) => (nums[i] == nums[j] ? i - j : nums[i] - nums[j]));
    const vis: boolean[] = new Array(n + 2).fill(false);
    let ans = 0;
    for (const i of idx) {
        if (!vis[i + 1]) {
            ans += nums[i];
            vis[i] = true;
            vis[i + 2] = true;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
