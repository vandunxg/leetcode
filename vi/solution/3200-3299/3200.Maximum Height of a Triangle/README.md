---
comments: true
difficulty: Easy
rating: 1451
source: Weekly Contest 404 Q1
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [3200. Maximum Height of a Triangle](https://leetcode.com/problems/maximum-height-of-a-triangle)

[Tài liệu tiếng Trung](/solution/3200-3299/3200.Maximum%20Height%20of%20a%20Triangle/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <code>red</code> và <code>blue</code>, lần lượt biểu thị số lượng quả bóng màu đỏ và xanh dương. Bạn phải sắp xếp các quả bóng này thành một tam giác sao cho hàng 1<sup>st</sup> có 1 quả bóng, hàng 2<sup>nd</sup> có 2 quả bóng, hàng 3<sup>rd</sup> có 3 quả bóng, v.v.</p>

<p>Mọi quả bóng trong cùng một hàng phải có <strong>cùng</strong> màu, và các hàng liền kề phải có <strong>màu khác nhau</strong>.</p>

<p>Trả về <strong>chiều cao lớn nhất</strong><em> của tam giác</em> có thể tạo được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">red = 2, blue = 4</span></p>

<p><strong>Đầu ra:</strong> 3</p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3200.Maximum%20Height%20of%20a%20Triangle/images/brb.png" style="width: 300px; height: 240px; padding: 10px;" /></p>

<p>Cách sắp xếp duy nhất có thể là cách được minh họa ở trên.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">red = 2, blue = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3200.Maximum%20Height%20of%20a%20Triangle/images/br.png" style="width: 150px; height: 135px; padding: 10px;" /><br />
Cách sắp xếp duy nhất có thể là cách được minh họa ở trên.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">red = 1, blue = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">red = 10, blue = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3200.Maximum%20Height%20of%20a%20Triangle/images/br.png" style="width: 150px; height: 135px; padding: 10px;" /><br />
Cách sắp xếp duy nhất có thể là cách được minh họa ở trên.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= red, blue &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một tam giác có chiều cao $h$ sử dụng $h(h+1)/2$ quả bóng, nên $h$ không vượt quá $O(\sqrt{\textit{red}+\textit{blue}})$. Với $1\le \textit{red},\textit{blue}\le 100$, ta có thể liệt kê các chiều cao và màu của các hàng, nhưng các hàng liền kề phải khác màu, nên toàn bộ cách tô màu được xác định ngay khi chọn màu của hàng đầu tiên.
>
> Do đó chỉ cần thử hai trường hợp hàng đầu tiên màu đỏ và màu xanh dương, rồi lần lượt trừ $1,2,\ldots$ quả bóng khỏi số lượng còn lại của hai màu. Dừng khi hàng tiếp theo có số bóng lớn hơn số lượng còn lại của màu tương ứng, và giữ lại chiều cao khả thi lớn hơn. Việc đảo chỉ số màu bằng XOR giúp mô phỏng với bộ nhớ phụ hằng số.

<!-- thinking:end -->

Có thể liệt kê màu của hàng đầu tiên, sau đó mô phỏng việc xây dựng tam giác để tính chiều cao lớn nhất.

Độ phức tạp thời gian là $O(\sqrt{n})$, trong đó $n$ là tổng số quả bóng đỏ và xanh dương. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxHeightOfTriangle(self, red: int, blue: int) -> int:
        ans = 0
        for k in range(2):
            c = [red, blue]
            i, j = 1, k
            while i <= c[j]:
                c[j] -= i
                j ^= 1
                ans = max(ans, i)
                i += 1
        return ans
```

#### Java

```java
class Solution {
    public int maxHeightOfTriangle(int red, int blue) {
        int ans = 0;
        for (int k = 0; k < 2; ++k) {
            int[] c = {red, blue};
            for (int i = 1, j = k; i <= c[j]; j ^= 1, ++i) {
                c[j] -= i;
                ans = Math.max(ans, i);
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
    int maxHeightOfTriangle(int red, int blue) {
        int ans = 0;
        for (int k = 0; k < 2; ++k) {
            int c[2] = {red, blue};
            for (int i = 1, j = k; i <= c[j]; j ^= 1, ++i) {
                c[j] -= i;
                ans = max(ans, i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxHeightOfTriangle(red int, blue int) (ans int) {
    for k := 0; k < 2; k++ {
        c := [2]int{red, blue}
        for i, j := 1, k; i <= c[j]; i, j = i+1, j^1 {
            c[j] -= i
            ans = max(ans, i)
        }
    }
    return
}
```

#### TypeScript

```ts
function maxHeightOfTriangle(red: number, blue: number): number {
    let ans = 0;
    for (let k = 0; k < 2; ++k) {
        const c: [number, number] = [red, blue];
        for (let i = 1, j = k; i <= c[j]; ++i, j ^= 1) {
            c[j] -= i;
            ans = Math.max(ans, i);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
