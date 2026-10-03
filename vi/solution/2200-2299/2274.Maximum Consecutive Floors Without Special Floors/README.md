---
comments: true
difficulty: Medium
rating: 1332
source: Weekly Contest 293 Q2
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [2274. Maximum Consecutive Floors Without Special Floors](https://leetcode.com/problems/maximum-consecutive-floors-without-special-floors)

[中文文档](/solution/2200-2299/2274.Maximum%20Consecutive%20Floors%20Without%20Special%20Floors/README.md)

## Mô tả

<!-- description:start -->

<p>Alice quản lý một công ty và đã thuê một số tầng trong một tòa nhà làm văn phòng. Alice quyết định một số tầng trong đó sẽ là <strong>tầng đặc biệt</strong>, chỉ được sử dụng để thư giãn.</p>

<p>Cho hai số nguyên <code>bottom</code> và <code>top</code>, biểu thị rằng Alice đã thuê tất cả các tầng từ <code>bottom</code> đến <code>top</code> (<strong>bao gồm cả hai đầu</strong>). Bạn cũng được cho mảng số nguyên <code>special</code>, trong đó <code>special[i]</code> biểu thị một tầng đặc biệt mà Alice chỉ định để thư giãn.</p>

<p>Hãy trả về <em>số lượng <strong>lớn nhất</strong> các tầng liên tiếp không có tầng đặc biệt</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> bottom = 2, top = 9, special = [4,6]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các đoạn (bao gồm cả hai đầu) gồm những tầng liên tiếp không có tầng đặc biệt là:
- (2, 3) với tổng cộng 2 tầng.
- (5, 5) với tổng cộng 1 tầng.
- (7, 9) với tổng cộng 3 tầng.
Vì vậy, ta trả về số lớn nhất là 3 tầng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> bottom = 6, top = 8, special = [7,6,8]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mọi tầng được thuê đều là tầng đặc biệt, nên ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= special.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= bottom &lt;= special[i] &lt;= top &lt;= 10<sup>9</sup></code></li>
	<li>Tất cả các giá trị của <code>special</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Các tầng đặc biệt chia $[\textit{bottom},\textit{top}]$ thành các đoạn trống; ta cần tìm đoạn dài nhất. Số tầng có thể lên tới $10^9$, nên không thể duyệt qua từng tầng. Các đoạn trống nằm giữa các tầng đặc biệt kề nhau và ở hai đầu.
>
> Sắp xếp các tầng đặc biệt; đáp án là giá trị lớn nhất trong $special[0]-bottom$, $top-special[-1]$ và $y-x-1$ với các cặp kề nhau.

<!-- thinking:end -->

Ta có thể sắp xếp các tầng đặc biệt theo thứ tự tăng dần, sau đó tính số tầng giữa mỗi cặp tầng đặc biệt kề nhau. Cuối cùng, ta tính số tầng giữa tầng đặc biệt đầu tiên và $\textit{bottom}$, cũng như giữa tầng đặc biệt cuối cùng và $\textit{top}$. Giá trị lớn nhất trong các số lượng tầng này là đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng $\textit{special}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxConsecutive(self, bottom: int, top: int, special: List[int]) -> int:
        special.sort()
        ans = max(special[0] - bottom, top - special[-1])
        for x, y in pairwise(special):
            ans = max(ans, y - x - 1)
        return ans
```

#### Java

```java
class Solution {
    public int maxConsecutive(int bottom, int top, int[] special) {
        Arrays.sort(special);
        int n = special.length;
        int ans = Math.max(special[0] - bottom, top - special[n - 1]);
        for (int i = 1; i < n; ++i) {
            ans = Math.max(ans, special[i] - special[i - 1] - 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxConsecutive(int bottom, int top, vector<int>& special) {
        ranges::sort(special);
        int ans = max(special[0] - bottom, top - special.back());
        for (int i = 1; i < special.size(); ++i) {
            ans = max(ans, special[i] - special[i - 1] - 1);
        }
        return ans;
    }
};
```

#### Go

```go
func maxConsecutive(bottom int, top int, special []int) int {
	sort.Ints(special)
	ans := max(special[0]-bottom, top-special[len(special)-1])
	for i, x := range special[1:] {
		ans = max(ans, x-special[i]-1)
	}
	return ans
}
```

#### TypeScript

```ts
function maxConsecutive(bottom: number, top: number, special: number[]): number {
    special.sort((a, b) => a - b);
    const n = special.length;
    let ans = Math.max(special[0] - bottom, top - special[n - 1]);
    for (let i = 1; i < n; ++i) {
        ans = Math.max(ans, special[i] - special[i - 1] - 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
