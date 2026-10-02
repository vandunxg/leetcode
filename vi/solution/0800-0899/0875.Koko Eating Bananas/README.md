---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas)

[中文文档](/solution/0800-0899/0875.Koko%20Eating%20Bananas/README.md)

## Mô tả

<!-- description:start -->

<p>Koko thích ăn chuối. Có <code>n</code> đống chuối, đống thứ <code>i<sup>th</sup></code> có <code>piles[i]</code> quả. Những người canh gác đã rời đi và sẽ quay lại sau <code>h</code> giờ.</p>

<p>Koko có thể chọn tốc độ ăn <code>k</code> quả chuối mỗi giờ. Mỗi giờ, cô chọn một đống chuối và ăn <code>k</code> quả từ đống đó. Nếu đống có ít hơn <code>k</code> quả, cô ăn hết số chuối còn lại và không ăn thêm chuối nào trong giờ đó.</p>

<p>Koko muốn ăn chậm nhất có thể nhưng vẫn phải ăn hết chuối trước khi những người canh gác quay lại.</p>

<p>Hãy trả về số nguyên <code>k</code> <em>nhỏ nhất sao cho cô ấy có thể ăn hết chuối trong vòng</em> <code>h</code> <em>giờ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> piles = [3,6,7,11], h = 8
<strong>Đầu ra:</strong> 4
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> piles = [30,11,23,4,20], h = 5
<strong>Đầu ra:</strong> 30
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> piles = [30,11,23,4,20], h = 6
<strong>Đầu ra:</strong> 23
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= piles.length &lt;= 10<sup>4</sup></code></li>
	<li><code>piles.length &lt;= h &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= piles[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Tốc độ $k$ càng lớn thì ăn xong càng sớm; ta cần tìm $k$ nhỏ nhất để hoàn thành trong $h$ giờ. $h$ và số chuối trong mỗi đống có thể lên đến $10^9$, nên ta tìm kiếm nhị phân trên điều kiện đơn điệu “có thể ăn hết trong $h$ giờ”.
>
> Tìm kiếm trên đoạn $[1,\max piles]$; hàm kiểm tra tính tổng $\lceil x/k\rceil$ trên tất cả các đống. Tốc độ nhỏ nhất thỏa mãn điều kiện là đáp án.

<!-- thinking:end -->

Ta nhận thấy nếu Koko có thể ăn hết chuối với tốc độ $k$ trong $h$ giờ thì cô ấy cũng có thể ăn hết chuối với tốc độ $k' > k$ trong $h$ giờ. Điều này cho thấy tính đơn điệu, vì vậy ta có thể dùng tìm kiếm nhị phân để tìm $k$ nhỏ nhất thỏa mãn điều kiện.

Ta đặt biên trái tìm kiếm nhị phân là $l = 1$ và biên phải là $r = \max(\textit{piles})$. Mỗi lượt, ta lấy giá trị giữa $mid = \frac{l + r}{2}$ rồi tính thời gian $s$ cần để ăn hết chuối với tốc độ $mid$. Nếu $s \leq h$, tốc độ $mid$ thỏa mãn điều kiện nên ta cập nhật biên phải $r$ thành $mid$; nếu không, cập nhật biên trái $l$ thành $mid + 1$. Cuối cùng, khi $l = r$, ta tìm được $k$ nhỏ nhất thỏa mãn điều kiện.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ là độ dài và $M$ là giá trị lớn nhất của mảng $\textit{piles}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        def check(k: int) -> bool:
            return sum((x + k - 1) // k for x in piles) <= h

        return 1 + bisect_left(range(1, max(piles) + 1), True, key=check)
```

#### Java

```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int l = 1, r = (int) 1e9;
        while (l < r) {
            int mid = (l + r) >> 1;
            int s = 0;
            for (int x : piles) {
                s += (x + mid - 1) / mid;
            }
            if (s <= h) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int l = 1, r = ranges::max(piles);
        while (l < r) {
            int mid = (l + r) >> 1;
            int s = 0;
            for (int x : piles) {
                s += (x + mid - 1) / mid;
            }
            if (s <= h) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func minEatingSpeed(piles []int, h int) int {
	return 1 + sort.Search(slices.Max(piles), func(k int) bool {
		k++
		s := 0
		for _, x := range piles {
			s += (x + k - 1) / k
		}
		return s <= h
	})
}
```

#### TypeScript

```ts
function minEatingSpeed(piles: number[], h: number): number {
    let [l, r] = [1, Math.max(...piles)];
    while (l < r) {
        const mid = (l + r) >> 1;
        const s = piles.map(x => Math.ceil(x / mid)).reduce((a, b) => a + b);
        if (s <= h) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_eating_speed(piles: Vec<i32>, h: i32) -> i32 {
        let mut l = 1;
        let mut r = *piles.iter().max().unwrap_or(&0);
        while l < r {
            let mid = (l + r) >> 1;
            let mut s = 0;
            for x in piles.iter() {
                s += (x + mid - 1) / mid;
            }
            if s <= h {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        l
    }
}
```

#### C#

```cs
public class Solution {
    public int MinEatingSpeed(int[] piles, int h) {
        int l = 1, r = (int) 1e9;
        while (l < r) {
            int mid = (l + r) >> 1;
            int s = 0;
            foreach (int x in piles) {
                s += (x + mid - 1) / mid;
            }
            if (s <= h) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
