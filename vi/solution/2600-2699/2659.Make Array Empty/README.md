---
comments: true
difficulty: Hard
rating: 2281
source: Biweekly Contest 103 Q4
tags:
    - Greedy
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Binary Search
    - Ordered Set
    - Sorting
---

<!-- problem:start -->

# [2659. Make Array Empty](https://leetcode.com/problems/make-array-empty)

[中文文档](/solution/2600-2699/2659.Make%20Array%20Empty/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> chứa các phần tử <strong>khác nhau</strong>, bạn có thể thực hiện các thao tác sau <strong>cho đến khi mảng rỗng</strong>:</p>

<ul>
    <li>Nếu phần tử đầu tiên có giá trị <strong>nhỏ nhất</strong>, xóa phần tử đó</li>
    <li>Nếu không, chuyển phần tử đầu tiên xuống <strong>cuối</strong> mảng.</li>
</ul>

<p>Trả về <em>số nguyên biểu thị số thao tác cần thực hiện để </em><code>nums</code><em> trở thành mảng rỗng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,-1]
<strong>Đầu ra:</strong> 5
</pre>

<table style="border: 2px solid black; border-collapse: collapse;">
    <thead>
        <tr>
            <th style="border: 2px solid black; padding: 5px;">Thao tác</th>
            <th style="border: 2px solid black; padding: 5px;">Mảng</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 2px solid black; padding: 5px;">1</td>
            <td style="border: 2px solid black; padding: 5px;">[4, -1, 3]</td>
        </tr>
        <tr>
            <td style="border: 2px solid black; padding: 5px;">2</td>
            <td style="border: 2px solid black; padding: 5px;">[-1, 3, 4]</td>
        </tr>
        <tr>
            <td style="border: 2px solid black; padding: 5px;">3</td>
            <td style="border: 2px solid black; padding: 5px;">[3, 4]</td>
        </tr>
        <tr>
            <td style="border: 2px solid black; padding: 5px;">4</td>
            <td style="border: 2px solid black; padding: 5px;">[4]</td>
        </tr>
        <tr>
            <td style="border: 2px solid black; padding: 5px;">5</td>
            <td style="border: 2px solid black; padding: 5px;">[]</td>
        </tr>
    </tbody>
</table>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,4,3]
<strong>Đầu ra:</strong> 5
</pre>

<table style="border: 2px solid black; border-collapse: collapse;">
    <thead>
        <tr>
            <th style="border: 2px solid black; padding: 5px;">Thao tác</th>
            <th style="border: 2px solid black; padding: 5px;">Mảng</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 2px solid black; padding: 5px;">1</td>
            <td style="border: 2px solid black; padding: 5px;">[2, 4, 3]</td>
        </tr>
        <tr>
            <td style="border: 2px solid black; padding: 5px;">2</td>
            <td style="border: 2px solid black; padding: 5px;">[4, 3]</td>
        </tr>
        <tr>
            <td style="border: 2px solid black; padding: 5px;">3</td>
            <td style="border: 2px solid black; padding: 5px;">[3, 4]</td>
        </tr>
        <tr>
            <td style="border: 2px solid black; padding: 5px;">4</td>
            <td style="border: 2px solid black; padding: 5px;">[4]</td>
        </tr>
        <tr>
            <td style="border: 2px solid black; padding: 5px;">5</td>
            <td style="border: 2px solid black; padding: 5px;">[]</td>
        </tr>
    </tbody>
</table>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3]
<strong>Đầu ra:</strong> 3
</pre>

<table style="border: 2px solid black; border-collapse: collapse;">
    <thead>
        <tr>
            <th style="border: 2px solid black; padding: 5px;">Thao tác</th>
            <th style="border: 2px solid black; padding: 5px;">Mảng</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 2px solid black; padding: 5px;">1</td>
            <td style="border: 2px solid black; padding: 5px;">[2, 3]</td>
        </tr>
        <tr>
            <td style="border: 2px solid black; padding: 5px;">2</td>
            <td style="border: 2px solid black; padding: 5px;">[3]</td>
        </tr>
        <tr>
            <td style="border: 2px solid black; padding: 5px;">3</td>
            <td style="border: 2px solid black; padding: 5px;">[]</td>
        </tr>
    </tbody>
