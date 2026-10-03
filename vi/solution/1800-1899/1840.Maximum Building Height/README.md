---
comments: true
difficulty: Hard
rating: 2374
source: Weekly Contest 238 Q4
tags:
    - Array
    - Math
    - Sorting
---

<!-- problem:start -->

# [1840. Maximum Building Height](https://leetcode.com/problems/maximum-building-height)

[中文文档](/solution/1800-1899/1840.Maximum%20Building%20Height/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn muốn xây dựng <code>n</code> tòa nhà mới trong một thành phố. Các tòa nhà mới được xây thành một hàng và được đánh số từ <code>1</code> đến <code>n</code>.</p>

<p>Tuy nhiên, thành phố có các giới hạn về chiều cao của những tòa nhà mới:</p>

<ul>
	<li>Chiều cao của mỗi tòa nhà phải là một số nguyên không âm.</li>
	<li>Chiều cao của tòa nhà đầu tiên <strong>phải</strong> bằng <code>0</code>.</li>
	<li>Chênh lệch chiều cao giữa hai tòa nhà liền kề bất kỳ <strong>không được vượt quá</strong> <code>1</code>.</li>
</ul>

<p>Ngoài ra, thành phố còn giới hạn chiều cao tối đa của một số tòa nhà cụ thể. Các giới hạn này được cho bởi một mảng số nguyên 2 chiều <code>restrictions</code>, trong đó <code>restrictions[i] = [id<sub>i</sub>, maxHeight<sub>i</sub>]</code> cho biết tòa nhà <code>id<sub>i</sub></code> phải có chiều cao <strong>nhỏ hơn hoặc bằng</strong> <code>maxHeight<sub>i</sub></code>.</p>

<p>Đảm bảo mỗi tòa nhà xuất hiện <strong>không quá một lần</strong> trong <code>restrictions</code>, và tòa nhà <code>1</code> <strong>không</strong> nằm trong <code>restrictions</code>.</p>

<p>Trả về <em><strong>chiều cao lớn nhất có thể</strong> của tòa nhà <strong>cao nhất</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1840.Maximum%20Building%20Height/images/ic236-q4-ex1-1.png" style="width: 400px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, restrictions = [[2,1],[4,1]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Vùng màu xanh trong hình biểu thị chiều cao tối đa cho phép của mỗi tòa nhà.
Ta có thể xây các tòa nhà với chiều cao [0,1,2,1,2], và tòa nhà cao nhất có chiều cao bằng 2.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1840.Maximum%20Building%20Height/images/ic236-q4-ex2.png" style="width: 500px; height: 269px;" />
<pre>
<strong>Đầu vào:</strong> n = 6, restrictions = []
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Vùng màu xanh trong hình biểu thị chiều cao tối đa cho phép của mỗi tòa nhà.
Ta có thể xây các tòa nhà với chiều cao [0,1,2,3,4,5], và tòa nhà cao nhất có chiều cao bằng 5.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1840.Maximum%20Building%20Height/images/ic236-q4-ex3.png" style="width: 500px; height: 187px;" />
<pre>
<strong>Đầu vào:</strong> n = 10, restrictions = [[5,3],[2,5],[7,4],[10,3]]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Vùng màu xanh trong hình biểu thị chiều cao tối đa cho phép của mỗi tòa nhà.
Ta có thể xây các tòa nhà với chiều cao [0,1,2,3,3,4,4,5,4,3], và tòa nhà cao nhất có chiều cao bằng 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= restrictions.length &lt;= min(n - 1, 10<sup>5</sup>)</code></li>
	<li><code>2 &lt;= id<sub>i</sub> &lt;= n</code></li>
	<li><code>id<sub>i</sub></code>&nbsp;là <strong>duy nhất</strong>.</li>
	<li><code>0 &lt;= maxHeight<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Chiều cao của hai tòa nhà liền kề chênh lệch nhiều nhất $1$, và một số tòa nhà có giới hạn chiều cao. Vì $n$ có thể lên tới $10^9$, ta không thể mô phỏng mọi tòa nhà.
>
> Các giới hạn chia hàng tòa nhà thành $O(m)$ đoạn. Ta sắp xếp chúng, rồi lan truyền ràng buộc khoảng cách từ hai đầu để thu hẹp từng giới hạn. Giữa hai giới hạn liên tiếp, đường bao tối ưu tăng rồi giảm; đỉnh có công thức trực tiếp từ hai giới hạn và khoảng cách giữa chúng. Đáp án là giá trị lớn nhất của các đỉnh đó.

<!-- thinking:end -->

Trước tiên, ta sắp xếp tất cả giới hạn theo số hiệu tòa nhà tăng dần.

Sau đó, ta duyệt các giới hạn từ trái sang phải. Với mỗi giới hạn, ta có thể tìm được một cận trên của chiều cao lớn nhất, cụ thể là $r_i[1] = \min(r_i[1], r_{i-1}[1] + r_i[0] - r_{i-1}[0])$, trong đó $r_i$ là giới hạn thứ $i$, còn $r_i[0]$ và $r_i[1]$ lần lượt là số hiệu tòa nhà và cận trên chiều cao lớn nhất của tòa nhà đó.

Tiếp theo, ta duyệt các giới hạn từ phải sang trái. Với mỗi giới hạn, ta có thể tìm được một cận trên của chiều cao lớn nhất, cụ thể là $r_i[1] = \min(r_i[1], r_{i+1}[1] + r_{i+1}[0] - r_i[0])$.

Như vậy, ta thu được cận trên của chiều cao lớn nhất cho mỗi tòa nhà bị giới hạn.

Bài toán yêu cầu chiều cao của tòa nhà cao nhất. Ta có thể duyệt các tòa nhà nằm giữa hai giới hạn kề nhau $i$ và $i+1$. Để chiều cao lớn nhất, chiều cao phải tăng trước rồi giảm. Giả sử chiều cao lớn nhất là $t$, khi đó $t - r_i[1] + t - r_{i+1}[1] \leq r_{i+1}[0] - r_i[0]$, hay $t \leq \frac{r_i[1] + r_{i+1}[1] + r_{i+1}[0] - r_{i}[0]}{2}$. Ta lấy giá trị lớn nhất của mọi $t$ như vậy.

Độ phức tạp thời gian là $O(m \times \log m)$ và độ phức tạp không gian là $O(m)$, trong đó $m$ là số giới hạn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxBuilding(self, n: int, restrictions: List[List[int]]) -> int:
        r = restrictions
        r.append([1, 0])
        r.sort()
        if r[-1][0] != n:
            r.append([n, n - 1])
        m = len(r)
        for i in range(1, m):
            r[i][1] = min(r[i][1], r[i - 1][1] + r[i][0] - r[i - 1][0])
        for i in range(m - 2, 0, -1):
            r[i][1] = min(r[i][1], r[i + 1][1] + r[i + 1][0] - r[i][0])
        ans = 0
        for i in range(m - 1):
            t = (r[i][1] + r[i + 1][1] + r[i + 1][0] - r[i][0]) // 2
            ans = max(ans, t)
        return ans
```

#### Java

```java
class Solution {
    public int maxBuilding(int n, int[][] restrictions) {
        List<int[]> r = new ArrayList<>();
        r.addAll(Arrays.asList(restrictions));
        r.add(new int[] {1, 0});
        Collections.sort(r, (a, b) -> a[0] - b[0]);
        if (r.get(r.size() - 1)[0] != n) {
            r.add(new int[] {n, n - 1});
        }
        int m = r.size();
        for (int i = 1; i < m; ++i) {
            int[] a = r.get(i - 1), b = r.get(i);
            b[1] = Math.min(b[1], a[1] + b[0] - a[0]);
        }
        for (int i = m - 2; i > 0; --i) {
            int[] a = r.get(i), b = r.get(i + 1);
            a[1] = Math.min(a[1], b[1] + b[0] - a[0]);
        }
        int ans = 0;
        for (int i = 0; i < m - 1; ++i) {
            int[] a = r.get(i), b = r.get(i + 1);
            int t = (a[1] + b[1] + b[0] - a[0]) / 2;
            ans = Math.max(ans, t);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxBuilding(int n, vector<vector<int>>& restrictions) {
        auto&& r = restrictions;
        r.push_back({1, 0});
        ranges::sort(r);
        if (r[r.size() - 1][0] != n) {
            r.push_back({n, n - 1});
        }
        int m = r.size();
        for (int i = 1; i < m; ++i) {
            r[i][1] = min(r[i][1], r[i - 1][1] + r[i][0] - r[i - 1][0]);
        }
        for (int i = m - 2; i > 0; --i) {
            r[i][1] = min(r[i][1], r[i + 1][1] + r[i + 1][0] - r[i][0]);
        }
        int ans = 0;
        for (int i = 0; i < m - 1; ++i) {
            int t = (r[i][1] + r[i + 1][1] + r[i + 1][0] - r[i][0]) / 2;
            ans = max(ans, t);
        }
        return ans;
    }
};
```

#### Go

```go
func maxBuilding(n int, restrictions [][]int) (ans int) {
	r := restrictions
	r = append(r, []int{1, 0})
	sort.Slice(r, func(i, j int) bool { return r[i][0] < r[j][0] })
	if r[len(r)-1][0] != n {
		r = append(r, []int{n, n - 1})
	}
	m := len(r)
	for i := 1; i < m; i++ {
		r[i][1] = min(r[i][1], r[i-1][1]+r[i][0]-r[i-1][0])
	}
	for i := m - 2; i > 0; i-- {
		r[i][1] = min(r[i][1], r[i+1][1]+r[i+1][0]-r[i][0])
	}
	for i := 0; i < m-1; i++ {
		t := (r[i][1] + r[i+1][1] + r[i+1][0] - r[i][0]) / 2
		ans = max(ans, t)
	}
	return ans
}
```

#### TypeScript

```ts
function maxBuilding(n: number, restrictions: number[][]): number {
    restrictions.push([1, 0]);
    restrictions.sort((a, b) => a[0] - b[0]);
    if (restrictions[restrictions.length - 1][0] !== n) {
        restrictions.push([n, n - 1]);
    }

    const m = restrictions.length;
    for (let i = 1; i < m; ++i) {
        restrictions[i][1] = Math.min(
            restrictions[i][1],
            restrictions[i - 1][1] + restrictions[i][0] - restrictions[i - 1][0],
        );
    }

    for (let i = m - 2; i >= 0; --i) {
        restrictions[i][1] = Math.min(
            restrictions[i][1],
            restrictions[i + 1][1] + restrictions[i + 1][0] - restrictions[i][0],
        );
    }

    let ans = 0;
    for (let i = 0; i < m - 1; ++i) {
        const t = Math.floor(
            (restrictions[i][1] +
                restrictions[i + 1][1] +
                restrictions[i + 1][0] -
                restrictions[i][0]) /
                2,
        );
        ans = Math.max(ans, t);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
