---
comments: true
difficulty: Hard
rating: 1948
source: Weekly Contest 324 Q4
tags:
    - Tree
    - Array
    - Binary Tree
    - Lowest Common Ancestor
    - Binary Lifting
---

<!-- problem:start -->

# [2509. Cycle Length Queries in a Tree](https://leetcode.com/problems/cycle-length-queries-in-a-tree)

[中文文档](/solution/2500-2599/2509.Cycle%20Length%20Queries%20in%20a%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>. Có một <strong>cây nhị phân đầy đủ</strong> gồm <code>2<sup>n</sup> - 1</code> nút. Nút gốc của cây có giá trị <code>1</code>, và mọi nút có giá trị <code>val</code> trong khoảng <code>[1, 2<sup>n - 1</sup> - 1]</code> đều có hai nút con:</p>

<ul>
	<li>Nút bên trái có giá trị <code>2 * val</code>, và</li>
	<li>Nút bên phải có giá trị <code>2 * val + 1</code>.</li>
</ul>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>queries</code> có độ dài <code>m</code>, trong đó <code>queries[i] = [a<sub>i</sub>, b<sub>i</sub>]</code>. Với mỗi truy vấn, hãy thực hiện các bước sau:</p>

<ol>
	<li>Thêm một cạnh giữa các nút có giá trị <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code>.</li>
	<li>Tìm độ dài của chu trình trong đồ thị.</li>
	<li>Xóa cạnh vừa thêm giữa các nút có giá trị <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code>.</li>
</ol>

<p><strong>Lưu ý</strong> rằng:</p>

<ul>
	<li><strong>Chu trình</strong> là một đường đi bắt đầu và kết thúc tại cùng một nút, trong đó mỗi cạnh trên đường đi chỉ được đi qua một lần.</li>
	<li>Độ dài của một chu trình là số cạnh được đi qua trong chu trình đó.</li>
	<li>Sau khi thêm cạnh của truy vấn, giữa hai nút trong cây có thể có nhiều cạnh.</li>
</ul>

<p>Hãy trả về <em>một mảng </em><code>answer</code><em> có độ dài </em><code>m</code><em>, trong đó </em><code>answer[i]</code> <em>là đáp án của truy vấn thứ </em><code>i<sup>th</sup></code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2509.Cycle%20Length%20Queries%20in%20a%20Tree/images/bexample1.png" style="width: 647px; height: 128px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, queries = [[5,3],[4,7],[2,3]]
<strong>Đầu ra:</strong> [4,5,3]
<strong>Giải thích:</strong> Các hình minh họa ở trên mô tả cây gồm 2<sup>3</sup> - 1 nút. Các nút được tô đỏ là những nút thuộc chu trình sau khi thêm cạnh.
- Sau khi thêm cạnh giữa các nút 3 và 5, đồ thị có một chu trình gồm các nút [5,2,1,3]. Vì vậy, đáp án của truy vấn đầu tiên là 4. Ta xóa cạnh vừa thêm rồi xử lý truy vấn tiếp theo.
- Sau khi thêm cạnh giữa các nút 4 và 7, đồ thị có một chu trình gồm các nút [4,2,1,3,7]. Vì vậy, đáp án của truy vấn thứ hai là 5. Ta xóa cạnh vừa thêm rồi xử lý truy vấn tiếp theo.
- Sau khi thêm cạnh giữa các nút 2 và 3, đồ thị có một chu trình gồm các nút [2,1,3]. Vì vậy, đáp án của truy vấn thứ ba là 3. Ta xóa cạnh vừa thêm.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2509.Cycle%20Length%20Queries%20in%20a%20Tree/images/aexample2.png" style="width: 146px; height: 71px;" />
<pre>
<strong>Đầu vào:</strong> n = 2, queries = [[1,2]]
<strong>Đầu ra:</strong> [2]
<strong>Giải thích:</strong> Hình minh họa ở trên mô tả cây gồm 2<sup>2</sup> - 1 nút. Các nút được tô đỏ là những nút thuộc chu trình sau khi thêm cạnh.
- Sau khi thêm cạnh giữa các nút 1 và 2, đồ thị có một chu trình gồm các nút [2,1]. Vì vậy, đáp án của truy vấn đầu tiên là 2. Ta xóa cạnh vừa thêm.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 30</code></li>
	<li><code>m == queries.length</code></li>
	<li><code>1 &lt;= m &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length == 2</code></li>
	<li><code>1 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= 2<sup>n</sup> - 1</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm tổ tiên chung gần nhất

<!-- thinking:start -->

> **Tư duy**
>
> Trong một cây nhị phân hoàn hảo, chu trình được tạo bởi cạnh $(a,b)$ có độ dài bằng đường đi từ $a$ đến $b$ cộng một. Việc dựng cây với $n$ tối đa là $30$ sẽ tạo ra $2^n-1$ nút.
>
> Nút cha của một nút là $\lfloor x/2\rfloor$, tức là phép dịch bit sang phải. Ta đưa cả hai nút đi lên, luôn di chuyển nút có nhãn lớn hơn, cho đến khi chúng gặp nhau; số bước cộng một chính là độ dài chu trình. Cách di chuyển này cũng chính là cách tính LCA theo quy tắc đánh số này.

<!-- thinking:end -->

Với mỗi truy vấn, ta tìm tổ tiên chung gần nhất của hai nút $a$ và $b$, đồng thời ghi nhận số bước đi lên. Đáp án của truy vấn là số bước cộng một.

Để tìm tổ tiên chung gần nhất, nếu $a > b$, ta đưa $a$ lên nút cha; nếu $a < b$, ta đưa $b$ lên nút cha. Ta tích lũy số bước cho đến khi $a = b$.

Độ phức tạp thời gian là $O(n \times m)$, trong đó $m$ là độ dài của mảng `queries`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def cycleLengthQueries(self, n: int, queries: List[List[int]]) -> List[int]:
        ans = []
        for a, b in queries:
            t = 1
            while a != b:
                if a > b:
                    a >>= 1
                else:
                    b >>= 1
                t += 1
            ans.append(t)
        return ans
```

#### Java

```java
class Solution {
    public int[] cycleLengthQueries(int n, int[][] queries) {
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int a = queries[i][0], b = queries[i][1];
            int t = 1;
            while (a != b) {
                if (a > b) {
                    a >>= 1;
                } else {
                    b >>= 1;
                }
                ++t;
            }
            ans[i] = t;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> cycleLengthQueries(int n, vector<vector<int>>& queries) {
        vector<int> ans;
        for (auto& q : queries) {
            int a = q[0], b = q[1];
            int t = 1;
            while (a != b) {
                if (a > b) {
                    a >>= 1;
                } else {
                    b >>= 1;
                }
                ++t;
            }
            ans.emplace_back(t);
        }
        return ans;
    }
};
```

#### Go

```go
func cycleLengthQueries(n int, queries [][]int) []int {
	ans := []int{}
	for _, q := range queries {
		a, b := q[0], q[1]
		t := 1
		for a != b {
			if a > b {
				a >>= 1
			} else {
				b >>= 1
			}
			t++
		}
		ans = append(ans, t)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
