---
comments: true
difficulty: Hard
rating: 2201
source: Weekly Contest 480 Q4
tags:
    - Segment Tree
    - String
---

<!-- problem:start -->

# [3777. Minimum Deletions to Make Alternating Substring](https://leetcode.com/problems/minimum-deletions-to-make-alternating-substring)

[中文文档](/solution/3700-3799/3777.Minimum%20Deletions%20to%20Make%20Alternating%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> có độ dài <code>n</code>, chỉ gồm các ký tự <code>&#39;A&#39;</code> và <code>&#39;B&#39;</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên 2D <code>queries</code> có độ dài <code>q</code>, trong đó mỗi <code>queries[i]</code> là một trong các dạng sau:</p>

<ul>
	<li><code>[1, j]</code>: <strong>Đảo</strong> ký tự tại chỉ số <code>j</code> của <code>s</code>, tức là <code>&#39;A&#39;</code> đổi thành <code>&#39;B&#39;</code> và ngược lại. Thao tác này thay đổi <code>s</code> và ảnh hưởng đến các truy vấn tiếp theo.</li>
	<li><code>[2, l, r]</code>: <strong>Tính</strong> số ký tự <strong>tối thiểu</strong> cần xóa để biến <strong>chuỗi con</strong> <code>s[l..r]</code> thành chuỗi <strong>xen kẽ</strong>. Thao tác này không thay đổi <code>s</code>; độ dài của <code>s</code> vẫn là <code>n</code>.</li>
</ul>

<p>Một <strong><span data-keyword="substring-nonempty">chuỗi con</span></strong> là <strong>xen kẽ</strong> nếu không có hai ký tự <strong>liền kề</strong> nào <strong>bằng nhau</strong>. Chuỗi con có độ dài 1 luôn xen kẽ.</p>

<p>Trả về một mảng số nguyên <code>answer</code>, trong đó <code>answer[i]</code> là kết quả của truy vấn thứ <code>i<sup>th</sup></code> có dạng <code>[2, l, r]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;ABA&quot;, queries = [[2,1,2],[1,1],[2,0,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,2]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th align="center" style="border: 1px solid black;"><code><strong>i</strong></code></th>
			<th align="center" style="border: 1px solid black;"><code><strong>queries[i]</strong></code></th>
			<th align="center" style="border: 1px solid black;"><code><strong>j</strong></code></th>
			<th align="center" style="border: 1px solid black;"><code><strong>l</strong></code></th>
			<th align="center" style="border: 1px solid black;"><code><strong>r</strong></code></th>
			<th align="center" style="border: 1px solid black;"><strong><code>s</code> trước truy vấn</strong></th>
			<th align="center" style="border: 1px solid black;"><code><strong>s[l..r]</strong></code></th>
			<th align="center" style="border: 1px solid black;"><strong>Kết quả</strong></th>
			<th align="center" style="border: 1px solid black;"><strong>Đáp án</strong></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">[2, 1, 2]</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;"><code>&quot;ABA&quot;</code></td>
			<td align="center" style="border: 1px solid black;"><code>&quot;BA&quot;</code></td>
			<td align="center" style="border: 1px solid black;">Đã xen kẽ</td>
			<td align="center" style="border: 1px solid black;">0</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">[1, 1]</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;"><code>&quot;ABA&quot;</code></td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">Đảo <code>s[1]</code> từ <code>&#39;B&#39;</code> thành <code>&#39;A&#39;</code></td>
			<td align="center" style="border: 1px solid black;">-</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">[2, 0, 2]</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;"><code>&quot;AAA&quot;</code></td>
			<td align="center" style="border: 1px solid black;"><code>&quot;AAA&quot;</code></td>
			<td align="center" style="border: 1px solid black;">Xóa hai ký tự <code>&#39;A&#39;</code> bất kỳ để được <code>&quot;A&quot;</code></td>
			<td align="center" style="border: 1px solid black;">2</td>
		</tr>
	</tbody>
</table>

<p>Do đó, đáp án là <code>[0, 2]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;ABB&quot;, queries = [[2,0,2],[1,2],[2,0,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,0]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th align="center" style="border: 1px solid black;"><code><strong>i</strong></code></th>
			<th align="center" style="border: 1px solid black;"><code><strong>queries[i]</strong></code></th>
			<th align="center" style="border: 1px solid black;"><code><strong>j</strong></code></th>
			<th align="center" style="border: 1px solid black;"><code><strong>l</strong></code></th>
			<th align="center" style="border: 1px solid black;"><code><strong>r</strong></code></th>
			<th align="center" style="border: 1px solid black;"><strong><code>s</code> trước truy vấn</strong></th>
			<th align="center" style="border: 1px solid black;"><code><strong>s[l..r]</strong></code></th>
			<th align="center" style="border: 1px solid black;"><strong>Kết quả</strong></th>
			<th align="center" style="border: 1px solid black;"><strong>Đáp án</strong></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">[2, 0, 2]</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;"><code>&quot;ABB&quot;</code></td>
			<td align="center" style="border: 1px solid black;"><code>&quot;ABB&quot;</code></td>
			<td align="center" style="border: 1px solid black;">Xóa một ký tự <code>&#39;B&#39;</code> để được <code>&quot;AB&quot;</code></td>
			<td align="center" style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">[1, 2]</td>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;"><code>&quot;ABB&quot;</code></td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">Đảo <code>s[2]</code> từ <code>&#39;B&#39;</code> thành <code>&#39;A&#39;</code></td>
			<td align="center" style="border: 1px solid black;">-</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">[2, 0, 2]</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;"><code>&quot;ABA&quot;</code></td>
			<td align="center" style="border: 1px solid black;"><code>&quot;ABA&quot;</code></td>
			<td align="center" style="border: 1px solid black;">Đã xen kẽ</td>
			<td align="center" style="border: 1px solid black;">0</td>
		</tr>
	</tbody>
</table>

<p>Do đó, đáp án là <code>[1, 0]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;BABA&quot;, queries = [[2,0,3],[1,1],[2,1,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,1]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th align="center" style="border: 1px solid black;"><code><strong>i</strong></code></th>
			<th align="center" style="border: 1px solid black;"><code><strong>queries[i]</strong></code></th>
			<th align="center" style="border: 1px solid black;"><code><strong>j</strong></code></th>
			<th align="center" style="border: 1px solid black;"><code><strong>l</strong></code></th>
			<th align="center" style="border: 1px solid black;"><code><strong>r</strong></code></th>
			<th align="center" style="border: 1px solid black;"><strong><code>s</code> trước truy vấn</strong></th>
			<th align="center" style="border: 1px solid black;"><code><strong>s[l..r]</strong></code></th>
			<th align="center" style="border: 1px solid black;"><strong>Kết quả</strong></th>
			<th align="center" style="border: 1px solid black;"><strong>Đáp án</strong></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">[2, 0, 3]</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">3</td>
			<td align="center" style="border: 1px solid black;"><code>&quot;BABA&quot;</code></td>
			<td align="center" style="border: 1px solid black;"><code>&quot;BABA&quot;</code></td>
			<td align="center" style="border: 1px solid black;">Đã xen kẽ</td>
			<td align="center" style="border: 1px solid black;">0</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">[1, 1]</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;"><code>&quot;BABA&quot;</code></td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">Đảo <code>s[1]</code> từ <code>&#39;A&#39;</code> thành <code>&#39;B&#39;</code></td>
			<td align="center" style="border: 1px solid black;">-</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">[2, 1, 3]</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">3</td>
			<td align="center" style="border: 1px solid black;"><code>&quot;BBBA&quot;</code></td>
			<td align="center" style="border: 1px solid black;"><code>&quot;BBA&quot;</code></td>
			<td align="center" style="border: 1px solid black;">Xóa một ký tự <code>&#39;B&#39;</code> để được <code>&quot;BA&quot;</code></td>
			<td align="center" style="border: 1px solid black;">1</td>
		</tr>
	</tbody>
</table>

<p>Do đó, đáp án là <code>[0, 1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;A&#39;</code> hoặc <code>&#39;B&#39;</code>.</li>
	<li><code>1 &lt;= q == queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length == 2</code> hoặc <code>3</code>
	<ul>
		<li><code>queries[i] == [1, j]</code> hoặc,</li>
		<li><code>queries[i] == [2, l, r]</code></li>
		<li><code>0 &lt;= j &lt;= n - 1</code></li>
		<li><code>0 &lt;= l &lt;= r &lt;= n - 1</code></li>
	</ul>
	</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi xen kẽ không cho phép hai ký tự liền kề giống nhau, nên số lần xóa ít nhất bằng số cặp ký tự liền kề giống nhau trong đoạn. Đánh dấu các cặp đó bằng $1$ sẽ biến truy vấn thành tính tổng trên đoạn; một lần đảo chỉ thay đổi tối đa hai cờ kề nhau, nên có thể cập nhật bằng Fenwick tree.

<!-- thinking:end -->

Ta có thể chuyển chuỗi $s$ thành một mảng $\textit{nums}$ có độ dài $n$, trong đó $\textit{nums}[0] = 0$, và với $1 \leq i < n$, nếu $s[i] = s[i-1]$ thì $\textit{nums}[i] = 1$, ngược lại $\textit{nums}[i] = 0$. Như vậy, $\textit{nums}[i]$ biểu thị xem tại chỉ số $i$ có hai ký tự liền kề giống nhau hay không. Khi đó, việc tính số ký tự tối thiểu cần xóa để biến chuỗi con $s[l..r]$ thành chuỗi xen kẽ trong đoạn $[l, r]$ tương đương với việc tính tổng các phần tử của mảng $\textit{nums}$ trên đoạn $[l+1, r]$.

Để xử lý các truy vấn hiệu quả, ta có thể dùng Binary Indexed Tree để duy trì tổng tiền tố của mảng $\textit{nums}$. Với truy vấn dạng $[1, j]$, ta cần đảo $\textit{nums}[j]$ và $\textit{nums}[j+1]$ (nếu $j+1 < n$), rồi cập nhật Binary Indexed Tree. Với truy vấn dạng $[2, l, r]$, ta có thể nhanh chóng tính tổng các phần tử trên đoạn $[l+1, r]$ thông qua Binary Indexed Tree.

Độ phức tạp thời gian là $O((n + q) \log n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$, còn $q$ là số truy vấn.

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    __slots__ = "n", "c"

    def __init__(self, n: int):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x: int, delta: int) -> None:
        while x <= self.n:
            self.c[x] += delta
            x += x & -x

    def query(self, x: int) -> int:
        s = 0
        while x:
            s += self.c[x]
            x -= x & -x
        return s


class Solution:
    def minDeletions(self, s: str, queries: List[List[int]]) -> List[int]:
        n = len(s)
        nums = [0] * n
        bit = BinaryIndexedTree(n)
        for i in range(1, n):
            nums[i] = int(s[i] == s[i - 1])
            if nums[i]:
                bit.update(i + 1, 1)
        ans = []
        for q in queries:
            if q[0] == 1:
                j = q[1]
                delta = (nums[j] ^ 1) - nums[j]
                nums[j] ^= 1
                bit.update(j + 1, delta)
                if j + 1 < n:
                    delta = (nums[j + 1] ^ 1) - nums[j + 1]
                    nums[j + 1] ^= 1
                    bit.update(j + 2, delta)
            else:
                _, l, r = q
                ans.append(bit.query(r + 1) - bit.query(l + 1))
        return ans
```

#### Java

```java
class BinaryIndexedTree {
    int n;
    int[] c;

    BinaryIndexedTree(int n) {
        this.n = n;
        this.c = new int[n + 1];
    }

    void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += x & -x;
        }
    }

    int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }
}

class Solution {
    public int[] minDeletions(String s, int[][] queries) {
        int n = s.length();
        int[] nums = new int[n];
        BinaryIndexedTree bit = new BinaryIndexedTree(n);

        for (int i = 1; i < n; i++) {
            nums[i] = (s.charAt(i) == s.charAt(i - 1)) ? 1 : 0;
            if (nums[i] == 1) {
                bit.update(i + 1, 1);
            }
        }

        int cnt = 0;
        for (int[] q : queries) {
            if (q[0] == 2) {
                cnt++;
            }
        }

        int[] ans = new int[cnt];
        int idx = 0;

        for (int[] q : queries) {
            if (q[0] == 1) {
                int j = q[1];

                int delta = (nums[j] ^ 1) - nums[j];
                nums[j] ^= 1;
                bit.update(j + 1, delta);

                if (j + 1 < n) {
                    delta = (nums[j + 1] ^ 1) - nums[j + 1];
                    nums[j + 1] ^= 1;
                    bit.update(j + 2, delta);
                }
            } else {
                int l = q[1];
                int r = q[2];
                ans[idx++] = bit.query(r + 1) - bit.query(l + 1);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
public:
    int n;
    vector<int> c;

    BinaryIndexedTree(int n)
        : n(n)
        , c(n + 1, 0) {}

    void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += x & -x;
        }
    }

    int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }
};

class Solution {
public:
    vector<int> minDeletions(string s, vector<vector<int>>& queries) {
        int n = s.size();
        vector<int> nums(n, 0);
        BinaryIndexedTree bit(n);

        for (int i = 1; i < n; i++) {
            nums[i] = (s[i] == s[i - 1]);
            if (nums[i]) {
                bit.update(i + 1, 1);
            }
        }

        vector<int> ans;

        for (auto& q : queries) {
            if (q[0] == 1) {
                int j = q[1];

                int delta = (nums[j] ^ 1) - nums[j];
                nums[j] ^= 1;
                bit.update(j + 1, delta);

                if (j + 1 < n) {
                    delta = (nums[j + 1] ^ 1) - nums[j + 1];
                    nums[j + 1] ^= 1;
                    bit.update(j + 2, delta);
                }
            } else {
                int l = q[1];
                int r = q[2];
                ans.push_back(bit.query(r + 1) - bit.query(l + 1));
            }
        }
        return ans;
    }
};
```

#### Go

```go
type binaryIndexedTree struct {
	n int
	c []int
}

func newBinaryIndexedTree(n int) *binaryIndexedTree {
	return &binaryIndexedTree{
		n: n,
		c: make([]int, n+1),
	}
}

func (bit *binaryIndexedTree) update(x, delta int) {
	for x <= bit.n {
		bit.c[x] += delta
		x += x & -x
	}
}

func (bit *binaryIndexedTree) query(x int) int {
	s := 0
	for x > 0 {
		s += bit.c[x]
		x -= x & -x
	}
	return s
}

func minDeletions(s string, queries [][]int) []int {
	n := len(s)
	nums := make([]int, n)
	bit := newBinaryIndexedTree(n)

	for i := 1; i < n; i++ {
		if s[i] == s[i-1] {
			nums[i] = 1
			bit.update(i+1, 1)
		}
	}

	ans := make([]int, 0)

	for _, q := range queries {
		if q[0] == 1 {
			j := q[1]

			delta := (nums[j] ^ 1 - nums[j])
			nums[j] ^= 1
			bit.update(j+1, delta)

			if j+1 < n {
				delta = (nums[j+1] ^ 1 - nums[j+1])
				nums[j+1] ^= 1
				bit.update(j+2, delta)
			}
		} else {
			l, r := q[1], q[2]
			ans = append(ans, bit.query(r+1)-bit.query(l+1))
		}
	}

	return ans
}
```

#### TypeScript

```ts
class BinaryIndexedTree {
    n: number;
    c: number[];

    constructor(n: number) {
        this.n = n;
        this.c = Array(n + 1).fill(0);
    }

    update(x: number, delta: number): void {
        while (x <= this.n) {
            this.c[x] += delta;
            x += x & -x;
        }
    }

    query(x: number): number {
        let s = 0;
        while (x > 0) {
            s += this.c[x];
            x -= x & -x;
        }
        return s;
    }
}

function minDeletions(s: string, queries: number[][]): number[] {
    const n = s.length;
    const nums: number[] = Array(n).fill(0);
    const bit = new BinaryIndexedTree(n);

    for (let i = 1; i < n; i++) {
        if (s[i] === s[i - 1]) {
            nums[i] = 1;
            bit.update(i + 1, 1);
        }
    }

    const ans: number[] = [];

    for (const q of queries) {
        if (q[0] === 1) {
            const j = q[1];

            let delta = (nums[j] ^ 1) - nums[j];
            nums[j] ^= 1;
            bit.update(j + 1, delta);

            if (j + 1 < n) {
                delta = (nums[j + 1] ^ 1) - nums[j + 1];
                nums[j + 1] ^= 1;
                bit.update(j + 2, delta);
            }
        } else {
            const l = q[1],
                r = q[2];
            ans.push(bit.query(r + 1) - bit.query(l + 1));
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
