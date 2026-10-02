---
comments: true
difficulty: Hard
rating: 2336
source: Weekly Contest 196 Q4
tags:
    - Greedy
    - Binary Indexed Tree
    - Segment Tree
    - String
---

<!-- problem:start -->

# [1505. Minimum Possible Integer After at Most K Adjacent Swaps On Digits](https://leetcode.com/problems/minimum-possible-integer-after-at-most-k-adjacent-swaps-on-digits)

[中文文档](/solution/1500-1599/1505.Minimum%20Possible%20Integer%20After%20at%20Most%20K%20Adjacent%20Swaps%20On%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>num</code> biểu diễn <strong>các chữ số</strong> của một số nguyên rất lớn và một số nguyên <code>k</code>. Bạn được phép đổi chỗ hai chữ số liền kề của số nguyên <strong>nhiều nhất</strong> <code>k</code> lần.</p>

<p>Hãy trả về <em>số nguyên nhỏ nhất có thể nhận được dưới dạng một chuỗi</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1505.Minimum%20Possible%20Integer%20After%20at%20Most%20K%20Adjacent%20Swaps%20On%20Digits/images/q4_1.jpg" style="width: 500px; height: 40px;" />
<pre>
<strong>Đầu vào:</strong> num = &quot;4321&quot;, k = 4
<strong>Đầu ra:</strong> &quot;1342&quot;
<strong>Giải thích:</strong> Các bước để nhận được số nguyên nhỏ nhất từ 4321 với 4 lần đổi chỗ liền kề được minh họa như trên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;100&quot;, k = 1
<strong>Đầu ra:</strong> &quot;010&quot;
<strong>Giải thích:</strong> Đầu ra có thể chứa các số 0 ở đầu, nhưng đầu vào được đảm bảo không có số 0 ở đầu.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;36789&quot;, k = 1000
<strong>Đầu ra:</strong> &quot;36789&quot;
<strong>Giải thích:</strong> Ta có thể giữ nguyên số mà không thực hiện lần đổi chỗ nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>num</code> chỉ gồm các <strong>chữ số</strong> và không chứa <strong>số 0 ở đầu</strong>.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể thực hiện nhiều nhất $k$ lần đổi chỗ liền kề và muốn số nhận được nhỏ nhất theo thứ tự từ điển. $k$ có thể lên tới $10^9$ trong khi $n\le 3\times 10^4$, nên không thể mô phỏng từng lần đổi chỗ, cũng không thể dùng phép quét tuyến tính để di chuyển một chữ số ở mỗi vị trí.
>
> Xây dựng đáp án từ trái sang phải, chữ số ở vị trí hiện tại phải là chữ số nhỏ nhất vẫn có thể di chuyển đến đó với ngân sách còn lại. Di chuyển một chỉ số ban đầu chưa được lấy $j$ đến vị trí $i$ tốn số lượng chữ số chưa được lấy nằm giữa chúng.
>
> Lưu các chỉ số ban đầu của các chữ số $0$–$9$ trong các deque, đồng thời dùng Fenwick tree để đánh dấu những vị trí ban đầu đã được lấy. Với mỗi chữ số ứng viên, tree tính khoảng cách thực tế trong $O(\log n)$; nếu phù hợp với $k$ còn lại, ta lấy chữ số đó và cập nhật tree. Mỗi vị trí chỉ xét một số lượng chữ số cố định, nên tổng thời gian là $O(n\log n)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n):
        self.n = n
        self.c = [0] * (n + 1)

    @staticmethod
    def lowbit(x):
        return x & -x

    def update(self, x, delta):
        while x <= self.n:
            self.c[x] += delta
            x += BinaryIndexedTree.lowbit(x)

    def query(self, x):
        s = 0
        while x:
            s += self.c[x]
            x -= BinaryIndexedTree.lowbit(x)
        return s