</table>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>-10<sup>9&nbsp;</sup>&lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
    <li>Tất cả giá trị trong <code>nums</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Sorting + Fenwick Tree

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước hoặc xoay mảng sang trái, hoặc xóa phần tử nhỏ nhất hiện tại. Mô phỏng các lần xoay sẽ không đáp ứng được với $n \le 10^5$. Các lần xóa tuân theo thứ tự đã sắp xếp; chi phí là khoảng cách vòng tròn giữa hai phần tử nhỏ nhất liên tiếp, trừ đi các chỉ số đã xóa ở giữa.
>
> Lưu vị trí ban đầu, sắp xếp các giá trị và duy trì các chỉ số đã xóa trong một danh sách có thứ tự. Khoảng cách $j-i$ được giảm đi số phần tử đã xóa nằm giữa chúng; nếu $j$ quay vòng và nằm trước $i$, cộng thêm độ dài còn lại của vòng hiện tại.

<!-- thinking:end -->

Đầu tiên, ta dùng một hash table $pos$ để lưu vị trí của mỗi phần tử trong mảng $nums$. Sau đó, ta sắp xếp mảng $nums$. Đáp án ban đầu là vị trí của phần tử nhỏ nhất trong mảng $nums$ cộng 1, tức là $ans = pos[nums[0]] + 1$.

Tiếp theo, ta duyệt mảng $nums$ đã sắp xếp. Gọi chỉ số của hai phần tử liên tiếp $a$ và $b$ là $i = pos[a]$, $j = pos[b]$. Số thao tác cần để đưa phần tử thứ hai $b$ lên vị trí đầu tiên của mảng rồi xóa nó bằng khoảng cách giữa hai chỉ số, trừ đi số chỉ số đã bị xóa nằm giữa chúng; sau đó cộng kết quả vào đáp án. Ta có thể dùng Fenwick tree hoặc một danh sách có thứ tự để duy trì các chỉ số đã xóa nằm giữa hai chỉ số, nhờ đó tìm số chỉ số đã xóa giữa chúng trong $O(\log n)$. Lưu ý rằng nếu $i \gt j$, ta cần tăng thêm $n - k$ thao tác, trong đó $k$ là vị trí hiện tại.

Sau khi duyệt xong, trả về số thao tác $ans$.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$. Trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countOperationsToEmptyArray(self, nums: List[int]) -> int:
        pos = {x: i for i, x in enumerate(nums)}
        nums.sort()
        sl = SortedList()
        ans = pos[nums[0]] + 1
        n = len(nums)
        for k, (a, b) in enumerate(pairwise(nums)):
            i, j = pos[a], pos[b]
            d = j - i - sl.bisect(j) + sl.bisect(i)
            ans += d + (n - k) * int(i > j)
            sl.add(i)
        return ans
```

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
    }

    public void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += x & -x;
        }
    }

    public int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }
}

class Solution {
    public long countOperationsToEmptyArray(int[] nums) {
        int n = nums.length;
        Map<Integer, Integer> pos = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            pos.put(nums[i], i);
        }
        Arrays.sort(nums);
        long ans = pos.get(nums[0]) + 1;
        BinaryIndexedTree tree = new BinaryIndexedTree(n);
        for (int k = 0; k < n - 1; ++k) {
            int i = pos.get(nums[k]), j = pos.get(nums[k + 1]);
            long d = j - i - (tree.query(j + 1) - tree.query(i + 1));
            ans += d + (n - k) * (i > j ? 1 : 0);
            tree.update(i + 1, 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
public:
    BinaryIndexedTree(int _n)
        : n(_n)
        , c(_n + 1) {}

    void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += x & -x;
        }
    }

    int query(int x) {
        int s = 0;
        while (x) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }

private:
    int n;
    vector<int> c;
};

class Solution {
public:
    long long countOperationsToEmptyArray(vector<int>& nums) {
        unordered_map<int, int> pos;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            pos[nums[i]] = i;
        }
        sort(nums.begin(), nums.end());
        BinaryIndexedTree tree(n);
        long long ans = pos[nums[0]] + 1;
        for (int k = 0; k < n - 1; ++k) {
            int i = pos[nums[k]], j = pos[nums[k + 1]];
            long long d = j - i - (tree.query(j + 1) - tree.query(i + 1));
            ans += d + (n - k) * int(i > j);
            tree.update(i + 1, 1);
        }
        return ans;
    }
};
```

