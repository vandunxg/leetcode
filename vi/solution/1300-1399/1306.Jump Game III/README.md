---
comments: true
difficulty: Medium
rating: 1396
source: Weekly Contest 169 Q3
tags:
    - Depth-First Search
    - Breadth-First Search
    - Array
---

<!-- problem:start -->

# [1306. Jump Game III](https://leetcode.com/problems/jump-game-iii)

[中文文档](/solution/1300-1399/1306.Jump%20Game%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên không âm <code>arr</code>, ban đầu bạn đứng tại chỉ số <code>start</code>. Khi ở chỉ số <code>i</code>, bạn có thể nhảy đến <code>i + arr[i]</code> hoặc <code>i - arr[i]</code>. Hãy kiểm tra xem có thể đến được <strong>bất kỳ</strong> chỉ số nào có giá trị 0 hay không.</p>

<p>Lưu ý: bạn không được nhảy ra ngoài mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [4,2,3,0,3,1,2], start = 5
<strong>Output:</strong> true
<strong>Giải thích:</strong> 
Mọi cách có thể đến chỉ số 3 có giá trị 0 là: 
index 5 -&gt; index 4 -&gt; index 1 -&gt; index 3 
index 5 -&gt; index 6 -&gt; index 4 -&gt; index 1 -&gt; index 3 
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [4,2,3,0,3,1,2], start = 0
<strong>Output:</strong> true 
<strong>Giải thích:
</strong>Một cách có thể đến chỉ số 3 có giá trị 0 là: 
index 0 -&gt; index 4 -&gt; index 1 -&gt; index 3
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> arr = [3,0,2,1,2], start = 2
<strong>Output:</strong> false
<strong>Giải thích: </strong>Không có cách nào đến chỉ số 1 có giá trị 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= arr[i] &lt;&nbsp;arr.length</code></li>
	<li><code>0 &lt;= start &lt; arr.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Từ $\textit{start}$, ta nhảy đến $i \pm \textit{arr}[i]$ và kiểm tra có thể đến được giá trị $0$ hay không. Với $n \le 5 \times 10^4$, nếu thăm lại cùng một chỉ số thì có thể lặp vô hạn. Mỗi chỉ số chỉ nên được đưa vào queue tối đa một lần.
>
> Đây là BFS thông thường trên graph đó: nếu lấy ra một chỉ số có giá trị $0$ thì thành công; nếu không, ta đánh dấu ô đã thăm (ghi $-1$) rồi đưa vào queue hai chỉ số nhảy còn trong mảng và chưa được thăm. Việc đánh dấu giúp cắt bỏ trạng thái đã xử lý, giữ độ phức tạp ở $O(n)$.

<!-- thinking:end -->

Ta có thể dùng BFS để xác định liệu có thể đến một chỉ số có giá trị $0$ hay không.

Dùng queue $q$ để lưu các chỉ số có thể đến được. Ban đầu, đưa chỉ số $start$ vào queue.

Khi queue chưa rỗng, lấy chỉ số đầu tiên $i$ ra. Nếu $arr[i] = 0$, trả về `true`. Nếu không, đánh dấu chỉ số $i$ đã được thăm. Nếu $i + arr[i]$ và $i - arr[i]$ nằm trong phạm vi mảng và chưa được thăm, đưa chúng vào queue rồi tiếp tục tìm kiếm.

Cuối cùng, nếu queue rỗng thì nghĩa là không thể đến chỉ số có giá trị $0$, nên trả về `false`.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canReach(self, arr: List[int], start: int) -> bool:
        q = deque([start])
        while q:
            i = q.popleft()
            if arr[i] == 0:
                return True
            x = arr[i]
            arr[i] = -1
            for j in (i + x, i - x):
                if 0 <= j < len(arr) and arr[j] >= 0:
                    q.append(j)
        return False
```

#### Java

```java
class Solution {
    public boolean canReach(int[] arr, int start) {
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(start);
        while (!q.isEmpty()) {
            int i = q.poll();
            if (arr[i] == 0) {
                return true;
            }
            int x = arr[i];
            arr[i] = -1;
            for (int j : List.of(i + x, i - x)) {
                if (j >= 0 && j < arr.length && arr[j] >= 0) {
                    q.offer(j);
                }
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canReach(vector<int>& arr, int start) {
        queue<int> q{{start}};
        while (!q.empty()) {
            int i = q.front();
            q.pop();
            if (arr[i] == 0) {
                return true;
            }
            int x = arr[i];
            arr[i] = -1;
            for (int j : {i + x, i - x}) {
                if (j >= 0 && j < arr.size() && ~arr[j]) {
                    q.push(j);
                }
            }
        }
        return false;
    }
};
```

#### Go

```go
func canReach(arr []int, start int) bool {
	q := []int{start}
	for len(q) > 0 {
		i := q[0]
		q = q[1:]
		if arr[i] == 0 {
			return true
		}
		x := arr[i]
		arr[i] = -1
		for _, j := range []int{i + x, i - x} {
			if j >= 0 && j < len(arr) && arr[j] >= 0 {
				q = append(q, j)
			}
		}
	}
	return false
}
```

#### TypeScript

```ts
function canReach(arr: number[], start: number): boolean {
    const q = [start];
    for (const i of q) {
        if (arr[i] === 0) {
            return true;
        }
        if (arr[i] === -1 || arr[i] === undefined) {
            continue;
        }
        q.push(i + arr[i], i - arr[i]);
        arr[i] = -1;
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
