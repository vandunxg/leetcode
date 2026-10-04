---
comments: true
difficulty: Medium
tags:
    - Design
    - Array
    - Hash Table
    - Matrix
    - Ordered Set
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3391. Design a 3D Binary Matrix with Efficient Layer Tracking 🔒](https://leetcode.com/problems/design-a-3d-binary-matrix-with-efficient-layer-tracking)

[中文文档](/solution/3300-3399/3391.Design%20a%203D%20Binary%20Matrix%20with%20Efficient%20Layer%20Tracking/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng 3D <strong>nhị phân</strong> kích thước <code>n x n x n</code> là <code>matrix</code>.</p>

<p>Hãy cài đặt lớp <code>Matrix3D</code>:</p>

<ul>
	<li><code>Matrix3D(int n)</code> khởi tạo đối tượng với mảng 3D nhị phân <code>matrix</code>, trong đó <strong>tất cả</strong> phần tử ban đầu được đặt bằng 0.</li>
	<li><code>void setCell(int x, int y, int z)</code> đặt giá trị tại <code>matrix[x][y][z]</code> bằng 1.</li>
	<li><code>void unsetCell(int x, int y, int z)</code> đặt giá trị tại <code>matrix[x][y][z]</code> bằng 0.</li>
	<li><code>int largestMatrix()</code> trả về chỉ số <code>x</code> sao cho <code>matrix[x]</code> chứa nhiều số 1 nhất. Nếu có nhiều chỉ số như vậy, trả về <code>x</code> <strong>lớn nhất</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;Matrix3D&quot;, &quot;setCell&quot;, &quot;largestMatrix&quot;, &quot;setCell&quot;, &quot;largestMatrix&quot;, &quot;setCell&quot;, &quot;largestMatrix&quot;]<br />
[[3], [0, 0, 0], [], [1, 1, 2], [], [0, 0, 1], []]</span></p>

<p><strong>Đầu ra:</strong><br />
<span class="example-io">[null, null, 0, null, 1, null, 0] </span></p>

<p><strong>Giải thích</strong></p>
Matrix3D matrix3D = new Matrix3D(3); // Initializes a <code>3 x 3 x 3</code> 3D array <code>matrix</code>, filled with all 0&#39;s.<br />
matrix3D.setCell(0, 0, 0); // Sets <code>matrix[0][0][0]</code> to 1.<br />
matrix3D.largestMatrix(); // Returns 0. <code>matrix[0]</code> has the most number of 1&#39;s.<br />
matrix3D.setCell(1, 1, 2); // Sets <code>matrix[1][1][2]</code> to 1.<br />
matrix3D.largestMatrix(); // Returns 1. <code>matrix[0]</code> and <code>matrix[1]</code> tie with the most number of 1&#39;s, but index 1 is bigger.<br />
matrix3D.setCell(0, 0, 1); // Sets <code>matrix[0][0][1]</code> to 1.<br />
matrix3D.largestMatrix(); // Returns 0. <code>matrix[0]</code> has the most number of 1&#39;s.</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;Matrix3D&quot;, &quot;setCell&quot;, &quot;largestMatrix&quot;, &quot;unsetCell&quot;, &quot;largestMatrix&quot;]<br />
[[4], [2, 1, 1], [], [2, 1, 1], []]</span></p>

<p><strong>Đầu ra:</strong><br />
<span class="example-io">[null, null, 2, null, 3] </span></p>

<p><strong>Giải thích</strong></p>
Matrix3D matrix3D = new Matrix3D(4); // Initializes a <code>4 x 4 x 4</code> 3D array <code>matrix</code>, filled with all 0&#39;s.<br />
matrix3D.setCell(2, 1, 1); // Sets <code>matrix[2][1][1]</code> to 1.<br />
matrix3D.largestMatrix(); // Returns 2. <code>matrix[2]</code> has the most number of 1&#39;s.<br />
matrix3D.unsetCell(2, 1, 1); // Sets <code>matrix[2][1][1]</code> to 0.<br />
matrix3D.largestMatrix(); // Returns 3. All indices from 0 to 3 tie with the same number of 1&#39;s, but index 3 is the biggest.</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>0 &lt;= x, y, z &lt; n</code></li>
	<li>Có nhiều nhất <code>10<sup>5</sup></code> lần gọi đến <code>setCell</code> và <code>unsetCell</code>.</li>
	<li>Có nhiều nhất <code>10<sup>4</sup></code> lần gọi đến <code>largestMatrix</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Ta lật trạng thái các ô trong $O(\log n)$ và truy vấn layer có nhiều số 1 nhất, ưu tiên chỉ số lớn hơn khi hòa. Với $n \le 100$, một mảng 3D lưu các bit và $\textit{cnt}[x]$ lưu tổng theo từng layer.
>
> Ordered set có khóa $(-\textit{cnt}[x],-x)$ sẽ đưa đáp án lên đầu. Các thao tác set và unset xóa pair cũ, cập nhật số đếm rồi chèn pair mới.
>
> Các layer có số đếm bằng 0 được bỏ qua; khi set rỗng, trả về $n-1$ theo đề bài.

<!-- thinking:end -->

Ta dùng một mảng ba chiều $\textit{g}$ để biểu diễn ma trận, trong đó $\textit{g}[x][y][z]$ là giá trị tại tọa độ $(x, y, z)$ trong ma trận. Ta dùng một mảng $\textit{cnt}$ có độ dài $n$ để ghi lại số lượng số 1 trong mỗi layer. Ta dùng một ordered set $\textit{sl}$ để duy trì số lượng số 1 và số thứ tự layer của mỗi layer. Các phần tử trong $\textit{sl}$ là $(\textit{cnt}[x], x)$, vì vậy $\textit{sl}$ có thể được sắp xếp giảm dần theo số lượng số 1, và giảm dần theo số thứ tự layer nếu số lượng số 1 bằng nhau.

Khi gọi phương thức `setCell`, trước tiên ta kiểm tra xem $(x, y, z)$ đã được đặt bằng 1 hay chưa. Nếu rồi, ta trả về ngay. Nếu chưa, ta đặt $\textit{g}[x][y][z]$ bằng 1, xóa $(\textit{cnt}[x], x)$ khỏi $\textit{sl}$, tăng $\textit{cnt}[x]$ lên 1, rồi thêm $(\textit{cnt}[x], x)$ vào $\textit{sl}$.

Khi gọi phương thức `unsetCell`, trước tiên ta kiểm tra xem $(x, y, z)$ đã được đặt bằng 0 hay chưa. Nếu rồi, ta trả về ngay. Nếu chưa, ta đặt $\textit{g}[x][y][z]$ bằng 0, xóa $(\textit{cnt}[x], x)$ khỏi $\textit{sl}$, giảm $\textit{cnt}[x]$ đi 1, và nếu $\textit{cnt}[x]$ lớn hơn 0 thì thêm $(\textit{cnt}[x], x)$ vào $\textit{sl}$.

Khi gọi phương thức `largestMatrix`, ta trả về giá trị thứ hai của phần tử đầu tiên trong $\textit{sl}$. Nếu $\textit{sl}$ rỗng, ta trả về $n - 1$.

Về độ phức tạp thời gian, hai phương thức `setCell` và `unsetCell` đều có độ phức tạp $O(\log n)$, còn phương thức `largestMatrix` có độ phức tạp $O(1)$. Độ phức tạp không gian là $O(n^3)$.

<!-- tabs:start -->

#### Python3

```python
class matrix3D:

    def __init__(self, n: int):
        self.g = [[[0] * n for _ in range(n)] for _ in range(n)]
        self.cnt = [0] * n
        self.sl = SortedList(key=lambda x: (-x[0], -x[1]))

    def setCell(self, x: int, y: int, z: int) -> None:
        if self.g[x][y][z]:
            return
        self.g[x][y][z] = 1
        self.sl.discard((self.cnt[x], x))
        self.cnt[x] += 1
        self.sl.add((self.cnt[x], x))

    def unsetCell(self, x: int, y: int, z: int) -> None:
        if self.g[x][y][z] == 0:
            return
        self.g[x][y][z] = 0
        self.sl.discard((self.cnt[x], x))
        self.cnt[x] -= 1
        if self.cnt[x]:
            self.sl.add((self.cnt[x], x))

    def largestMatrix(self) -> int:
        return self.sl[0][1] if self.sl else len(self.g) - 1


# Your matrix3D object will be instantiated and called as such:
# obj = matrix3D(n)
# obj.setCell(x,y,z)
# obj.unsetCell(x,y,z)
# param_3 = obj.largestMatrix()
```

#### Java

```java
class matrix3D {
    private final int[][][] g;
    private final int[] cnt;
    private final TreeSet<int[]> sl
        = new TreeSet<>((a, b) -> a[0] == b[0] ? b[1] - a[1] : b[0] - a[0]);

    public matrix3D(int n) {
        g = new int[n][n][n];
        cnt = new int[n];
    }

    public void setCell(int x, int y, int z) {
        if (g[x][y][z] == 1) {
            return;
        }
        g[x][y][z] = 1;
        sl.remove(new int[] {cnt[x], x});
        cnt[x]++;
        sl.add(new int[] {cnt[x], x});
    }

    public void unsetCell(int x, int y, int z) {
        if (g[x][y][z] == 0) {
            return;
        }
        g[x][y][z] = 0;
        sl.remove(new int[] {cnt[x], x});
        cnt[x]--;
        if (cnt[x] > 0) {
            sl.add(new int[] {cnt[x], x});
        }
    }

    public int largestMatrix() {
        return sl.isEmpty() ? g.length - 1 : sl.first()[1];
    }
}

/**
 * Your matrix3D object will be instantiated and called as such:
 * matrix3D obj = new matrix3D(n);
 * obj.setCell(x,y,z);
 * obj.unsetCell(x,y,z);
 * int param_3 = obj.largestMatrix();
 */
```

#### C++

```cpp
class matrix3D {
private:
    vector<vector<vector<int>>> g;
    vector<int> cnt;
    set<pair<int, int>> sl;

public:
    matrix3D(int n) {
        g.resize(n, vector<vector<int>>(n, vector<int>(n, 0)));
        cnt.resize(n, 0);
    }

    void setCell(int x, int y, int z) {
        if (g[x][y][z] == 1) {
            return;
        }
        g[x][y][z] = 1;
        sl.erase({-cnt[x], -x});
        cnt[x]++;
        sl.insert({-cnt[x], -x});
    }

    void unsetCell(int x, int y, int z) {
        if (g[x][y][z] == 0) {
            return;
        }
        g[x][y][z] = 0;
        sl.erase({-cnt[x], -x});
        cnt[x]--;
        if (cnt[x]) {
            sl.insert({-cnt[x], -x});
        }
    }

    int largestMatrix() {
        return sl.empty() ? g.size() - 1 : -sl.begin()->second;
    }
};

/**
 * Your matrix3D object will be instantiated and called as such:
 * matrix3D* obj = new matrix3D(n);
 * obj->setCell(x,y,z);
 * obj->unsetCell(x,y,z);
 * int param_3 = obj->largestMatrix();
 */
```

#### Go

```go

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
