---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Counting Sort
    - Sorting
---

<!-- problem:start -->

# [3189. Minimum Moves to Get a Peaceful Board 🔒](https://leetcode.com/problems/minimum-moves-to-get-a-peaceful-board)

[中文文档](/solution/3100-3199/3189.Minimum%20Moves%20to%20Get%20a%20Peaceful%20Board/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng 2 chiều <code>rooks</code> có độ dài <code>n</code>, trong đó <code>rooks[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu thị vị trí của một quân xe trên bàn cờ có kích thước <code>n x n</code>. Nhiệm vụ của bạn là di chuyển các quân xe <strong>từng ô một</strong> theo chiều dọc hoặc chiều ngang (sang một ô <em>kề</em>) sao cho bàn cờ trở nên <strong>yên bình</strong>.</p>

<p>Một bàn cờ được gọi là <strong>yên bình</strong> nếu có <strong>chính xác</strong> một quân xe trên mỗi hàng và mỗi cột.</p>

<p>Trả về số bước di chuyển <strong>ít nhất</strong> cần thực hiện để có được một <em>bàn cờ yên bình</em>.</p>

<p><strong>Lưu ý</strong> rằng <strong>không được phép</strong> có hai quân xe cùng nằm trên một ô tại bất kỳ thời điểm nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">rooks = [[0,0],[1,0],[1,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3189.Minimum%20Moves%20to%20Get%20a%20Peaceful%20Board/images/ex1-edited.gif" style="width: 150px; height: 150px;" /></div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">rooks = [[0,0],[0,1],[0,2],[0,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3189.Minimum%20Moves%20to%20Get%20a%20Peaceful%20Board/images/ex2-edited.gif" style="width: 200px; height: 200px;" /></div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == rooks.length &lt;= 500</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= n - 1</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho không có hai quân xe nào nằm trên cùng một ô.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Quân xe có thể di chuyển theo bốn hướng và phải nằm trên các hàng và cột khác nhau. Các xung đột giữa các hàng không ảnh hưởng lẫn nhau với các xung đột giữa các cột.
>
> Sau khi sắp xếp theo hàng, quân xe thứ $i$ nên được đưa đến hàng $i$, tương tự với các cột. Việc hoán đổi hai vị trí đích không bao giờ làm giảm tổng số bước.
>
> Hai lần sắp xếp sẽ cộng các giá trị $|x-i|$ và $|y-j|$. Các bước theo khoảng cách Manhattan được cộng độc lập trên hai trục.

<!-- thinking:end -->

Ta có thể sắp xếp tất cả các quân xe theo tọa độ x, sau đó lần lượt phân bổ chúng vào từng hàng và tính tổng khoảng cách từ mỗi quân xe đến vị trí đích. Tiếp theo, sắp xếp tất cả các quân xe theo tọa độ y và dùng cách tương tự để tính tổng khoảng cách từ mỗi quân xe đến vị trí đích. Cuối cùng, đáp án là tổng của hai khoảng cách này.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là số quân xe.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMoves(self, rooks: List[List[int]]) -> int:
        rooks.sort()
        ans = sum(abs(x - i) for i, (x, _) in enumerate(rooks))
        rooks.sort(key=lambda x: x[1])
        ans += sum(abs(y - j) for j, (_, y) in enumerate(rooks))
        return ans
```

#### Java

```java
class Solution {
    public int minMoves(int[][] rooks) {
        Arrays.sort(rooks, (a, b) -> a[0] - b[0]);
        int ans = 0;
        int n = rooks.length;
        for (int i = 0; i < n; ++i) {
            ans += Math.abs(rooks[i][0] - i);
        }
        Arrays.sort(rooks, (a, b) -> a[1] - b[1]);
        for (int j = 0; j < n; ++j) {
            ans += Math.abs(rooks[j][1] - j);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMoves(vector<vector<int>>& rooks) {
        sort(rooks.begin(), rooks.end());
        int ans = 0;
        int n = rooks.size();
        for (int i = 0; i < n; ++i) {
            ans += abs(rooks[i][0] - i);
        }
        sort(rooks.begin(), rooks.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[1] < b[1];
        });
        for (int j = 0; j < n; ++j) {
            ans += abs(rooks[j][1] - j);
        }
        return ans;
    }
};
```

#### Go

```go
func minMoves(rooks [][]int) (ans int) {
	sort.Slice(rooks, func(i, j int) bool { return rooks[i][0] < rooks[j][0] })
	for i, row := range rooks {
		ans += int(math.Abs(float64(row[0] - i)))
	}
	sort.Slice(rooks, func(i, j int) bool { return rooks[i][1] < rooks[j][1] })
	for j, col := range rooks {
		ans += int(math.Abs(float64(col[1] - j)))
	}
	return
}
```

#### TypeScript

```ts
function minMoves(rooks: number[][]): number {
    rooks.sort((a, b) => a[0] - b[0]);
    let ans = rooks.reduce((sum, rook, i) => sum + Math.abs(rook[0] - i), 0);
    rooks.sort((a, b) => a[1] - b[1]);
    ans += rooks.reduce((sum, rook, j) => sum + Math.abs(rook[1] - j), 0);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
