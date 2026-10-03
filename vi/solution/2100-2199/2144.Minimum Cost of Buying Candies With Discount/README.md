---
comments: true
difficulty: Easy
rating: 1260
source: Biweekly Contest 70 Q1
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [2144. Minimum Cost of Buying Candies With Discount](https://leetcode.com/problems/minimum-cost-of-buying-candies-with-discount)

[中文文档](/solution/2100-2199/2144.Minimum%20Cost%20of%20Buying%20Candies%20With%20Discount/README.md)

## Mô tả

<!-- description:start -->

<p>Một cửa hàng đang bán kẹo với chương trình giảm giá. Với <strong>mỗi hai</strong> viên kẹo được bán, cửa hàng tặng <strong>viên thứ ba</strong> <strong>miễn phí</strong>.</p>

<p>Khách hàng có thể chọn <strong>bất kỳ</strong> viên kẹo nào để lấy miễn phí, miễn là giá của viên được chọn nhỏ hơn hoặc bằng <strong>giá thấp nhất</strong> của hai viên kẹo đã mua.</p>

<ul>
	<li>Ví dụ, nếu có <code>4</code> viên kẹo với giá lần lượt là <code>1</code>, <code>2</code>, <code>3</code> và <code>4</code>, khách hàng mua hai viên có giá <code>2</code> và <code>3</code> thì có thể lấy viên giá <code>1</code> miễn phí, nhưng không thể lấy viên giá <code>4</code>.</li>
</ul>

<p>Cho một mảng số nguyên <code>cost</code> được <strong>đánh chỉ số từ 0</strong>, trong đó <code>cost[i]</code> là giá của viên kẹo thứ <code>i<sup>th</sup></code>, hãy trả về <em><strong>chi phí nhỏ nhất</strong> để mua <strong>tất cả</strong> các viên kẹo</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> cost = [1,2,3]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Ta mua hai viên có giá 2 và 3, rồi lấy viên có giá 1 miễn phí.
Tổng chi phí để mua tất cả các viên kẹo là 2 + 3 = 5. Đây là <strong>cách duy nhất</strong> để mua các viên kẹo.
Lưu ý rằng ta không thể mua hai viên có giá 1 và 3 rồi lấy viên có giá 2 miễn phí.
Giá của viên kẹo miễn phí phải nhỏ hơn hoặc bằng giá thấp nhất của các viên kẹo đã mua.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> cost = [6,5,7,9,2,2]
<strong>Đầu ra:</strong> 23
<strong>Giải thích:</strong> Cách để đạt được chi phí nhỏ nhất được mô tả dưới đây:
- Mua hai viên có giá 9 và 7
- Lấy viên có giá 6 miễn phí
- Mua hai viên có giá 5 và 2
- Lấy viên có giá 2 còn lại miễn phí
Vì vậy, chi phí nhỏ nhất để mua tất cả các viên kẹo là 9 + 7 + 5 + 2 = 23.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> cost = [5,5]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Vì chỉ có 2 viên kẹo, ta phải mua cả hai viên. Không có viên thứ ba để lấy miễn phí.
Vì vậy, chi phí nhỏ nhất để mua tất cả các viên kẹo là 5 + 5 = 10.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= cost.length &lt;= 100</code></li>
	<li><code>1 &lt;= cost[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi hai viên kẹo trả tiền, ta được lấy miễn phí một viên không đắt hơn viên rẻ hơn trong hai viên đã mua. Để tối đa hóa giá trị các viên được miễn phí, ta nên trả tiền cho các viên đắt trước. Không cần thử các cách chia nhóm.
>
> Sau khi sắp xếp giá theo thứ tự giảm dần, cứ viên kẹo thứ ba là miễn phí, nên chi phí bằng tổng giá trừ các vị trí đó.
>
> Sắp xếp rồi trừ $\textit{cost}[2::3]$ khỏi tổng.

<!-- thinking:end -->

Trước hết, ta có thể sắp xếp các viên kẹo theo giá giảm dần, sau đó cứ ba viên thì lấy hai viên. Cách này đảm bảo các viên được miễn phí là những viên đắt nhất, từ đó tổng chi phí là nhỏ nhất.

Độ phức tạp thời gian là $O(n \log n)$, và độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là số lượng viên kẹo.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCost(self, cost: List[int]) -> int:
        cost.sort(reverse=True)
        return sum(cost) - sum(cost[2::3])
```

#### Java

```java
class Solution {
    public int minimumCost(int[] cost) {
        Arrays.sort(cost);
        int ans = 0;
        for (int i = cost.length - 1; i >= 0; i -= 3) {
            ans += cost[i];
            if (i > 0) {
                ans += cost[i - 1];
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
    int minimumCost(vector<int>& cost) {
        sort(cost.rbegin(), cost.rend());
        int ans = 0;
        for (int i = 0; i < cost.size(); i += 3) {
            ans += cost[i];
            if (i < cost.size() - 1) {
                ans += cost[i + 1];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumCost(cost []int) (ans int) {
	sort.Ints(cost)
	for i := len(cost) - 1; i >= 0; i -= 3 {
		ans += cost[i]
		if i > 0 {
			ans += cost[i-1]
		}
	}
	return
}
```

#### TypeScript

```ts
function minimumCost(cost: number[]): number {
    cost.sort((a, b) => a - b);
    let ans = 0;
    for (let i = cost.length - 1; i >= 0; i -= 3) {
        ans += cost[i];
        if (i) {
            ans += cost[i - 1];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
