---
comments: true
difficulty: Medium
tags:
    - Reservoir Sampling
    - Hash Table
    - Math
    - Randomized
---

<!-- problem:start -->

# [519. Random Flip Matrix](https://leetcode.com/problems/random-flip-matrix)

[中文文档](/solution/0500-0599/0519.Random%20Flip%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một lưới nhị phân <code>m x n</code> tên là <code>matrix</code>, ban đầu tất cả giá trị đều bằng <code>0</code>. Hãy thiết kế thuật toán chọn ngẫu nhiên một chỉ số <code>(i, j)</code> sao cho <code>matrix[i][j] == 0</code>, rồi đổi giá trị đó thành <code>1</code>. Mọi chỉ số <code>(i, j)</code> thỏa mãn <code>matrix[i][j] == 0</code> phải có xác suất được chọn như nhau.</p>

<p>Tối ưu thuật toán để giảm số lần gọi hàm random <strong>có sẵn</strong> trong ngôn ngữ của bạn, đồng thời tối ưu độ phức tạp thời gian và không gian.</p>

<p>Hãy triển khai class <code>Solution</code>:</p>

<ul>
	<li><code>Solution(int m, int n)</code> Khởi tạo object với kích thước ma trận nhị phân là <code>m</code> và <code>n</code>.</li>
	<li><code>int[] flip()</code> Trả về ngẫu nhiên một chỉ số <code>[i, j]</code> của ma trận sao cho <code>matrix[i][j] == 0</code>, rồi đổi giá trị đó thành <code>1</code>.</li>
	<li><code>void reset()</code> Đặt lại tất cả giá trị trong ma trận thành <code>0</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;Solution&quot;, &quot;flip&quot;, &quot;flip&quot;, &quot;flip&quot;, &quot;reset&quot;, &quot;flip&quot;]
[[3, 1], [], [], [], [], []]
<strong>Đầu ra</strong>
[null, [1, 0], [2, 0], [0, 0], null, [2, 0]]

<strong>Giải thích</strong>
Solution solution = new Solution(3, 1);
solution.flip();  // return [1, 0], [0,0], [1,0], and [2,0] should be equally likely to be returned.
solution.flip();  // return [2, 0], Since [1,0] was returned, [2,0] and [0,0]
solution.flip();  // return [0, 0], Based on the previously returned indices, only [0,0] can be returned.
solution.reset(); // All the values are reset to 0 and can be returned.
solution.flip();  // return [2, 0], [0,0], [1,0], and [2,0] should be equally likely to be returned.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 10<sup>4</sup></code></li>
	<li>Mỗi lần gọi <code>flip</code> sẽ luôn có ít nhất một ô trống.</li>
	<li>Có tối đa <code>1000</code> lần gọi <code>flip</code> và <code>reset</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có thể chọn đều một ô chưa được lật bằng cách lưu mọi tọa độ còn lại, nhưng cách đó tốn $O(mn)$ bộ nhớ.
>
> Xem lưới như đoạn $[0,\textit{total})$ và áp dụng Fisher–Yates: chọn $x$ trong $[0,\textit{total})$, đổi chỗ nó với chỉ số chưa dùng cuối cùng, rồi giảm $\textit{total}$. Hash map chỉ lưu các chỉ số đã được ánh xạ lại; nếu không có key thì chỉ số giữ nguyên. `reset` xóa map và khôi phục $\textit{total}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def __init__(self, m: int, n: int):
        self.m = m
        self.n = n
        self.total = m * n
        self.mp = {}

    def flip(self) -> List[int]:
        self.total -= 1
        x = random.randint(0, self.total)
        idx = self.mp.get(x, x)
        self.mp[x] = self.mp.get(self.total, self.total)
        return [idx // self.n, idx % self.n]

    def reset(self) -> None:
        self.total = self.m * self.n
        self.mp.clear()


# Your Solution object will be instantiated and called as such:
# obj = Solution(m, n)
# param_1 = obj.flip()
# obj.reset()
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private int total;
    private Random rand = new Random();
    private Map<Integer, Integer> mp = new HashMap<>();

    public Solution(int m, int n) {
        this.m = m;
        this.n = n;
        this.total = m * n;
    }

    public int[] flip() {
        int x = rand.nextInt(total--);
        int idx = mp.getOrDefault(x, x);
        mp.put(x, mp.getOrDefault(total, total));
        return new int[] {idx / n, idx % n};
    }

    public void reset() {
        total = m * n;
        mp.clear();
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(m, n);
 * int[] param_1 = obj.flip();
 * obj.reset();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
