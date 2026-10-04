---
comments: true
difficulty: Medium
rating: 1448
source: Biweekly Contest 140 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [3301. Maximize the Total Height of Unique Towers](https://leetcode.com/problems/maximize-the-total-height-of-unique-towers)

[中文文档](/solution/3300-3399/3301.Maximize%20the%20Total%20Height%20of%20Unique%20Towers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>maximumHeight</code>, trong đó <code>maximumHeight[i]</code> biểu thị chiều cao <strong>tối đa</strong> có thể gán cho tòa tháp thứ <code>i<sup>th</sup></code>.</p>

<p>Nhiệm vụ của bạn là gán chiều cao cho mỗi tòa tháp sao cho:</p>

<ol>
	<li>Chiều cao của tòa tháp thứ <code>i<sup>th</sup></code> là một số nguyên dương và không vượt quá <code>maximumHeight[i]</code>.</li>
	<li>Không có hai tòa tháp nào có cùng chiều cao.</li>
</ol>

<p>Hãy trả về <strong>tổng</strong> chiều cao lớn nhất có thể của các tòa tháp. Nếu không thể gán chiều cao, hãy trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> maximumHeight<span class="example-io"> = [2,3,4,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể gán chiều cao theo cách sau: <code>[1, 2, 4, 3]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> maximumHeight<span class="example-io"> = [15,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">25</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể gán chiều cao theo cách sau: <code>[15, 10]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> maximumHeight<span class="example-io"> = [2,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể gán các chiều cao dương cho từng chỉ số sao cho không có hai tòa tháp nào trùng chiều cao.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= maximumHeight.length&nbsp;&lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= maximumHeight[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Chiều cao phải đôi một khác nhau và không vượt quá giới hạn của từng tòa tháp. Với $n \le 10^5$, việc liệt kê các cách gán là không khả thi. Nếu điền cho các tòa tháp thấp trước, ta có thể lấy mất những số lớn mà các tòa tháp cao hơn vẫn cần.
>
> Để tổng lớn nhất, ta nên gán các chiều cao lớn hơn cho những tòa tháp có giới hạn lớn hơn. Sau khi sắp xếp $\textit{maximumHeight}$ theo thứ tự giảm dần, $mx$ lưu chiều cao được gán gần nhất.
>
> Tòa tháp hiện tại nhận giá trị $\min(x, mx-1)$: giá trị này không vượt quá giới hạn của nó và nhỏ hơn nghiêm ngặt chiều cao trước đó. Nếu giá trị không dương thì không tồn tại cách gán hợp lệ.

<!-- thinking:end -->

Ta có thể sắp xếp các chiều cao tối đa của các tòa tháp theo thứ tự giảm dần, sau đó lần lượt gán chiều cao bắt đầu từ chiều cao lớn nhất. Sử dụng biến $mx$ để lưu chiều cao lớn nhất hiện tại đã được gán.

Nếu chiều cao hiện tại $x$ lớn hơn $mx - 1$, cập nhật $x$ thành $mx - 1$. Nếu $x$ nhỏ hơn hoặc bằng $0$, nghĩa là không thể gán chiều cao, khi đó trả về $-1$ ngay lập tức. Ngược lại, cộng $x$ vào đáp án và cập nhật $mx$ thành $x$.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng $\textit{maximumHeight}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumTotalSum(self, maximumHeight: List[int]) -> int:
        maximumHeight.sort()
        ans, mx = 0, inf
        for x in maximumHeight[::-1]:
            x = min(x, mx - 1)
            if x <= 0:
                return -1
            ans += x
            mx = x
        return ans
```

#### Java

```java
class Solution {
    public long maximumTotalSum(int[] maximumHeight) {
        long ans = 0;
        int mx = 1 << 30;
        Arrays.sort(maximumHeight);
        for (int i = maximumHeight.length - 1; i >= 0; --i) {
            int x = Math.min(maximumHeight[i], mx - 1);
            if (x <= 0) {
                return -1;
            }
            ans += x;
            mx = x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumTotalSum(vector<int>& maximumHeight) {
        ranges::sort(maximumHeight, greater<int>());
        long long ans = 0;
        int mx = 1 << 30;
        for (int x : maximumHeight) {
            x = min(x, mx - 1);
            if (x <= 0) {
                return -1;
            }
            ans += x;
            mx = x;
        }
        return ans;
    }
};
```

#### Go

```go
func maximumTotalSum(maximumHeight []int) int64 {
	slices.SortFunc(maximumHeight, func(a, b int) int { return b - a })
	ans := int64(0)
	mx := 1 << 30
	for _, x := range maximumHeight {
		x = min(x, mx-1)
		if x <= 0 {
			return -1
		}
		ans += int64(x)
		mx = x
	}
	return ans
}
```

#### TypeScript

```ts
function maximumTotalSum(maximumHeight: number[]): number {
    maximumHeight.sort((a, b) => a - b).reverse();
    let ans: number = 0;
    let mx: number = Infinity;
    for (let x of maximumHeight) {
        x = Math.min(x, mx - 1);
        if (x <= 0) {
            return -1;
        }
        ans += x;
        mx = x;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
