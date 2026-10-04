---
comments: true
difficulty: Hard
rating: 1789
source: Weekly Contest 406 Q4
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [3219. Minimum Cost for Cutting Cake II](https://leetcode.com/problems/minimum-cost-for-cutting-cake-ii)

[Tài liệu tiếng Trung](/solution/3200-3299/3219.Minimum%20Cost%20for%20Cutting%20Cake%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Có một chiếc bánh kích thước <code>m x n</code> cần được cắt thành các miếng <code>1 x 1</code>.</p>

<p>Bạn được cho các số nguyên <code>m</code>, <code>n</code> và hai mảng:</p>

<ul>
	<li><code>horizontalCut</code> có kích thước <code>m - 1</code>, trong đó <code>horizontalCut[i]</code> biểu thị chi phí cắt theo đường ngang <code>i</code>.</li>
	<li><code>verticalCut</code> có kích thước <code>n - 1</code>, trong đó <code>verticalCut[j]</code> biểu thị chi phí cắt theo đường dọc <code>j</code>.</li>
</ul>

<p>Trong một thao tác, bạn có thể chọn bất kỳ miếng bánh nào chưa phải là hình vuông <code>1 x 1</code> và thực hiện một trong các nhát cắt sau:</p>

<ol>
	<li>Cắt theo đường ngang <code>i</code> với chi phí <code>horizontalCut[i]</code>.</li>
	<li>Cắt theo đường dọc <code>j</code> với chi phí <code>verticalCut[j]</code>.</li>
</ol>

<p>Sau khi cắt, miếng bánh được chia thành hai phần riêng biệt.</p>

<p>Chi phí của một nhát cắt chỉ phụ thuộc vào chi phí ban đầu của đường cắt và không thay đổi.</p>

<p>Trả về <strong>tổng chi phí nhỏ nhất</strong> để cắt toàn bộ bánh thành các miếng <code>1 x 1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 3, n = 2, horizontalCut = [1,3], verticalCut = [5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">13</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3219.Minimum%20Cost%20for%20Cutting%20Cake%20II/images/ezgifcom-animated-gif-maker-1.gif" style="width: 280px; height: 320px;" /></p>

<ul>
	<li>Thực hiện nhát cắt trên đường dọc 0 với chi phí 5, tổng chi phí hiện tại là 5.</li>
	<li>Thực hiện nhát cắt trên đường ngang 0 của lưới con <code>3 x 1</code> với chi phí 1.</li>
	<li>Thực hiện nhát cắt trên đường ngang 0 của lưới con <code>3 x 1</code> với chi phí 1.</li>
	<li>Thực hiện nhát cắt trên đường ngang 1 của lưới con <code>2 x 1</code> với chi phí 3.</li>
	<li>Thực hiện nhát cắt trên đường ngang 1 của lưới con <code>2 x 1</code> với chi phí 3.</li>
</ul>

<p>Tổng chi phí là <code>5 + 1 + 1 + 3 + 3 = 13</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 2, n = 2, horizontalCut = [7], verticalCut = [4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Thực hiện nhát cắt trên đường ngang 0 với chi phí 7.</li>
	<li>Thực hiện nhát cắt trên đường dọc 0 của lưới con <code>1 x 2</code> với chi phí 4.</li>
	<li>Thực hiện nhát cắt trên đường dọc 0 của lưới con <code>1 x 2</code> với chi phí 4.</li>
</ul>

<p>Tổng chi phí là <code>7 + 4 + 4 = 15</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>horizontalCut.length == m - 1</code></li>
	<li><code>verticalCut.length == n - 1</code></li>
	<li><code>1 &lt;= horizontalCut[i], verticalCut[i] &lt;= 10<sup>3</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Bài toán này tương tự Cutting Cake I, nhưng $m,n\le 10^5$, nên DP theo cấp số mũ hoặc bậc hai không còn khả thi; ta phải duy trì cách gộp "cắt các chi phí lớn trước".
>
> Sắp xếp cả hai mảng chi phí theo thứ tự giảm dần và luôn cắt phía đang có chi phí lớn hơn, cộng $h$ hoặc $v$ lần chi phí đó. Việc sắp xếp có độ phức tạp $O((m+n)\log(m+n))$, còn phép trộn có độ phức tạp tuyến tính, phù hợp với giới hạn của bài toán.

<!-- thinking:end -->

Với một vị trí, cắt càng sớm thì số lần cắt cần thực hiện càng ít, do đó rõ ràng các vị trí có chi phí cao hơn nên được cắt trước.

Vì vậy, ta có thể sắp xếp các mảng $\textit{horizontalCut}$ và $\textit{verticalCut}$ theo thứ tự giảm dần, sau đó dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến các chi phí trong $\textit{horizontalCut}$ và $\textit{verticalCut}$. Mỗi lần, ta chọn vị trí có chi phí lớn hơn để cắt, đồng thời cập nhật số hàng và số cột tương ứng.

Mỗi khi thực hiện một nhát cắt ngang, nếu số cột trước nhát cắt là $v$, thì chi phí của nhát cắt này là $\textit{horizontalCut}[i] \times v$, sau đó số hàng $h$ tăng thêm một; tương tự, mỗi khi thực hiện một nhát cắt dọc, nếu số hàng trước nhát cắt là $h$, thì chi phí của nhát cắt này là $\textit{verticalCut}[j] \times h$, sau đó số cột $v$ tăng thêm một.

Cuối cùng, khi cả $i$ và $j$ đều đã đi đến cuối, trả về tổng chi phí.

Độ phức tạp thời gian là $O(m \times \log m + n \times \log n)$, và độ phức tạp không gian là $O(\log m + \log n)$. Ở đây, $m$ và $n$ lần lượt là độ dài của $\textit{horizontalCut}$ và $\textit{verticalCut}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCost(
        self, m: int, n: int, horizontalCut: List[int], verticalCut: List[int]
    ) -> int:
        horizontalCut.sort(reverse=True)
        verticalCut.sort(reverse=True)
        ans = i = j = 0
        h = v = 1
        while i < m - 1 or j < n - 1:
            if j == n - 1 or (i < m - 1 and horizontalCut[i] > verticalCut[j]):
                ans += horizontalCut[i] * v
                h, i = h + 1, i + 1
            else:
                ans += verticalCut[j] * h
                v, j = v + 1, j + 1
        return ans
```

#### Java

```java
class Solution {
    public long minimumCost(int m, int n, int[] horizontalCut, int[] verticalCut) {
        Arrays.sort(horizontalCut);
        Arrays.sort(verticalCut);
        long ans = 0;
        int i = m - 2, j = n - 2;
        int h = 1, v = 1;
        while (i >= 0 || j >= 0) {
            if (j < 0 || (i >= 0 && horizontalCut[i] > verticalCut[j])) {
                ans += 1L * horizontalCut[i--] * v;
                ++h;
            } else {
                ans += 1L * verticalCut[j--] * h;
                ++v;
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
    long long minimumCost(int m, int n, vector<int>& horizontalCut, vector<int>& verticalCut) {
        sort(horizontalCut.rbegin(), horizontalCut.rend());
        sort(verticalCut.rbegin(), verticalCut.rend());
        long long ans = 0;
        int i = 0, j = 0;
        int h = 1, v = 1;
        while (i < m - 1 || j < n - 1) {
            if (j == n - 1 || (i < m - 1 && horizontalCut[i] > verticalCut[j])) {
                ans += 1LL * horizontalCut[i++] * v;
                h++;
            } else {
                ans += 1LL * verticalCut[j++] * h;
                v++;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumCost(m int, n int, horizontalCut []int, verticalCut []int) (ans int64) {
	sort.Sort(sort.Reverse(sort.IntSlice(horizontalCut)))
	sort.Sort(sort.Reverse(sort.IntSlice(verticalCut)))
	i, j := 0, 0
	h, v := 1, 1
	for i < m-1 || j < n-1 {
		if j == n-1 || (i < m-1 && horizontalCut[i] > verticalCut[j]) {
			ans += int64(horizontalCut[i] * v)
			h++
			i++
		} else {
			ans += int64(verticalCut[j] * h)
			v++
			j++
		}
	}
	return
}
```

#### TypeScript

```ts
function minimumCost(m: number, n: number, horizontalCut: number[], verticalCut: number[]): number {
    horizontalCut.sort((a, b) => b - a);
    verticalCut.sort((a, b) => b - a);
    let ans = 0;
    let [i, j] = [0, 0];
    let [h, v] = [1, 1];
    while (i < m - 1 || j < n - 1) {
        if (j === n - 1 || (i < m - 1 && horizontalCut[i] > verticalCut[j])) {
            ans += horizontalCut[i] * v;
            h++;
            i++;
        } else {
            ans += verticalCut[j] * h;
            v++;
            j++;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_cost(m: i32, n: i32, mut horizontal_cut: Vec<i32>, mut vertical_cut: Vec<i32>) -> i64 {
        horizontal_cut.sort();
        vertical_cut.sort();
        let (mut i, mut j) = ((m - 2) as isize, (n - 2) as isize);
        let (mut h, mut v) = (1_i64, 1_i64);
        let mut ans: i64 = 0;

        while i >= 0 || j >= 0 {
            if j < 0 || (i >= 0 && horizontal_cut[i as usize] > vertical_cut[j as usize]) {
                ans += horizontal_cut[i as usize] as i64 * v;
                i -= 1;
                h += 1;
            } else {
                ans += vertical_cut[j as usize] as i64 * h;
                j -= 1;
                v += 1;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
