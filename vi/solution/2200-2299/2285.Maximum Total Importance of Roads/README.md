---
comments: true
difficulty: Medium
rating: 1496
source: Biweekly Contest 79 Q3
tags:
    - Greedy
    - Graph
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2285. Maximum Total Importance of Roads](https://leetcode.com/problems/maximum-total-importance-of-roads)

[中文文档](/solution/2200-2299/2285.Maximum%20Total%20Importance%20of%20Roads/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> biểu thị số lượng thành phố trong một quốc gia. Các thành phố được đánh số từ <code>0</code> đến <code>n - 1</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>roads</code>, trong đó <code>roads[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị có một con đường <strong>hai chiều</strong> nối các thành phố <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code>.</p>

<p>Bạn cần gán cho mỗi thành phố một giá trị nguyên từ <code>1</code> đến <code>n</code>, trong đó mỗi giá trị chỉ được sử dụng <strong>một lần</strong>. Khi đó, <strong>độ quan trọng</strong> của một con đường được định nghĩa là <strong>tổng</strong> giá trị của hai thành phố mà nó nối.</p>

<p>Hãy trả về <em><strong>tổng độ quan trọng lớn nhất</strong> của tất cả các con đường có thể đạt được sau khi gán các giá trị một cách tối ưu.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2285.Maximum%20Total%20Importance%20of%20Roads/images/ex1drawio.png" style="width: 290px; height: 215px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, roads = [[0,1],[1,2],[2,3],[0,2],[1,3],[2,4]]
<strong>Đầu ra:</strong> 43
<strong>Giải thích:</strong> Hình trên cho thấy quốc gia và các giá trị được gán là [2,4,5,3,1].
- Con đường (0,1) có độ quan trọng là 2 + 4 = 6.
- Con đường (1,2) có độ quan trọng là 4 + 5 = 9.
- Con đường (2,3) có độ quan trọng là 5 + 3 = 8.
- Con đường (0,2) có độ quan trọng là 2 + 5 = 7.
- Con đường (1,3) có độ quan trọng là 4 + 3 = 7.
- Con đường (2,4) có độ quan trọng là 5 + 1 = 6.
Tổng độ quan trọng của tất cả các con đường là 6 + 9 + 8 + 7 + 7 + 6 = 43.
Có thể chứng minh rằng không thể đạt được tổng độ quan trọng lớn hơn 43.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2285.Maximum%20Total%20Importance%20of%20Roads/images/ex2drawio.png" style="width: 281px; height: 151px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, roads = [[0,3],[2,4],[1,3]]
<strong>Đầu ra:</strong> 20
<strong>Giải thích:</strong> Hình trên cho thấy quốc gia và các giá trị được gán là [4,3,2,5,1].
- Con đường (0,3) có độ quan trọng là 4 + 5 = 9.
- Con đường (2,4) có độ quan trọng là 2 + 1 = 3.
- Con đường (1,3) có độ quan trọng là 3 + 5 = 8.
Tổng độ quan trọng của tất cả các con đường là 9 + 3 + 8 = 20.
Có thể chứng minh rằng không thể đạt được tổng độ quan trọng lớn hơn 20.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= roads.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>roads[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>Không có các con đường trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Ta gán các giá trị phân biệt từ $1$ đến $n$ cho các thành phố; độ quan trọng của một con đường là tổng giá trị tại hai đầu mút. Đóng góp của một thành phố bằng bậc của nó nhân với giá trị được gán, vì vậy các thành phố có bậc lớn hơn nên nhận các giá trị lớn hơn.
>
> Đếm bậc của các thành phố, sắp xếp chúng, rồi lấy tích vô hướng với $1..n$.

<!-- thinking:end -->

Ta xét đóng góp của từng thành phố vào tổng độ quan trọng của tất cả các con đường, được lưu trong mảng $\textit{deg}$. Sau đó, ta sắp xếp $\textit{deg}$ theo đóng góp từ nhỏ đến lớn và lần lượt gán $[1, 2, ..., n]$ cho các thành phố theo thứ tự đó.

Độ phức tạp thời gian là $O(n \log n)$, còn độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumImportance(self, n: int, roads: List[List[int]]) -> int:
        deg = [0] * n
        for a, b in roads:
            deg[a] += 1
            deg[b] += 1
        deg.sort()
        return sum(i * v for i, v in enumerate(deg, 1))
```

#### Java

```java
class Solution {
    public long maximumImportance(int n, int[][] roads) {
        int[] deg = new int[n];
        for (int[] r : roads) {
            ++deg[r[0]];
            ++deg[r[1]];
        }
        Arrays.sort(deg);
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += (long) (i + 1) * deg[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumImportance(int n, vector<vector<int>>& roads) {
        vector<int> deg(n);
        for (auto& r : roads) {
            ++deg[r[0]];
            ++deg[r[1]];
        }
        sort(deg.begin(), deg.end());
        long long ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += (i + 1LL) * deg[i];
        }
        return ans;
    }
};
```

#### Go

```go
func maximumImportance(n int, roads [][]int) (ans int64) {
	deg := make([]int, n)
	for _, r := range roads {
		deg[r[0]]++
		deg[r[1]]++
	}
	sort.Ints(deg)
	for i, x := range deg {
		ans += int64(x) * int64(i+1)
	}
	return
}
```

#### TypeScript

```ts
function maximumImportance(n: number, roads: number[][]): number {
    const deg: number[] = Array(n).fill(0);
    for (const [a, b] of roads) {
        ++deg[a];
        ++deg[b];
    }
    deg.sort((a, b) => a - b);
    return deg.reduce((acc, cur, idx) => acc + (idx + 1) * cur, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