class Solution:
    def minInteger(self, num: str, k: int) -> str:
        pos = defaultdict(deque)
        for i, v in enumerate(num, 1):
            pos[int(v)].append(i)
        ans = []
        n = len(num)
        tree = BinaryIndexedTree(n)
        for i in range(1, n + 1):
            for v in range(10):
                q = pos[v]
                if q:
                    j = q[0]
                    dist = tree.query(n) - tree.query(j) + j - i
                    if dist <= k:
                        k -= dist
                        q.popleft()
                        ans.append(str(v))
                        tree.update(j, 1)
                        break
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String minInteger(String num, int k) {
        Queue<Integer>[] pos = new Queue[10];
        for (int i = 0; i < 10; ++i) {
            pos[i] = new ArrayDeque<>();
        }
        int n = num.length();
        for (int i = 0; i < n; ++i) {
            pos[num.charAt(i) - '0'].offer(i + 1);
        }
        StringBuilder ans = new StringBuilder();
        BinaryIndexedTree tree = new BinaryIndexedTree(n);
        for (int i = 1; i <= n; ++i) {
            for (int v = 0; v < 10; ++v) {
                if (!pos[v].isEmpty()) {
                    Queue<Integer> q = pos[v];
                    int j = q.peek();
                    int dist = tree.query(n) - tree.query(j) + j - i;
                    if (dist <= k) {
                        k -= dist;
                        q.poll();
                        ans.append(v);
                        tree.update(j, 1);
                        break;
                    }
                }
            }
        }
        return ans.toString();
    }
}

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
            x += lowbit(x);
        }
    }

    public int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= lowbit(x);
        }
        return s;
    }

    public static int lowbit(int x) {
        return x & -x;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
public:
    int n;
    vector<int> c;

    BinaryIndexedTree(int _n)
        : n(_n)
        , c(_n + 1) {}

    void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += lowbit(x);
        }
    }

    int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= lowbit(x);
        }
        return s;
    }

    int lowbit(int x) {
        return x & -x;
    }
};

class Solution {
public:
    string minInteger(string num, int k) {
        vector<queue<int>> pos(10);
        int n = num.size();
        for (int i = 0; i < n; ++i) pos[num[i] - '0'].push(i + 1);
        BinaryIndexedTree* tree = new BinaryIndexedTree(n);
        string ans = "";
        for (int i = 1; i <= n; ++i) {
            for (int v = 0; v < 10; ++v) {
                auto& q = pos[v];
                if (!q.empty()) {
                    int j = q.front();
                    int dist = tree->query(n) - tree->query(j) + j - i;
                    if (dist <= k) {
                        k -= dist;
                        q.pop();
                        ans += (v + '0');
                        tree->update(j, 1);
                        break;
                    }
                }
            }
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

func (this *BinaryIndexedTree) lowbit(x int) int {
    return x & -x
}

func (this *BinaryIndexedTree) update(x, delta int) {
    for x <= this.n {
        this.c[x] += delta
        x += this.lowbit(x)
    }
}

func (this *BinaryIndexedTree) query(x int) int {
    s := 0
    for x > 0 {
        s += this.c[x]
        x -= this.lowbit(x)
    }
    return s
}

func minInteger(num string, k int) string {
    pos := make([][]int, 10)
    for i, c := range num {
        pos[c-'0'] = append(pos[c-'0'], i+1)
    }
    n := len(num)
    tree := newBinaryIndexedTree(n)
    var ans strings.Builder
    for i := 1; i <= n; i++ {
        for v := 0; v < 10; v++ {
            if len(pos[v]) > 0 {
                j := pos[v][0]
                dist := tree.query(n) - tree.query(j) + j - i
                if dist <= k {
                    k -= dist
                    pos[v] = pos[v][1:]
                    ans.WriteByte(byte(v + '0'))
                    tree.update(j, 1)
                    break
                }
            }
        }
    }
    return ans.String()
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
