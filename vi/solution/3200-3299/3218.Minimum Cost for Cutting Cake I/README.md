---
comments: true
difficulty: Medium
rating: 1654
source: Weekly Contest 406 Q3
tags:
    - Greedy
    - Array
    - Two Pointers
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [3218. Minimum Cost for Cutting Cake I](https://leetcode.com/problems/minimum-cost-for-cutting-cake-i)

[中文文档](/solution/3200-3299/3218.Minimum%20Cost%20for%20Cutting%20Cake%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Có một chiếc bánh kích thước <code>m x n</code> cần được cắt thành các miếng <code>1 x 1</code>.</p>

<p>Cho các số nguyên <code>m</code>, <code>n</code> và hai mảng:</p>

<ul>
	<li><code>horizontalCut</code> có kích thước <code>m - 1</code>, trong đó <code>horizontalCut[i]</code> là chi phí cắt theo đường ngang <code>i</code>.</li>
	<li><code>verticalCut</code> có kích thước <code>n - 1</code>, trong đó <code>verticalCut[j]</code> là chi phí cắt theo đường dọc <code>j</code>.</li>
</ul>

<p>Trong một thao tác, bạn có thể chọn bất kỳ miếng bánh nào chưa phải là hình vuông <code>1 x 1</code> và thực hiện một trong các lần cắt sau:</p>

<ol>
	<li>Cắt theo đường ngang <code>i</code> với chi phí <code>horizontalCut[i]</code>.</li>
	<li>Cắt theo đường dọc <code>j</code> với chi phí <code>verticalCut[j]</code>.</li>
</ol>

<p>Sau khi cắt, miếng bánh được chia thành hai miếng riêng biệt.</p>

<p>Chi phí của một lần cắt chỉ phụ thuộc vào chi phí ban đầu của đường cắt và không thay đổi.</p>

<p>Trả về <strong>tổng chi phí nhỏ nhất</strong> để cắt toàn bộ bánh thành các miếng <code>1 x 1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 3, n = 2, horizontalCut = [1,3], verticalCut = [5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">13</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3218.Minimum%20Cost%20for%20Cutting%20Cake%20I/images/ezgifcom-animated-gif-maker-1.gif" style="width: 280px; height: 320px;" /></p>

<ul>
	<li>Cắt theo đường dọc 0 với chi phí 5, tổng chi phí hiện tại là 5.</li>
	<li>Cắt theo đường ngang 0 trên lưới con <code>3 x 1</code> với chi phí 1.</li>
	<li>Cắt theo đường ngang 0 trên lưới con <code>3 x 1</code> với chi phí 1.</li>
	<li>Cắt theo đường ngang 1 trên lưới con <code>2 x 1</code> với chi phí 3.</li>
	<li>Cắt theo đường ngang 1 trên lưới con <code>2 x 1</code> với chi phí 3.</li>
</ul>

<p>Tổng chi phí là <code>5 + 1 + 1 + 3 + 3 = 13</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 2, n = 2, horizontalCut = [7], verticalCut = [4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Cắt theo đường ngang 0 với chi phí 7.</li>
	<li>Cắt theo đường dọc 0 trên lưới con <code>1 x 2</code> với chi phí 4.</li>
	<li>Cắt theo đường dọc 0 trên lưới con <code>1 x 2</code> với chi phí 4.</li>
</ul>

<p>Tổng chi phí là <code>7 + 4 + 4 = 15</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 20</code></li>
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
> Chi phí của mỗi lần cắt được nhân với số miếng đã có theo hướng còn lại, nên cắt càng muộn thì chi phí càng lớn. Với $m,n\le 20$, ta có thể dùng quy hoạch động, nhưng thứ tự tối ưu có một quy luật đơn giản.
>
> Các đường cắt đắt nên được thực hiện sớm, khi hệ số nhân còn nhỏ. Sắp xếp giảm dần hai mảng chi phí và luôn chọn đường cắt hiện tại có chi phí lớn hơn: một lần cắt ngang được nhân với số miếng dọc $v$, còn một lần cắt dọc được nhân với $h$, sau đó cập nhật số lượng tương ứng. Thứ tự tham lam này khớp với quá trình mô phỏng.

<!-- thinking:end -->

Với một vị trí bất kỳ, cắt càng sớm thì càng cần ít lần cắt hơn, nên rõ ràng là các vị trí có chi phí cao hơn nên được cắt trước.

Do đó, ta có thể sắp xếp các mảng $\textit{horizontalCut}$ và $\textit{verticalCut}$ theo thứ tự giảm dần, sau đó dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến các chi phí trong $\textit{horizontalCut}$ và $\textit{verticalCut}$. Mỗi lần, ta chọn vị trí có chi phí lớn hơn để cắt, đồng thời cập nhật số hàng và số cột tương ứng.

Mỗi khi thực hiện một lần cắt ngang, nếu số cột trước khi cắt là $v$, thì chi phí của lần cắt này là $\textit{horizontalCut}[i] \times v$, sau đó số hàng $h$ tăng lên một; tương tự, mỗi khi thực hiện một lần cắt dọc, nếu số hàng trước khi cắt là $h$, thì chi phí của lần cắt này là $\textit{verticalCut}[j] \times h$, sau đó số cột $v$ tăng lên một.

Cuối cùng, khi cả $i$ và $j$ đều đã đi đến cuối mảng, trả về tổng chi phí.

Độ phức tạp thời gian là $O(m \times \log m + n \times \log n)$, và độ phức tạp không gian là $O(\log m + \log n)$. Trong đó, $m$ và $n$ lần lượt là độ dài của $\textit{horizontalCut}$ và $\textit{verticalCut}$.

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
    public int minimumCost(int m, int n, int[] horizontalCut, int[] verticalCut) {
        Arrays.sort(horizontalCut);
        Arrays.sort(verticalCut);
        int ans = 0;
        int i = m - 2, j = n - 2;
        int h = 1, v = 1;
        while (i >= 0 || j >= 0) {
            if (j < 0 || (i >= 0 && horizontalCut[i] > verticalCut[j])) {
                ans += horizontalCut[i--] * v;
                ++h;
            } else {
                ans += verticalCut[j--] * h;
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
    int minimumCost(int m, int n, vector<int>& horizontalCut, vector<int>& verticalCut) {
        sort(horizontalCut.rbegin(), horizontalCut.rend());
        sort(verticalCut.rbegin(), verticalCut.rend());
        int ans = 0;
        int i = 0, j = 0;
        int h = 1, v = 1;
        while (i < m - 1 || j < n - 1) {
            if (j == n - 1 || (i < m - 1 && horizontalCut[i] > verticalCut[j])) {
                ans += horizontalCut[i++] * v;
                h++;
            } else {
                ans += verticalCut[j++] * h;
                v++;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumCost(m int, n int, horizontalCut []int, verticalCut []int) (ans int) {
	sort.Sort(sort.Reverse(sort.IntSlice(horizontalCut)))
	sort.Sort(sort.Reverse(sort.IntSlice(verticalCut)))
	i, j := 0, 0
	h, v := 1, 1
	for i < m-1 || j < n-1 {
		if j == n-1 || (i < m-1 && horizontalCut[i] > verticalCut[j]) {
			ans += horizontalCut[i] * v
			h++
			i++
		} else {
			ans += verticalCut[j] * h
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
    pub fn minimum_cost(m: i32, n: i32, mut horizontal_cut: Vec<i32>, mut vertical_cut: Vec<i32>) -> i32 {
        horizontal_cut.sort();
        vertical_cut.sort();
        let (mut i, mut j) = ((m - 2) as isize, (n - 2) as isize);
        let (mut h, mut v) = (1, 1);
        let mut ans = 0;

        while i >= 0 || j >= 0 {
            if j < 0 || (i >= 0 && horizontal_cut[i as usize] > vertical_cut[j as usize]) {
                ans += horizontal_cut[i as usize] * v;
                i -= 1;
                h += 1;
            } else {
                ans += vertical_cut[j as usize] * h;
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
