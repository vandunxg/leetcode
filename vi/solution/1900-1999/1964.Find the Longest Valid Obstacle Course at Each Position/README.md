---
comments: true
difficulty: Hard
rating: 1933
source: Weekly Contest 253 Q4
tags:
    - Binary Indexed Tree
    - Array
    - Binary Search
    - Longest Increasing Subsequence
---

<!-- problem:start -->

# [1964. Find the Longest Valid Obstacle Course at Each Position](https://leetcode.com/problems/find-the-longest-valid-obstacle-course-at-each-position)

[中文文档](/solution/1900-1999/1964.Find%20the%20Longest%20Valid%20Obstacle%20Course%20at%20Each%20Position/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn muốn xây dựng một số đường đua chướng ngại vật. Cho một mảng số nguyên <code>obstacles</code> được đánh chỉ số từ <strong>0</strong>, có độ dài <code>n</code>, trong đó <code>obstacles[i]</code> mô tả chiều cao của chướng ngại vật thứ <code>i<sup>th</sup></code>.</p>

<p>Với mỗi chỉ số <code>i</code> từ <code>0</code> đến <code>n - 1</code> (<strong>bao gồm cả hai đầu</strong>), hãy tìm độ dài của <strong>đường đua chướng ngại vật dài nhất</strong> trong <code>obstacles</code> sao cho:</p>

<ul>
	<li>Bạn chọn một số bất kỳ chướng ngại vật từ <code>0</code> đến <code>i</code> <strong>bao gồm cả hai vị trí</strong>.</li>
	<li>Bạn phải đưa chướng ngại vật thứ <code>i<sup>th</sup></code> vào đường đua.</li>
	<li>Bạn phải đặt các chướng ngại vật đã chọn theo <strong>đúng thứ tự</strong> xuất hiện trong <code>obstacles</code>.</li>
	<li>Mỗi chướng ngại vật (trừ chướng ngại vật đầu tiên) phải <strong>cao hơn</strong> hoặc <strong>có cùng chiều cao</strong> với chướng ngại vật ngay trước nó.</li>
</ul>

<p>Trả về <em>một mảng</em> <code>ans</code> <em>có độ dài</em> <code>n</code>, <em>trong đó</em> <code>ans[i]</code> <em>là độ dài của <strong>đường đua chướng ngại vật dài nhất</strong> tại chỉ số</em> <code>i</code> <em>như mô tả ở trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> obstacles = [1,2,3,2]
<strong>Đầu ra:</strong> [1,2,3,3]
<strong>Giải thích:</strong> Đường đua chướng ngại vật hợp lệ dài nhất tại mỗi vị trí là:
- i = 0: [<u>1</u>], [1] có độ dài 1.
- i = 1: [<u>1</u>,<u>2</u>], [1,2] có độ dài 2.
- i = 2: [<u>1</u>,<u>2</u>,<u>3</u>], [1,2,3] có độ dài 3.
- i = 3: [<u>1</u>,<u>2</u>,3,<u>2</u>], [1,2,2] có độ dài 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> obstacles = [2,2,1]
<strong>Đầu ra:</strong> [1,2,1]
<strong>Giải thích: </strong>Đường đua chướng ngại vật hợp lệ dài nhất tại mỗi vị trí là:
- i = 0: [<u>2</u>], [2] có độ dài 1.
- i = 1: [<u>2</u>,<u>2</u>], [2,2] có độ dài 2.
- i = 2: [2,2,<u>1</u>], [1] có độ dài 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> obstacles = [3,1,5,6,4,2]
<strong>Đầu ra:</strong> [1,1,2,3,2,2]
<strong>Giải thích:</strong> Đường đua chướng ngại vật hợp lệ dài nhất tại mỗi vị trí là:
- i = 0: [<u>3</u>], [3] có độ dài 1.
- i = 1: [3,<u>1</u>], [1] có độ dài 1.
- i = 2: [<u>3</u>,1,<u>5</u>], [3,5] có độ dài 2. [1,5] cũng hợp lệ.
- i = 3: [<u>3</u>,1,<u>5</u>,<u>6</u>], [3,5,6] có độ dài 3. [1,5,6] cũng hợp lệ.
- i = 4: [<u>3</u>,1,5,6,<u>4</u>], [3,4] có độ dài 2. [1,4] cũng hợp lệ.
- i = 5: [3,<u>1</u>,5,6,4,<u>2</u>], [1,2] có độ dài 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == obstacles.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= obstacles[i] &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Indexed Tree (Fenwick Tree)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chướng ngại vật nối tiếp đường đua không giảm dài nhất ở bên trái. Duyệt tuyến tính trên tiền tố sẽ tốn $O(n^2)$.
>
> Sau khi nén chiều cao, Fenwick tree lưu độ dài tốt nhất trong các chiều cao $\le h$. Ta truy vấn giá trị lớn nhất trên tiền tố đó, cộng thêm một, rồi cập nhật lại.
>
> Mỗi chỉ số tốn $O(\log n)$.

<!-- thinking:end -->

Ta có thể sử dụng Binary Indexed Tree để duy trì một mảng chứa độ dài của các dãy con tăng dài nhất.

Sau đó, với mỗi chướng ngại vật, ta truy vấn Binary Indexed Tree để tìm độ dài của dãy con tăng dài nhất có giá trị nhỏ hơn hoặc bằng chướng ngại vật hiện tại, gọi độ dài đó là $l$. Khi đó, độ dài của dãy con tăng dài nhất kết thúc tại chướng ngại vật hiện tại là $l+1$. Ta thêm $l+1$ vào mảng kết quả và cập nhật giá trị $l+1$ vào Binary Indexed Tree.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó $n$ là số lượng chướng ngại vật.

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    __slots__ = ["n", "c"]

    def __init__(self, n: int):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x: int, v: int):
        while x <= self.n:
            self.c[x] = max(self.c[x], v)
            x += x & -x

    def query(self, x: int) -> int:
        s = 0
        while x:
            s = max(s, self.c[x])
            x -= x & -x
        return s


