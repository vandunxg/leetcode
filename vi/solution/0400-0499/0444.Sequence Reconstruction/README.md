---
comments: true
difficulty: Medium
tags:
    - Graph
    - Topological Sort
    - Array
    - Directed Acyclic Graph
---

<!-- problem:start -->

# [444. Sequence Reconstruction 🔒](https://leetcode.com/problems/sequence-reconstruction)

[中文文档](/solution/0400-0499/0444.Sequence%20Reconstruction/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> có độ dài <code>n</code>, trong đó <code>nums</code> là một hoán vị của các số nguyên trong khoảng <code>[1, n]</code>. Bạn cũng được cho mảng số nguyên 2D <code>sequences</code>, trong đó <code>sequences[i]</code> là một dãy con của <code>nums</code>.</p>

<p>Hãy kiểm tra xem <code>nums</code> có phải là <strong>supersequence</strong> ngắn nhất duy nhất hay không. <strong>Supersequence</strong> ngắn nhất là dãy có <strong>độ dài nhỏ nhất</strong> và chứa mọi <code>sequences[i]</code> dưới dạng dãy con. Với mảng <code>sequences</code> đã cho, có thể tồn tại nhiều <strong>supersequence</strong> hợp lệ.</p>

<ul>
	<li>Ví dụ, với <code>sequences = [[1,2],[1,3]]</code>, có hai <strong>supersequence</strong> ngắn nhất là <code>[1,2,3]</code> và <code>[1,3,2]</code>.</li>
	<li>Còn với <code>sequences = [[1,2],[1,3],[1,2,3]]</code>, <strong>supersequence</strong> ngắn nhất duy nhất có thể có là <code>[1,2,3]</code>. <code>[1,2,3,4]</code> cũng là một supersequence hợp lệ nhưng không ngắn nhất.</li>
</ul>

<p>Trả về <code>true</code><em> nếu </em><code>nums</code><em> là <strong>supersequence</strong> ngắn nhất duy nhất của </em><code>sequences</code><em>; nếu không thì trả về </em><code>false</code>.</p>

<p><strong>Dãy con</strong> là dãy có thể được tạo từ một dãy khác bằng cách xóa một số phần tử (hoặc không xóa phần tử nào) mà không làm thay đổi thứ tự của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3], sequences = [[1,2],[1,3]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Có hai supersequence có thể có: [1,2,3] và [1,3,2].
Dãy [1,2] là dãy con của cả hai: [<strong><u>1</u></strong>,<strong><u>2</u></strong>,3] và [<strong><u>1</u></strong>,3,<strong><u>2</u></strong>].
Dãy [1,3] là dãy con của cả hai: [<strong><u>1</u></strong>,2,<strong><u>3</u></strong>] và [<strong><u>1</u></strong>,<strong><u>3</u></strong>,2].
Vì nums không phải supersequence ngắn nhất duy nhất, ta trả về false.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3], sequences = [[1,2]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Supersequence ngắn nhất có thể là [1,2].
Dãy [1,2] là dãy con của nó: [<strong><u>1</u></strong>,<strong><u>2</u></strong>].
Vì nums không phải supersequence ngắn nhất, ta trả về false.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3], sequences = [[1,2],[1,3],[2,3]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Supersequence ngắn nhất có thể là [1,2,3].
Dãy [1,2] là dãy con của nó: [<strong><u>1</u></strong>,<strong><u>2</u></strong>,3].
Dãy [1,3] là dãy con của nó: [<strong><u>1</u></strong>,2,<strong><u>3</u></strong>].
Dãy [2,3] là dãy con của nó: [1,<strong><u>2</u></strong>,<strong><u>3</u></strong>].
Vì nums là supersequence ngắn nhất duy nhất, ta trả về true.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>nums</code> là hoán vị của tất cả số nguyên trong khoảng <code>[1, n]</code>.</li>
	<li><code>1 &lt;= sequences.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= sequences[i].length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= sum(sequences[i].length) &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= sequences[i][j] &lt;= n</code></li>
	<li>Các mảng trong <code>sequences</code> đều <strong>khác nhau</strong>.</li>
	<li><code>sequences[i]</code> là dãy con của <code>nums</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp topo

<!-- thinking:start -->

> **Tư duy**
>
> Cần xác định liệu $nums$ có phải supersequence ngắn nhất duy nhất của các dãy đã cho hay không. Nếu có nhiều thứ tự topo thì sẽ có nhiều supersequence.
>
> Chuyển mỗi cặp phần tử liên tiếp trong $\textit{seq}$ thành cạnh có hướng và ghi nhận bậc vào. Queue chỉ được có đúng một node bậc vào bằng 0 tại mỗi bước; nếu có hai node ứng viên thì thứ tự không duy nhất.
>
> Nếu queue rỗng khi kết thúc, mọi node đều đã được xác định thứ tự. Không cần so sánh từng vị trí với $nums$: nếu chỉ có một thứ tự thì thứ tự đó phải là $nums$.

<!-- thinking:end -->

Trước tiên, duyệt từng dãy con `seq`. Với mỗi cặp phần tử liên tiếp $a$ và $b$, tạo cạnh có hướng $a \to b$. Đồng thời, đếm bậc vào của từng node, rồi thêm tất cả node có bậc vào bằng $0$ vào queue.

Khi queue có đúng một node, lấy node đầu $i$ ra khỏi queue và đồ thị, đồng thời giảm bậc vào của tất cả node kề với $i$ đi $1$. Nếu bậc vào của node kề trở thành $0$, thêm node đó vào queue. Lặp lại thao tác trên cho đến khi queue không còn đúng một node. Lúc này, kiểm tra queue có rỗng không. Nếu queue không rỗng, có nhiều supersequence ngắn nhất nên trả về `false`; nếu queue rỗng, chỉ có một supersequence ngắn nhất nên trả về `true`.

Độ phức tạp thời gian và không gian đều là $O(n + m)$, trong đó $n$ và $m$ lần lượt là số node và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sequenceReconstruction(
        self, nums: List[int], sequences: List[List[int]]
    ) -> bool:
        n = len(nums)
        g = [[] for _ in range(n)]
        indeg = [0] * n
        for seq in sequences:
            for a, b in pairwise(seq):
                a, b = a - 1, b - 1
                g[a].append(b)
                indeg[b] += 1
        q = deque(i for i, x in enumerate(indeg) if x == 0)
        while len(q) == 1:
            i = q.popleft()
            for j in g[i]:
                indeg[j] -= 1
                if indeg[j] == 0:
                    q.append(j)
        return len(q) == 0
```

#### Java

```java
class Solution {
    public boolean sequenceReconstruction(int[] nums, List<List<Integer>> sequences) {
        int n = nums.length;
        int[] indeg = new int[n];
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var seq : sequences) {
            for (int i = 1; i < seq.size(); ++i) {
                int a = seq.get(i - 1) - 1, b = seq.get(i) - 1;
                g[a].add(b);
                ++indeg[b];
            }
        }
        Deque<Integer> q = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            if (indeg[i] == 0) {
                q.offer(i);
            }
        }
        while (q.size() == 1) {
            int i = q.poll();
            for (int j : g[i]) {
                if (--indeg[j] == 0) {
                    q.offer(j);
                }
            }
        }
        return q.isEmpty();
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool sequenceReconstruction(vector<int>& nums, vector<vector<int>>& sequences) {
        int n = nums.size();
        vector<int> indeg(n);
        vector<int> g[n];
        for (auto& seq : sequences) {
            for (int i = 1; i < seq.size(); ++i) {
                int a = seq[i - 1] - 1, b = seq[i] - 1;
                g[a].push_back(b);
                ++indeg[b];
            }
        }
        queue<int> q;
        for (int i = 0; i < n; ++i) {
            if (indeg[i] == 0) {
                q.push(i);
            }
        }
        while (q.size() == 1) {
            int i = q.front();
            q.pop();
            for (int j : g[i]) {
                if (--indeg[j] == 0) {
                    q.push(j);
                }
            }
        }
        return q.empty();
    }
};
```

#### Go

```go
func sequenceReconstruction(nums []int, sequences [][]int) bool {
	n := len(nums)
	indeg := make([]int, n)
	g := make([][]int, n)
	for _, seq := range sequences {
		for i, b := range seq[1:] {
			a := seq[i] - 1
			b -= 1
			g[a] = append(g[a], b)
			indeg[b]++
		}
	}
	q := []int{}
	for i, x := range indeg {
		if x == 0 {
			q = append(q, i)
		}
	}
	for len(q) == 1 {
		i := q[0]
		q = q[1:]
		for _, j := range g[i] {
			indeg[j]--
			if indeg[j] == 0 {
				q = append(q, j)
			}
		}
	}
	return len(q) == 0
}
```

#### TypeScript

```ts
function sequenceReconstruction(nums: number[], sequences: number[][]): boolean {
    const n = nums.length;
    const g: number[][] = Array.from({ length: n }, () => []);
    const indeg: number[] = Array(n).fill(0);
    for (const seq of sequences) {
        for (let i = 1; i < seq.length; ++i) {
            const [a, b] = [seq[i - 1] - 1, seq[i] - 1];
            g[a].push(b);
            ++indeg[b];
        }
    }
    const q: number[] = indeg.map((v, i) => (v === 0 ? i : -1)).filter(v => v !== -1);
    while (q.length === 1) {
        const i = q.pop()!;
        for (const j of g[i]) {
            if (--indeg[j] === 0) {
                q.push(j);
            }
        }
    }
    return q.length === 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
