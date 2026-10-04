---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Simulation
---

<!-- problem:start -->

# [3237. Alt and Tab Simulation 🔒](https://leetcode.com/problems/alt-and-tab-simulation)

[中文文档](/solution/3200-3299/3237.Alt%20and%20Tab%20Simulation/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> cửa sổ đang mở, được đánh số từ <code>1</code> đến <code>n</code>; ta muốn mô phỏng thao tác alt + tab để chuyển đổi giữa các cửa sổ.</p>

<p>Bạn được cho một mảng <code>windows</code> chứa thứ tự ban đầu của các cửa sổ (phần tử đầu tiên ở trên cùng và phần tử cuối cùng ở dưới cùng).</p>

<p>Bạn cũng được cho một mảng <code>queries</code>, trong đó với mỗi truy vấn, cửa sổ <code>queries[i]</code> được đưa lên đầu.</p>

<p>Hãy trả về trạng thái cuối cùng của mảng <code>windows</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">windows = [1,2,3], queries = [3,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,3,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dưới đây là mảng cửa sổ sau mỗi truy vấn:</p>

<ul>
	<li>Thứ tự ban đầu: <code>[1,2,3]</code></li>
	<li>Sau truy vấn đầu tiên: <code>[<u><strong>3</strong></u>,1,2]</code></li>
	<li>Sau truy vấn thứ hai: <code>[<u><strong>3</strong></u>,1,2]</code></li>
	<li>Sau truy vấn cuối cùng: <code>[<u><strong>2</strong></u>,3,1]</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">windows = [1,4,2,3], queries = [4,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,1,4,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dưới đây là mảng cửa sổ sau mỗi truy vấn:</p>

<ul>
	<li>Thứ tự ban đầu: <code>[1,4,2,3]</code></li>
	<li>Sau truy vấn đầu tiên: <code>[<u><strong>4</strong></u>,1,2,3]</code></li>
	<li>Sau truy vấn thứ hai: <code>[<u><strong>1</strong></u>,4,2,3]</code></li>
	<li>Sau truy vấn cuối cùng: <code>[<u><strong>3</strong></u>,1,4,2]</code></li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == windows.length &lt;= 10<sup>5</sup></code></li>
	<li><code>windows</code> là một hoán vị của <code>[1, n]</code>.</li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries[i] &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Duyệt ngược

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn đưa một cửa sổ lên đầu. $n,q\le 10^5$, vì vậy việc đưa một phần tử lên đầu mảng sau mỗi truy vấn sẽ có độ phức tạp bậc hai. Thứ tự cuối cùng chính là thứ tự đưa lên đầu lần cuối: các truy vấn thực hiện sau sẽ nằm về bên trái hơn.
>
> Duyệt mảng truy vấn từ phải sang trái, thêm mỗi id chưa xuất hiện vào kết quả, sau đó thêm các cửa sổ chưa từng xuất hiện trong mảng ban đầu. Mỗi cửa sổ được thêm vào kết quả nhiều nhất một lần.

<!-- thinking:end -->

Theo mô tả bài toán, truy vấn thực hiện sau sẽ xuất hiện sớm hơn trong kết quả. Vì vậy, ta có thể duyệt mảng $\textit{queries}$ theo thứ tự ngược, sử dụng một bảng băm $\textit{s}$ để ghi lại các cửa sổ đã xuất hiện. Với mỗi truy vấn, nếu cửa sổ hiện tại chưa có trong bảng băm, ta thêm nó vào mảng kết quả và đồng thời thêm nó vào bảng băm. Cuối cùng, ta duyệt lại mảng $\textit{windows}$, thêm những cửa sổ chưa có trong bảng băm vào mảng kết quả.

Độ phức tạp thời gian là $O(n + m)$, còn độ phức tạp không gian là $O(m)$. Trong đó, $n$ và $m$ lần lượt là độ dài của mảng $\textit{windows}$ và $\textit{queries}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def simulationResult(self, windows: List[int], queries: List[int]) -> List[int]:
        s = set()
        ans = []
        for q in queries[::-1]:
            if q not in s:
                ans.append(q)
                s.add(q)
        for w in windows:
            if w not in s:
                ans.append(w)
        return ans
```

#### Java

```java
class Solution {
    public int[] simulationResult(int[] windows, int[] queries) {
        int n = windows.length;
        boolean[] s = new boolean[n + 1];
        int[] ans = new int[n];
        int k = 0;
        for (int i = queries.length - 1; i >= 0; --i) {
            int q = queries[i];
            if (!s[q]) {
                ans[k++] = q;
                s[q] = true;
            }
        }
        for (int w : windows) {
            if (!s[w]) {
                ans[k++] = w;
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
    vector<int> simulationResult(vector<int>& windows, vector<int>& queries) {
        int n = windows.size();
        vector<bool> s(n + 1);
        vector<int> ans;
        for (int i = queries.size() - 1; ~i; --i) {
            int q = queries[i];
            if (!s[q]) {
                s[q] = true;
                ans.push_back(q);
            }
        }
        for (int w : windows) {
            if (!s[w]) {
                ans.push_back(w);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func simulationResult(windows []int, queries []int) (ans []int) {
	n := len(windows)
	s := make([]bool, n+1)
	for i := len(queries) - 1; i >= 0; i-- {
		q := queries[i]
		if !s[q] {
			s[q] = true
			ans = append(ans, q)
		}
	}
	for _, w := range windows {
		if !s[w] {
			ans = append(ans, w)
		}
	}
	return
}
```

#### TypeScript

```ts
function simulationResult(windows: number[], queries: number[]): number[] {
    const n = windows.length;
    const s: boolean[] = Array(n + 1).fill(false);
    const ans: number[] = [];
    for (let i = queries.length - 1; i >= 0; i--) {
        const q = queries[i];
        if (!s[q]) {
            s[q] = true;
            ans.push(q);
        }
    }
    for (const w of windows) {
        if (!s[w]) {
            ans.push(w);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
