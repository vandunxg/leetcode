---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2431. Maximize Total Tastiness of Purchased Fruits 🔒](https://leetcode.com/problems/maximize-total-tastiness-of-purchased-fruits)

[中文文档](/solution/2400-2499/2431.Maximize%20Total%20Tastiness%20of%20Purchased%20Fruits/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên không âm <code>price</code> và <code>tastiness</code>, cả hai mảng có cùng độ dài <code>n</code>. Bạn cũng được cho hai số nguyên không âm <code>maxAmount</code> và <code>maxCoupons</code>.</p>

<p>Với mỗi số nguyên <code>i</code> trong đoạn <code>[0, n - 1]</code>:</p>

<ul>
	<li><code>price[i]</code> biểu thị giá của loại trái cây thứ <code>i<sup>th</sup></code>.</li>
	<li><code>tastiness[i]</code> biểu thị độ ngon của loại trái cây thứ <code>i<sup>th</sup></code>.</li>
</ul>

<p>Bạn muốn mua một số loại trái cây sao cho tổng độ ngon là lớn nhất và tổng giá không vượt quá <code>maxAmount</code>.</p>

<p>Ngoài ra, bạn có thể dùng một coupon để mua trái cây với <strong>một nửa giá</strong> (làm tròn xuống số nguyên gần nhất). Bạn có thể sử dụng nhiều nhất <code>maxCoupons</code> coupon.</p>

<p>Hãy trả về <em>tổng độ ngon tối đa có thể mua được</em>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Bạn chỉ có thể mua mỗi loại trái cây nhiều nhất một lần.</li>
	<li>Bạn chỉ có thể sử dụng coupon trên mỗi loại trái cây nhiều nhất một lần.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> price = [10,20,20], tastiness = [5,8,8], maxAmount = 20, maxCoupons = 1
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Có thể đạt tổng độ ngon bằng 13 theo cách sau:
- Mua loại trái cây đầu tiên không dùng coupon, khi đó tổng giá = 0 + 10 và tổng độ ngon = 0 + 5.
- Mua loại trái cây thứ hai bằng coupon, khi đó tổng giá = 10 + 10 và tổng độ ngon = 5 + 8.
- Không mua loại trái cây thứ ba, khi đó tổng giá = 20 và tổng độ ngon = 13.
Có thể chứng minh rằng 13 là tổng độ ngon tối đa có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> price = [10,15,7], tastiness = [5,8,20], maxAmount = 10, maxCoupons = 2
<strong>Đầu ra:</strong> 28
<strong>Giải thích:</strong> Có thể đạt tổng độ ngon bằng 20 theo cách sau:
- Không mua loại trái cây đầu tiên, khi đó tổng giá = 0 và tổng độ ngon = 0.
- Mua loại trái cây thứ hai bằng coupon, khi đó tổng giá = 0 + 7 và tổng độ ngon = 0 + 8.
- Mua loại trái cây thứ ba bằng coupon, khi đó tổng giá = 7 + 3 và tổng độ ngon = 8 + 20.
Có thể chứng minh rằng 28 là tổng độ ngon tối đa có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == price.length == tastiness.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>0 &lt;= price[i], tastiness[i], maxAmount &lt;= 1000</code></li>
	<li><code>0 &lt;= maxCoupons &lt;= 5</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi loại trái cây, ta có thể bỏ qua, mua với giá đầy đủ hoặc mua với một nửa giá bằng coupon. Trạng thái $(i,j,k)$ lần lượt là chỉ số, số tiền còn lại và số coupon còn lại. Tích $n\times\textit{maxAmount}\times\textit{maxCoupons}$ đủ nhỏ để ghi nhớ các trạng thái.
>
> Các chuyển trạng thái là bỏ qua, trả đủ giá nếu $j$ cho phép, hoặc dùng một coupon với giá $\lfloor price/2\rfloor$. Ta lấy tổng độ ngon lớn nhất.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i, j, k)$ biểu diễn tổng độ ngon tối đa khi bắt đầu từ loại trái cây thứ $i$, với $j$ tiền và $k$ coupon còn lại.

Với loại trái cây thứ $i$, ta có thể chọn mua hoặc không mua. Nếu chọn mua, ta có thể quyết định có dùng coupon hay không.

Nếu không mua, tổng độ ngon tối đa là $dfs(i + 1, j, k)$;

Nếu mua và không dùng coupon (cần $j\ge price[i]$), tổng độ ngon tối đa là $dfs(i + 1, j - price[i], k) + tastiness[i]$; nếu dùng coupon (cần $k\gt 0$ và $j\ge \lfloor \frac{price[i]}{2} \rfloor$), tổng độ ngon tối đa là $dfs(i + 1, j - \lfloor \frac{price[i]}{2} \rfloor, k - 1) + tastiness[i]$.

Đáp án cuối cùng là $dfs(0, maxAmount, maxCoupons)$.

Độ phức tạp thời gian là $O(n \times maxAmount \times maxCoupons)$, trong đó $n$ là số loại trái cây.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTastiness(
        self, price: List[int], tastiness: List[int], maxAmount: int, maxCoupons: int
    ) -> int:
        @cache
        def dfs(i, j, k):
            if i == len(price):
                return 0
            ans = dfs(i + 1, j, k)
            if j >= price[i]:
                ans = max(ans, dfs(i + 1, j - price[i], k) + tastiness[i])
            if j >= price[i] // 2 and k:
                ans = max(ans, dfs(i + 1, j - price[i] // 2, k - 1) + tastiness[i])
            return ans

        return dfs(0, maxAmount, maxCoupons)
```

#### Java

```java
class Solution {
    private int[][][] f;
    private int[] price;
    private int[] tastiness;
    private int n;

    public int maxTastiness(int[] price, int[] tastiness, int maxAmount, int maxCoupons) {
        n = price.length;
        this.price = price;
        this.tastiness = tastiness;
        f = new int[n][maxAmount + 1][maxCoupons + 1];
        return dfs(0, maxAmount, maxCoupons);
    }

    private int dfs(int i, int j, int k) {
        if (i == n) {
            return 0;
        }
        if (f[i][j][k] != 0) {
            return f[i][j][k];
        }
        int ans = dfs(i + 1, j, k);
        if (j >= price[i]) {
            ans = Math.max(ans, dfs(i + 1, j - price[i], k) + tastiness[i]);
        }
        if (j >= price[i] / 2 && k > 0) {
            ans = Math.max(ans, dfs(i + 1, j - price[i] / 2, k - 1) + tastiness[i]);
        }
        f[i][j][k] = ans;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxTastiness(vector<int>& price, vector<int>& tastiness, int maxAmount, int maxCoupons) {
        int n = price.size();
        memset(f, 0, sizeof f);
        function<int(int i, int j, int k)> dfs;
        dfs = [&](int i, int j, int k) {
            if (i == n) return 0;
            if (f[i][j][k]) return f[i][j][k];
            int ans = dfs(i + 1, j, k);
            if (j >= price[i]) ans = max(ans, dfs(i + 1, j - price[i], k) + tastiness[i]);
            if (j >= price[i] / 2 && k) ans = max(ans, dfs(i + 1, j - price[i] / 2, k - 1) + tastiness[i]);
            f[i][j][k] = ans;
            return ans;
        };
        return dfs(0, maxAmount, maxCoupons);
    }

private:
    int f[101][1001][6];
};
```

#### Go

```go
func maxTastiness(price []int, tastiness []int, maxAmount int, maxCoupons int) int {
	n := len(price)
	f := make([][][]int, n+1)
	for i := range f {
		f[i] = make([][]int, maxAmount+1)
		for j := range f[i] {
			f[i][j] = make([]int, maxCoupons+1)
		}
	}
	var dfs func(i, j, k int) int
	dfs = func(i, j, k int) int {
		if i == n {
			return 0
		}
		if f[i][j][k] != 0 {
			return f[i][j][k]
		}
		ans := dfs(i+1, j, k)
		if j >= price[i] {
			ans = max(ans, dfs(i+1, j-price[i], k)+tastiness[i])
		}
		if j >= price[i]/2 && k > 0 {
			ans = max(ans, dfs(i+1, j-price[i]/2, k-1)+tastiness[i])
		}
		f[i][j][k] = ans
		return ans
	}
	return dfs(0, maxAmount, maxCoupons)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
