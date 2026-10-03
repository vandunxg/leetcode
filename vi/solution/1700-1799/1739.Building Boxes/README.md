---
comments: true
difficulty: Hard
rating: 2198
source: Weekly Contest 225 Q4
tags:
    - Greedy
    - Math
    - Binary Search
---

<!-- problem:start -->

# [1739. Building Boxes](https://leetcode.com/problems/building-boxes)

[中文文档](/solution/1700-1799/1739.Building%20Boxes/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một nhà kho hình lập phương với chiều rộng, chiều dài và chiều cao đều bằng <code>n</code> đơn vị. Bạn cần đặt <code>n</code> chiếc hộp vào phòng, mỗi hộp là một khối lập phương có cạnh dài một đơn vị. Tuy nhiên, việc đặt hộp phải tuân theo các quy tắc sau:</p>

<ul>
	<li>Bạn có thể đặt các hộp ở bất kỳ đâu trên sàn.</li>
	<li>Nếu hộp <code>x</code> được đặt lên trên hộp <code>y</code>, thì mỗi mặt trong bốn mặt đứng của hộp <code>y</code> <strong>phải</strong> kề với một hộp khác hoặc một bức tường.</li>
</ul>

<p>Cho số nguyên <code>n</code>, hãy trả về <em><strong>số hộp ít nhất</strong> có thể chạm sàn.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1739.Building%20Boxes/images/3-boxes.png" style="width: 135px; height: 143px;" /></p>

<pre>
<strong>Input:</strong> n = 3
<strong>Output:</strong> 3
<strong>Giải thích:</strong> Hình trên minh họa cách đặt ba chiếc hộp.
Các hộp được đặt vào góc phòng, với góc nằm bên trái.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1739.Building%20Boxes/images/4-boxes.png" style="width: 135px; height: 179px;" /></p>

<pre>
<strong>Input:</strong> n = 4
<strong>Output:</strong> 3
<strong>Giải thích:</strong> Hình trên minh họa cách đặt bốn chiếc hộp.
Các hộp được đặt vào góc phòng, với góc nằm bên trái.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1739.Building%20Boxes/images/10-boxes.png" style="width: 271px; height: 257px;" /></p>

<pre>
<strong>Input:</strong> n = 10
<strong>Output:</strong> 6
<strong>Giải thích:</strong> Hình trên minh họa cách đặt mười chiếc hộp.
Các hộp được đặt vào góc phòng, với góc nằm ở phía sau.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy luật toán học

<!-- thinking:start -->

> **Tư duy**
>
> Các hộp xếp ở góc thành dạng bậc thang; một hộp ở trên cần được đỡ ở cả bốn mặt. Muốn giảm số hộp chạm đất, trước tiên ta lấp đầy khối tứ diện hoàn chỉnh cao nhất có thể.
>
> Một lớp đầy ở mức $k$ chứa $1+\cdots+k$ hộp. Ta thêm các lớp đầy khi có thể, sau đó đặt số hộp còn lại trên sàn theo các nhóm $1,2,3,\ldots$, mỗi lần tăng số hộp chạm sàn lên một.

<!-- thinking:end -->

Theo đề bài, hộp có nhiều lớp nhất cần được đặt ở góc tường, các hộp được xếp thành dạng bậc thang để tối thiểu số hộp chạm sàn.

Giả sử các hộp được xếp thành $k$ lớp. Từ trên xuống dưới, nếu mỗi lớp được lấp đầy thì số hộp trong từng lớp lần lượt là $1, 1+2, 1+2+3, \cdots, 1+2+\cdots+k$.

Nếu sau đó vẫn còn hộp, ta tiếp tục đặt chúng từ lớp thấp nhất. Khi đặt $i$ hộp, tổng số hộp có thể đặt thêm là $1+2+\cdots+i$.

Độ phức tạp thời gian là $O(\sqrt{n})$, trong đó $n$ là số hộp được cho trong đề bài. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumBoxes(self, n: int) -> int:
        s, k = 0, 1
        while s + k * (k + 1) // 2 <= n:
            s += k * (k + 1) // 2
            k += 1
        k -= 1
        ans = k * (k + 1) // 2
        k = 1
        while s < n:
            ans += 1
            s += k
            k += 1
        return ans
```

#### Java

```java
class Solution {
    public int minimumBoxes(int n) {
        int s = 0, k = 1;
        while (s + k * (k + 1) / 2 <= n) {
            s += k * (k + 1) / 2;
            ++k;
        }
        --k;
        int ans = k * (k + 1) / 2;
        k = 1;
        while (s < n) {
            ++ans;
            s += k;
            ++k;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumBoxes(int n) {
        int s = 0, k = 1;
        while (s + k * (k + 1) / 2 <= n) {
            s += k * (k + 1) / 2;
            ++k;
        }
        --k;
        int ans = k * (k + 1) / 2;
        k = 1;
        while (s < n) {
            ++ans;
            s += k;
            ++k;
        }
        return ans;
    }
};
```

#### Go

```go
func minimumBoxes(n int) int {
	s, k := 0, 1
	for s+k*(k+1)/2 <= n {
		s += k * (k + 1) / 2
		k++
	}
	k--
	ans := k * (k + 1) / 2
	k = 1
	for s < n {
		ans++
		s += k
		k++
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
