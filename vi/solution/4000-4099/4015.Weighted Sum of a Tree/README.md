---
comments: true
difficulty: Medium
rating: 1534
source: Weekly Contest 514 Q2
tags:
    - Tree
    - Depth-First Search
    - Array
---

<!-- problem:start -->

# [4015. Weighted Sum of a Tree](https://leetcode.com/problems/weighted-sum-of-a-tree)

[中文文档](/solution/4000-4099/4015.Weighted%20Sum%20of%20a%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>parent</code> có độ dài <code>n</code>, biểu diễn một cây có gốc với các node được đánh nhãn từ 0 đến <code>n - 1</code>.</p>

<p>Cây được <strong>đặt gốc</strong> tại node 0, nên <code>parent[0] = -1</code>. Với mỗi node <code>i</code> thỏa mãn <code>1 &lt;= i &lt;= n - 1</code>, <code>parent[i]</code> là node cha của node <code>i</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>, trong đó <code>nums[i]</code> là giá trị của node <code>i</code>.</p>

<p>Trọng số của node <code>i</code> ở độ sâu <code>d</code> là <code>nums[i] * (h - d + 1)</code>, trong đó <code>h</code> là chiều cao của cây.</p>

<p>Trả về <strong>tổng</strong> trọng số của tất cả các node trong cây.</p>

<p><strong>Độ sâu</strong> của một node là số node trên đường đi từ gốc đến node đó, bao gồm cả hai đầu mút, trong đó node gốc có độ sâu là 1.</p>

<p><strong>Chiều cao</strong> của cây là độ sâu lớn nhất trong tất cả các node của cây.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4015.Weighted%20Sum%20of%20a%20Tree/images/t1.png" style="width: 200px; height: 190px;" />​​​​​​​</p>

<p><strong>Đầu vào:</strong> <span class="example-io">parent = [-1,0,0,0,2,2], nums = [5,2,3,1,4,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">37</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chiều cao của cây là 3.</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Node</th>
			<th style="border: 1px solid black;"><code>nums[i]</code></th>
			<th style="border: 1px solid black;">Độ sâu (<code>d</code>)</th>
			<th style="border: 1px solid black;">Trọng số</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>5 * (3 - 1 + 1) = 15</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>2 * (3 - 2 + 1) = 4</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>3 * (3 - 2 + 1) = 6</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>1 * (3 - 2 + 1) = 2</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;"><code>4 * (3 - 3 + 1) = 4</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;"><code>6 * (3 - 3 + 1) = 6</code></td>
		</tr>
	</tbody>
</table>

<p>Tổng trọng số của tất cả các node là <code>15 + 4 + 6 + 2 + 4 + 6 = 37</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4015.Weighted%20Sum%20of%20a%20Tree/images/t2.png" style="width: 250px; height: 56px;" />​​​​​​​​​​​​​​</p>

<p><strong>Đầu vào:</strong> <span class="example-io">parent = [-1,0,1,2], nums = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">20</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chiều cao của cây là 4.</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Node</th>
			<th style="border: 1px solid black;"><code>nums[i]</code></th>
			<th style="border: 1px solid black;">Độ sâu (<code>d</code>)</th>
			<th style="border: 1px solid black;">Trọng số</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>1 * (4 - 1 + 1) = 4</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>2 * (4 - 2 + 1) = 6</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;"><code>3 * (4 - 3 + 1) = 6</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;"><code>4 * (4 - 4 + 1) = 4</code></td>
		</tr>
	</tbody>
</table>

<p>Tổng trọng số của tất cả các node là <code>4 + 6 + 6 + 4 = 20</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>n == parent.length == nums.length</code></li>
	<li><code>parent[0] == -1</code></li>
	<li><code>0 &lt;= parent[i] &lt;= n - 1</code> với mọi <code>i</code> trong <code>[1, n - 1]</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li>Dữ liệu đầu vào được tạo sao cho mảng <code>parent</code> biểu diễn một cây hợp lệ có gốc là node 0.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Trọng số của node $i$ là $\textit{nums}[i]\times(h-d_i+1)$. Nếu tính chiều cao trước rồi tính tổng theo định nghĩa, ta cần duyệt cây hai lần và lưu độ sâu của các node.
>
> Tách tổng thành $h\sum\textit{nums}[i]+\sum\textit{nums}[i](1-d_i)$ cho phép một lần BFS cộng dồn hạng tử thứ hai trong quá trình duyệt; số layer ở cuối chính là $h$.
>
> Danh sách kề chỉ lưu các cạnh từ cha đến con, nên thứ tự duyệt theo level tương ứng với định nghĩa về độ sâu.

<!-- thinking:end -->

Trọng số của node $i$ là $\textit{nums}[i] \times (h - d_i + 1)$, trong đó $d_i$ là độ sâu của node $i$ và $h$ là chiều cao của cây. Vì vậy, tổng trọng số của tất cả các node là:

$$\sum_{i=0}^{n-1} \textit{nums}[i] \times (h - d_i + 1) = h \times \sum_{i=0}^{n-1} \textit{nums}[i] + \sum_{i=0}^{n-1} \textit{nums}[i] \times (1 - d_i)$$

Ta có thể dùng BFS để duyệt cây theo từng level. Trong quá trình duyệt, ta duy trì level hiện tại $d$ (node gốc ở level $1$) và cộng dồn $\textit{nums}[i] \times (1 - d)$ cho mỗi node. Sau khi duyệt xong, $d$ bằng chiều cao $h$ của cây; cộng thêm $h \times \sum \textit{nums}[i]$ sẽ cho đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def weightedSum(self, parent: list[int], nums: list[int]) -> int:
        n = len(nums)
        g = [[] for _ in range(n)]
        for i in range(1, n):
            g[parent[i]].append(i)
        ans = 0
        q = [0]
        d = 0
        while q:
            d += 1
            nq = []
            for i in q:
                ans += nums[i] * (1 - d)
                nq.extend(g[i])
            q = nq
        ans += d * sum(nums)
        return ans
```

#### Java

```java
class Solution {
    public long weightedSum(int[] parent, int[] nums) {
        int n = nums.length;

        List<Integer>[] g = new ArrayList[n];
        Arrays.setAll(g, e -> new ArrayList<>());

        for (int i = 1; i < n; i++) {
            g[parent[i]].add(i);
        }

        long ans = 0;

        List<Integer> q = new ArrayList<>();
        q.add(0);

        int d = 0;

        while (!q.isEmpty()) {
            d++;

            List<Integer> nq = new ArrayList<>();

            for (int i : q) {
                ans += (long) nums[i] * (1 - d);
                nq.addAll(g[i]);
            }

            q = nq;
        }

        long sum = 0;
        for (int x : nums) {
            sum += x;
        }

        ans += (long) d * sum;

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long weightedSum(vector<int>& parent, vector<int>& nums) {
        int n = nums.size();

        vector<vector<int>> g(n);

        for (int i = 1; i < n; i++) {
            g[parent[i]].push_back(i);
        }

        long long ans = 0;

        vector<int> q = {0};

        int d = 0;

        while (!q.empty()) {
            d++;

            vector<int> nq;

            for (int i : q) {
                ans += 1LL * nums[i] * (1 - d);
                for (int son : g[i]) {
                    nq.push_back(son);
                }
            }

            q = move(nq);
        }

        long long sum = 0;
        for (int x : nums) {
            sum += x;
        }

        ans += 1LL * d * sum;

        return ans;
    }
};
```

#### Go

```go
func weightedSum(parent []int, nums []int) int64 {
	n := len(nums)

	g := make([][]int, n)

	for i := 1; i < n; i++ {
		g[parent[i]] = append(g[parent[i]], i)
	}

	var ans int64

	q := []int{0}

	d := 0

	for len(q) > 0 {
		d++

		nq := make([]int, 0)

		for _, i := range q {
			ans += int64(nums[i]) * int64(1-d)

			for _, son := range g[i] {
				nq = append(nq, son)
			}
		}

		q = nq
	}

	var sum int64
	for _, x := range nums {
		sum += int64(x)
	}

	ans += int64(d) * sum

	return ans
}
```

#### TypeScript

```ts
function weightedSum(parent: number[], nums: number[]): number {
    const n = nums.length;

    const g: number[][] = Array.from({ length: n }, () => []);

    for (let i = 1; i < n; i++) {
        g[parent[i]].push(i);
    }

    let ans = 0;

    let q: number[] = [0];

    let d = 0;

    while (q.length > 0) {
        d++;

        const nq: number[] = [];

        for (const i of q) {
            ans += nums[i] * (1 - d);

            for (const son of g[i]) {
                nq.push(son);
            }
        }

        q = nq;
    }

    let sum = 0;
    for (const x of nums) {
        sum += x;
    }

    ans += d * sum;

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