#### Go

```go
type BinaryIndexedTree struct {
	n int
	c []int
}

func newBinaryIndexedTree(n int) *BinaryIndexedTree {
	c := make([]int, n+1)
	return &BinaryIndexedTree{n, c}
}

func (this *BinaryIndexedTree) update(x, delta int) {
	for x <= this.n {
		this.c[x] += delta
		x += x & -x
	}
}

func (this *BinaryIndexedTree) query(x int) int {
	s := 0
	for x > 0 {
		s += this.c[x]
		x -= x & -x
	}
	return s
}

func countOperationsToEmptyArray(nums []int) int64 {
	n := len(nums)
	pos := map[int]int{}
	for i, x := range nums {
		pos[x] = i
	}
	sort.Ints(nums)
	tree := newBinaryIndexedTree(n)
	ans := pos[nums[0]] + 1
	for k := 0; k < n-1; k++ {
		i, j := pos[nums[k]], pos[nums[k+1]]
		d := j - i - (tree.query(j+1) - tree.query(i+1))
		if i > j {
			d += n - k
		}
		ans += d
		tree.update(i+1, 1)
	}
	return int64(ans)
}
```

#### TypeScript

```ts
class BinaryIndexedTree {
    private n: number;
    private c: number[];

    constructor(n: number) {
        this.n = n;
        this.c = Array(n + 1).fill(0);
    }

    public update(x: number, v: number): void {
        while (x <= this.n) {
            this.c[x] += v;
            x += x & -x;
        }
    }

    public query(x: number): number {
        let s = 0;
        while (x > 0) {
            s += this.c[x];
            x -= x & -x;
        }
        return s;
    }
}

function countOperationsToEmptyArray(nums: number[]): number {
    const pos: Map<number, number> = new Map();
    const n = nums.length;
    for (let i = 0; i < n; ++i) {
        pos.set(nums[i], i);
    }
    nums.sort((a, b) => a - b);
    const tree = new BinaryIndexedTree(n);
    let ans = pos.get(nums[0])! + 1;
    for (let k = 0; k < n - 1; ++k) {
        const i = pos.get(nums[k])!;
        const j = pos.get(nums[k + 1])!;
        let d = j - i - (tree.query(j + 1) - tree.query(i + 1));
        if (i > j) {
            d += n - k;
        }
        ans += d;
        tree.update(i + 1, 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 truy vấn số lượng phần tử đã xóa trong một ordered set. Fenwick tree cũng cung cấp các tổng tiền tố tương tự: sau khi dịch chỉ số lên một, `query(j+1)-query(i+1)` là số phần tử đã xóa, còn update là phép cộng tại một điểm. Logic quay vòng không thay đổi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x, delta):
        while x <= self.n:
            self.c[x] += delta
            x += x & -x

    def query(self, x):
        s = 0
        while x:
            s += self.c[x]
            x -= x & -x
        return s


class Solution:
    def countOperationsToEmptyArray(self, nums: List[int]) -> int:
        pos = {x: i for i, x in enumerate(nums)}
        nums.sort()
        ans = pos[nums[0]] + 1
        n = len(nums)
        tree = BinaryIndexedTree(n)
        for k, (a, b) in enumerate(pairwise(nums)):
            i, j = pos[a], pos[b]
            d = j - i - tree.query(j + 1) + tree.query(i + 1)
            ans += d + (n - k) * int(i > j)
            tree.update(i + 1, 1)
        return ans
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
