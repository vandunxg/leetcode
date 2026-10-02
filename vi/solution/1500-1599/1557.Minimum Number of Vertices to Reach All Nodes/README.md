---
comments: true
difficulty: Medium
rating: 1512
source: Biweekly Contest 33 Q2
tags:
    - Graph
    - Directed Acyclic Graph
---

<!-- problem:start -->

# [1557. Minimum Number of Vertices to Reach All Nodes](https://leetcode.com/problems/minimum-number-of-vertices-to-reach-all-nodes)

[中文文档](/solution/1500-1599/1557.Minimum%20Number%20of%20Vertices%20to%20Reach%20All%20Nodes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <strong>&nbsp;đồ thị có hướng không chu kỳ</strong> với&nbsp;<code>n</code>&nbsp;đỉnh được đánh số từ&nbsp;<code>0</code>&nbsp;đến&nbsp;<code>n-1</code>, và mảng&nbsp;<code>edges</code>&nbsp;trong đó&nbsp;<code>edges[i] = [from<sub>i</sub>, to<sub>i</sub>]</code>&nbsp;biểu diễn cạnh có hướng từ node&nbsp;<code>from<sub>i</sub></code>&nbsp;đến node&nbsp;<code>to<sub>i</sub></code>.</p>

<p>Hãy tìm <em>tập đỉnh nhỏ nhất mà từ đó có thể đi tới mọi node trong đồ thị</em>. Đề bài đảm bảo nghiệm là duy nhất.</p>

<p>Lưu ý rằng có thể trả về các đỉnh theo bất kỳ thứ tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1557.Minimum%20Number%20of%20Vertices%20to%20Reach%20All%20Nodes/images/untitled22.png" style="width: 231px; height: 181px;" /></p>

<pre>
<strong>Đầu vào:</strong> n = 6, edges = [[0,1],[0,2],[2,5],[3,4],[4,2]]
<strong>Đầu ra:</strong> [0,3]
<b>Giải thích: </b>Không thể đi tới mọi node chỉ từ một đỉnh. Từ 0 ta đi được tới [0,1,2,5]. Từ 3 ta đi được tới [3,4,2,5]. Vì vậy ta trả về [0,3].</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1557.Minimum%20Number%20of%20Vertices%20to%20Reach%20All%20Nodes/images/untitled.png" style="width: 201px; height: 201px;" /></p>

<pre>
<strong>Đầu vào:</strong> n = 5, edges = [[0,1],[2,1],[3,1],[1,4],[2,4]]
<strong>Đầu ra:</strong> [0,2,3]
<strong>Giải thích: </strong>Các đỉnh 0, 3 và 2 không thể được đi tới từ node nào khác, nên ta phải đưa chúng vào kết quả. Ngoài ra, mỗi đỉnh trong số này đều có thể đi tới các node 1 và 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10^5</code></li>
	<li><code>1 &lt;= edges.length &lt;= min(10^5, n * (n - 1) / 2)</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= from<sub>i,</sub>&nbsp;to<sub>i</sub> &lt; n</code></li>
	<li>Mọi cặp <code>(from<sub>i</sub>, to<sub>i</sub>)</code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chọn ít đỉnh nhất sao cho mọi node đều có thể đi tới từ tập đã chọn trong một DAG. $n$ và số cạnh có thể đạt $10^5$, nên không thể liệt kê các tập con.
>
> Đỉnh có in-degree bằng $0$ không thể được đi tới từ đỉnh nào khác và bắt buộc phải chọn. Mọi đỉnh khác đều có cạnh đi vào, nên có thể đi tới từ một source nào đó trong DAG. Đáp án chính là tập các đỉnh có in-degree bằng không.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findSmallestSetOfVertices(self, n: int, edges: List[List[int]]) -> List[int]:
        cnt = Counter(t for _, t in edges)
        return [i for i in range(n) if cnt[i] == 0]
```

#### Java

```java
class Solution {
    public List<Integer> findSmallestSetOfVertices(int n, List<List<Integer>> edges) {
        var cnt = new int[n];
        for (var e : edges) {
            ++cnt[e.get(1)];
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            if (cnt[i] == 0) {
                ans.add(i);
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
    vector<int> findSmallestSetOfVertices(int n, vector<vector<int>>& edges) {
        vector<int> cnt(n);
        for (auto& e : edges) {
            ++cnt[e[1]];
        }
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            if (cnt[i] == 0) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findSmallestSetOfVertices(n int, edges [][]int) (ans []int) {
	cnt := make([]int, n)
	for _, e := range edges {
		cnt[e[1]]++
	}
	for i, c := range cnt {
		if c == 0 {
			ans = append(ans, i)
		}
	}
	return
}
```

#### TypeScript

```ts
function findSmallestSetOfVertices(n: number, edges: number[][]): number[] {
    const cnt: number[] = new Array(n).fill(0);
    for (const [_, t] of edges) {
        cnt[t]++;
    }
    const ans: number[] = [];
    for (let i = 0; i < n; ++i) {
        if (cnt[i] === 0) {
            ans.push(i);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_smallest_set_of_vertices(n: i32, edges: Vec<Vec<i32>>) -> Vec<i32> {
        let mut arr = vec![true; n as usize];
        edges.iter().for_each(|edge| {
            arr[edge[1] as usize] = false;
        });
        arr.iter()
            .enumerate()
            .filter_map(|(i, &v)| if v { Some(i as i32) } else { None })
            .collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
