---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Binary Tree
---

<!-- problem:start -->

# [2445. Number of Nodes With Value One 🔒](https://leetcode.com/problems/number-of-nodes-with-value-one)

[中文文档](/solution/2400-2499/2445.Number%20of%20Nodes%20With%20Value%20One/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây liên thông <strong>vô hướng</strong> gồm <code>n</code> node được đánh số từ <code>1</code> đến <code>n</code> và có <code>n - 1</code> cạnh. Bạn được cung cấp số nguyên <code>n</code>. Node cha của node có nhãn <code>v</code> là node có nhãn <code>floor (v / 2)</code>. Gốc của cây là node có nhãn <code>1</code>.</p>

<ul>
	<li>Ví dụ, nếu <code>n = 7</code>, node có nhãn <code>3</code> có node có nhãn <code>floor(3 / 2) = 1</code> làm node cha, còn node có nhãn <code>7</code> có node có nhãn <code>floor(7 / 2) = 3</code> làm node cha.</li>
</ul>

<p>Bạn cũng được cung cấp một mảng số nguyên <code>queries</code>. Ban đầu, mỗi node đều có giá trị <code>0</code>. Với mỗi truy vấn <code>queries[i]</code>, hãy đảo giá trị của tất cả các node trong cây con của node có nhãn <code>queries[i]</code>.</p>

<p>Trả về <em>tổng số node có giá trị </em><code>1</code><em> sau khi <strong>xử lý tất cả các truy vấn</strong></em>.</p>

<p><strong>Lưu ý</strong>:</p>

<ul>
	<li>Đảo giá trị của một node có nghĩa là node có giá trị <code>0</code> trở thành <code>1</code> và ngược lại.</li>
	<li><code>floor(x)</code> tương đương với việc làm tròn <code>x</code> xuống số nguyên gần nhất.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2445.Number%20of%20Nodes%20With%20Value%20One/images/ex1.jpg" style="width: 600px; height: 297px;" />
<pre>
<strong>Đầu vào:</strong> n = 5 , queries = [1,2,5]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Sơ đồ phía trên thể hiện cấu trúc cây và trạng thái của cây sau khi thực hiện các truy vấn. Node màu xanh biểu thị giá trị 0, còn node màu đỏ biểu thị giá trị 1.
Sau khi xử lý các truy vấn, có ba node màu đỏ (các node có giá trị 1): 1, 3 và 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2445.Number%20of%20Nodes%20With%20Value%20One/images/ex2.jpg" style="width: 650px; height: 88px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, queries = [2,3,3]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Sơ đồ phía trên thể hiện cấu trúc cây và trạng thái của cây sau khi thực hiện các truy vấn. Node màu xanh biểu thị giá trị 0, còn node màu đỏ biểu thị giá trị 1.
Sau khi xử lý các truy vấn, có một node màu đỏ (node có giá trị 1): 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries[i] &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một truy vấn đảo giá trị của một node và toàn bộ cây con của nó trong một binary heap hoàn hảo. Các truy vấn xuất hiện số lần chẵn sẽ triệt tiêu nhau, nên chỉ cần giữ lại node được truy vấn số lần lẻ.
>
> Duyệt DFS từ mỗi root còn lại để đảo giá trị cây con, sau đó đếm số node có giá trị 1. Mỗi query root chỉ được xử lý một lần.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể mô phỏng quá trình xử lý từng truy vấn, tức là đảo giá trị của node được truy vấn và các node trong cây con của nó. Cuối cùng, ta đếm số node có giá trị 1.

Ở đây có một điểm có thể tối ưu. Nếu một node và cây con tương ứng của nó được truy vấn một số lần chẵn, giá trị của node sẽ không thay đổi. Vì vậy, ta có thể ghi nhận số lần truy vấn của mỗi node, rồi chỉ đảo giá trị của các node và cây con của chúng khi số lần truy vấn là lẻ.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfNodes(self, n: int, queries: List[int]) -> int:
        def dfs(i):
            if i > n:
                return
            tree[i] ^= 1
            dfs(i << 1)
            dfs(i << 1 | 1)

        tree = [0] * (n + 1)
        cnt = Counter(queries)
        for i, v in cnt.items():
            if v & 1:
                dfs(i)
        return sum(tree)
```

#### Java

```java
class Solution {
    private int[] tree;

    public int numberOfNodes(int n, int[] queries) {
        tree = new int[n + 1];
        int[] cnt = new int[n + 1];
        for (int v : queries) {
            ++cnt[v];
        }
        for (int i = 0; i < n + 1; ++i) {
            if (cnt[i] % 2 == 1) {
                dfs(i);
            }
        }
        int ans = 0;
        for (int i = 0; i < n + 1; ++i) {
            ans += tree[i];
        }
        return ans;
    }

    private void dfs(int i) {
        if (i >= tree.length) {
            return;
        }
        tree[i] ^= 1;
        dfs(i << 1);
        dfs(i << 1 | 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfNodes(int n, vector<int>& queries) {
        vector<int> tree(n + 1);
        vector<int> cnt(n + 1);
        for (int v : queries) ++cnt[v];
        function<void(int)> dfs = [&](int i) {
            if (i > n) return;
            tree[i] ^= 1;
            dfs(i << 1);
            dfs(i << 1 | 1);
        };
        for (int i = 0; i < n + 1; ++i) {
            if (cnt[i] & 1) {
                dfs(i);
            }
        }
        return accumulate(tree.begin(), tree.end(), 0);
    }
};
```

#### Go

```go
func numberOfNodes(n int, queries []int) int {
	tree := make([]int, n+1)
	cnt := make([]int, n+1)
	for _, v := range queries {
		cnt[v]++
	}
	var dfs func(int)
	dfs = func(i int) {
		if i > n {
			return
		}
		tree[i] ^= 1
		dfs(i << 1)
		dfs(i<<1 | 1)
	}
	for i, v := range cnt {
		if v%2 == 1 {
			dfs(i)
		}
	}
	ans := 0
	for _, v := range tree {
		ans += v
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
