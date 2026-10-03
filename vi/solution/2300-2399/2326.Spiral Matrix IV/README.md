---
comments: true
difficulty: Medium
rating: 1421
source: Weekly Contest 300 Q2
tags:
    - Array
    - Linked List
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [2326. Spiral Matrix IV](https://leetcode.com/problems/spiral-matrix-iv)

[中文文档](/solution/2300-2399/2326.Spiral%20Matrix%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>m</code> và <code>n</code>, lần lượt biểu thị kích thước của một ma trận.</p>

<p>Đồng thời, cho <code>head</code> của một linked list gồm các số nguyên.</p>

<p>Hãy tạo một ma trận <code>m x n</code> chứa các số nguyên trong linked list theo thứ tự <strong>xoắn ốc</strong> <strong>(theo chiều kim đồng hồ)</strong>, bắt đầu từ <strong>góc trên bên trái</strong> của ma trận. Nếu còn các ô trống, hãy điền chúng bằng <code>-1</code>.</p>

<p>Trả về <em>ma trận đã tạo</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2326.Spiral%20Matrix%20IV/images/ex1new.jpg" style="width: 240px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> m = 3, n = 5, head = [3,0,2,6,8,1,7,9,4,2,5,5,0]
<strong>Đầu ra:</strong> [[3,0,2,6,8],[5,0,-1,-1,1],[5,2,4,9,7]]
<strong>Giải thích:</strong> Sơ đồ trên minh họa cách các giá trị được điền vào ma trận.
Lưu ý rằng các ô còn lại trong ma trận được điền bằng -1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2326.Spiral%20Matrix%20IV/images/ex2.jpg" style="width: 221px; height: 60px;" />
<pre>
<strong>Đầu vào:</strong> m = 1, n = 4, head = [0,1,2]
<strong>Đầu ra:</strong> [[0,1,2,-1]]
<strong>Giải thích:</strong> Sơ đồ trên minh họa cách các giá trị được điền từ trái sang phải.
Ô cuối cùng trong ma trận được đặt là -1.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li>Số node trong linked list nằm trong khoảng <code>[1, m * n]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần điền một ma trận $m \times n$ theo thứ tự xoắn ốc từ một linked list, để các ô còn lại là $-1$. Vì có nhiều nhất $10^5$ ô, chỉ cần mô phỏng đường đi.
>
> Điền sẵn $-1$, rồi lần lượt đi sang phải, xuống dưới, sang trái và lên trên. Đổi hướng khi ô tiếp theo nằm ngoài phạm vi hoặc đã được ghi. Dừng khi linked list hết; các ô chưa được chạm tới vẫn giữ giá trị $-1$.

<!-- thinking:end -->

Ta định nghĩa một mảng hai chiều $\textit{ans}$ để lưu các phần tử trong linked list, ban đầu tất cả đều được điền bằng $-1$. Ta định nghĩa ba biến $i, j, k$, lần lượt biểu thị hàng, cột hiện tại và hướng. Ta định nghĩa một mảng $\textit{dirs}$ để biểu diễn độ lệch của bốn hướng.

Sau đó, ta bắt đầu duyệt linked list. Mỗi khi duyệt qua một node, ta điền giá trị của node hiện tại vào $\textit{ans}[i][j]$, rồi cập nhật con trỏ của linked list. Nếu linked list rỗng, điều đó có nghĩa là mọi phần tử đã được điền và ta thoát khỏi vòng lặp.

Nếu linked list chưa rỗng, ta cần tìm vị trí của phần tử tiếp theo. Ta có thể tính vị trí tiếp theo $(x, y)$ từ vị trí hiện tại $(i, j)$ và hướng hiện tại $k$. Nếu $(x, y)$ nằm trong phạm vi ma trận và $\textit{ans}[x][y]$ là $-1$, nghĩa là $(x, y)$ chưa được điền, nên ta chọn $(x, y)$ làm vị trí tiếp theo. Nếu không, ta cần đổi hướng.

Sau khi duyệt xong linked list, ta thu được ma trận xoắn ốc và trả về ma trận đó.

Độ phức tạp thời gian là $O(m \times n)$, độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def spiralMatrix(self, m: int, n: int, head: Optional[ListNode]) -> List[List[int]]:
        ans = [[-1] * n for _ in range(m)]
        i = j = k = 0
        dirs = (0, 1, 0, -1, 0)
        while 1:
            ans[i][j] = head.val
            head = head.next
            if head is None:
                break
            while 1:
                x, y = i + dirs[k], j + dirs[k + 1]
                if 0 <= x < m and 0 <= y < n and ans[x][y] == -1:
                    i, j = x, y
                    break
                k = (k + 1) % 4
        return ans
```

#### Java

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public int[][] spiralMatrix(int m, int n, ListNode head) {
        int[][] ans = new int[m][n];
        for (var row : ans) {
            Arrays.fill(row, -1);
        }
        int i = 0, j = 0, k = 0;
        final int[] dirs = {0, 1, 0, -1, 0};
        while (true) {
            ans[i][j] = head.val;
            head = head.next;
            if (head == null) {
                break;
            }
            while (true) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && ans[x][y] == -1) {
                    i = x;
                    j = y;
                    break;
                }
                k = (k + 1) % 4;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> spiralMatrix(int m, int n, ListNode* head) {
        vector<vector<int>> ans(m, vector<int>(n, -1));
        int i = 0, j = 0, k = 0;
        const int dirs[5] = {0, 1, 0, -1, 0};
        while (1) {
            ans[i][j] = head->val;
            head = head->next;
            if (!head) {
                break;
            }
            while (1) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && ans[x][y] == -1) {
                    i = x;
                    j = y;
                    break;
                }
                k = (k + 1) % 4;
            }
        }
        return ans;
    }
};
```

#### Go

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func spiralMatrix(m int, n int, head *ListNode) [][]int {
	ans := make([][]int, m)
	for i := range ans {
		ans[i] = make([]int, n)
		for j := range ans[i] {
			ans[i][j] = -1
		}
	}
	i, j, k := 0, 0, 0
	dirs := [5]int{0, 1, 0, -1, 0}
	for {
		ans[i][j] = head.Val
		if head = head.Next; head == nil {
			break
		}
		for {
			x, y := i+dirs[k], j+dirs[k+1]
			if x >= 0 && x < m && y >= 0 && y < n && ans[x][y] == -1 {
				i, j = x, y
				break
			}
			k = (k + 1) % 4
		}
	}
	return ans
}
```

#### TypeScript

```ts
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     val: number
 *     next: ListNode | null
 *     constructor(val?: number, next?: ListNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.next = (next===undefined ? null : next)
 *     }
 * }
 */

function spiralMatrix(m: number, n: number, head: ListNode | null): number[][] {
    const ans: number[][] = Array.from({ length: m }, () => Array(n).fill(-1));
    const dirs: number[] = [0, 1, 0, -1, 0];
    let [i, j, k] = [0, 0, 0];
    while (1) {
        ans[i][j] = head.val;
        head = head.next;
        if (!head) {
            break;
        }
        while (1) {
            const [x, y] = [i + dirs[k], j + dirs[k + 1]];
            if (x >= 0 && x < m && y >= 0 && y < n && ans[x][y] === -1) {
                i = x;
                j = y;
                break;
            }
            k = (k + 1) % 4;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
