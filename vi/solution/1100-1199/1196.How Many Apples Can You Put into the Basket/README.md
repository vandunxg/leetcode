---
comments: true
difficulty: Easy
rating: 1248
source: Biweekly Contest 9 Q1
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [1196. How Many Apples Can You Put into the Basket 🔒](https://leetcode.com/problems/how-many-apples-can-you-put-into-the-basket)

[中文文档](/solution/1100-1199/1196.How%20Many%20Apples%20Can%20You%20Put%20into%20the%20Basket/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một số quả táo và một chiếc giỏ có thể chứa tối đa <code>5000</code> đơn vị trọng lượng.</p>

<p>Cho mảng số nguyên <code>weight</code>, trong đó <code>weight[i]</code> là trọng lượng của quả táo thứ <code>i<sup>th</sup></code>. Hãy trả về <em>số quả táo tối đa có thể cho vào giỏ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> weight = [100,200,150,1000]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Giỏ có thể chứa cả 4 quả táo vì tổng trọng lượng của chúng là 1450.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> weight = [900,950,800,1000,700,800]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Tổng trọng lượng của cả 6 quả táo vượt quá 5000, nên ta chọn 5 quả bất kỳ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= weight.length &lt;= 10<sup>3</sup></code></li>
	<li><code>1 &lt;= weight[i] &lt;= 10<sup>3</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Giỏ chứa được tổng trọng lượng tối đa $5000$, nên để chọn được nhiều táo nhất, ta ưu tiên những quả nhẹ nhất. Sắp xếp trọng lượng rồi cộng lần lượt từ nhỏ đến lớn; khi tổng vượt giới hạn thì trả về số quả đã chọn, còn nếu tất cả đều vừa thì trả về $n$.

<!-- thinking:end -->

Để tối đa số quả táo, ta cần chọn các quả có tổng trọng lượng nhỏ nhất. Vì vậy, sắp xếp trọng lượng táo rồi lần lượt cho vào giỏ theo thứ tự tăng dần cho đến khi tổng trọng lượng vượt quá $5000$. Khi đó, trả về số quả táo đã cho vào giỏ trước khi vượt giới hạn.

Nếu có thể cho tất cả táo vào giỏ, trả về tổng số quả táo.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là số quả táo.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxNumberOfApples(self, weight: List[int]) -> int:
        weight.sort()
        s = 0
        for i, x in enumerate(weight):
            s += x
            if s > 5000:
                return i
        return len(weight)
```

#### Java

```java
class Solution {
    public int maxNumberOfApples(int[] weight) {
        Arrays.sort(weight);
        int s = 0;
        for (int i = 0; i < weight.length; ++i) {
            s += weight[i];
            if (s > 5000) {
                return i;
            }
        }
        return weight.length;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxNumberOfApples(vector<int>& weight) {
        sort(weight.begin(), weight.end());
        int s = 0;
        for (int i = 0; i < weight.size(); ++i) {
            s += weight[i];
            if (s > 5000) {
                return i;
            }
        }
        return weight.size();
    }
};
```

#### Go

```go
func maxNumberOfApples(weight []int) int {
	sort.Ints(weight)
	s := 0
	for i, x := range weight {
		s += x
		if s > 5000 {
			return i
		}
	}
	return len(weight)
}
```

#### TypeScript

```ts
function maxNumberOfApples(weight: number[]): number {
    weight.sort((a, b) => a - b);
    let s = 0;
    for (let i = 0; i < weight.length; ++i) {
        s += weight[i];
        if (s > 5000) {
            return i;
        }
    }
    return weight.length;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