class Solution:
    def longestObstacleCourseAtEachPosition(self, obstacles: List[int]) -> List[int]:
        nums = sorted(set(obstacles))
        n = len(nums)
        tree = BinaryIndexedTree(n)
        ans = []
        for x in obstacles:
            i = bisect_left(nums, x) + 1
            ans.append(tree.query(i) + 1)
            tree.update(i, ans[-1])
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

    public void update(int x, int v) {
        while (x <= n) {
            c[x] = Math.max(c[x], v);
            x += x & -x;
        }
    }

    public int query(int x) {
        int s = 0;
        while (x > 0) {
            s = Math.max(s, c[x]);
            x -= x & -x;
        }
        return s;
    }
}

class Solution {
    public int[] longestObstacleCourseAtEachPosition(int[] obstacles) {
        int[] nums = obstacles.clone();
        Arrays.sort(nums);
        int n = nums.length;
        int[] ans = new int[n];
        BinaryIndexedTree tree = new BinaryIndexedTree(n);
        for (int k = 0; k < n; ++k) {
            int x = obstacles[k];
            int i = Arrays.binarySearch(nums, x) + 1;
            ans[k] = tree.query(i) + 1;
            tree.update(i, ans[k]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
private:
    int n;
    vector<int> c;

public:
    BinaryIndexedTree(int n) {
        this->n = n;
        c = vector<int>(n + 1);
    }

    void update(int x, int v) {
        while (x <= n) {
            c[x] = max(c[x], v);
            x += x & -x;
        }
    }

    int query(int x) {
        int s = 0;
        while (x > 0) {
            s = max(s, c[x]);
            x -= x & -x;
        }
        return s;
    }
};

class Solution {
public:
    vector<int> longestObstacleCourseAtEachPosition(vector<int>& obstacles) {
        vector<int> nums = obstacles;
        sort(nums.begin(), nums.end());
        int n = nums.size();
        vector<int> ans(n);
        BinaryIndexedTree tree(n);
        for (int k = 0; k < n; ++k) {
            int x = obstacles[k];
            auto it = lower_bound(nums.begin(), nums.end(), x);
            int i = distance(nums.begin(), it) + 1;
            ans[k] = tree.query(i) + 1;
            tree.update(i, ans[k]);
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

func NewBinaryIndexedTree(n int) *BinaryIndexedTree {
	return &BinaryIndexedTree{n, make([]int, n+1)}
}

func (bit *BinaryIndexedTree) update(x, v int) {
	for x <= bit.n {
		bit.c[x] = max(bit.c[x], v)
		x += x & -x
	}
}

func (bit *BinaryIndexedTree) query(x int) (s int) {
	for x > 0 {
		s = max(s, bit.c[x])
		x -= x & -x
	}
	return
}

func longestObstacleCourseAtEachPosition(obstacles []int) (ans []int) {
	nums := slices.Clone(obstacles)
	sort.Ints(nums)
	n := len(nums)
	tree := NewBinaryIndexedTree(n)
	for k, x := range obstacles {
		i := sort.SearchInts(nums, x) + 1
		ans = append(ans, tree.query(i)+1)
		tree.update(i, ans[k])
	}
	return
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

    update(x: number, v: number): void {
        while (x <= this.n) {
            this.c[x] = Math.max(this.c[x], v);
            x += x & -x;
        }
    }

    query(x: number): number {
        let s = 0;
        while (x > 0) {
            s = Math.max(s, this.c[x]);
            x -= x & -x;
        }
        return s;
    }
}

function longestObstacleCourseAtEachPosition(obstacles: number[]): number[] {
    const nums: number[] = [...obstacles];
    nums.sort((a, b) => a - b);
    const n: number = nums.length;
    const ans: number[] = [];
    const tree: BinaryIndexedTree = new BinaryIndexedTree(n);
    const search = (x: number): number => {
        let [l, r] = [0, n];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    for (let k = 0; k < n; ++k) {
        const i: number = search(obstacles[k]) + 1;
        ans[k] = tree.query(i) + 1;
        tree.update(i, ans[k]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
