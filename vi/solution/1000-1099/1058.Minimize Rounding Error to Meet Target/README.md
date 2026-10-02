---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Math
    - String
    - Sorting
---

<!-- problem:start -->

# [1058. Minimize Rounding Error to Meet Target 🔒](https://leetcode.com/problems/minimize-rounding-error-to-meet-target)

[中文文档](/solution/1000-1099/1058.Minimize%20Rounding%20Error%20to%20Meet%20Target/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>prices</code> <code>[p<sub>1</sub>,p<sub>2</sub>...,p<sub>n</sub>]</code> và một <code>target</code>, hãy làm tròn từng giá <code>p<sub>i</sub></code> thành <code>Round<sub>i</sub>(p<sub>i</sub>)</code> sao cho tổng mảng sau khi làm tròn <code>[Round<sub>1</sub>(p<sub>1</sub>),Round<sub>2</sub>(p<sub>2</sub>)...,Round<sub>n</sub>(p<sub>n</sub>)]</code> bằng <code>target</code>. Mỗi phép làm tròn <code>Round<sub>i</sub>(p<sub>i</sub>)</code> có thể là <code>Floor(p<sub>i</sub>)</code> hoặc <code>Ceil(p<sub>i</sub>)</code>.</p>

<p>Trả về chuỗi <code>&quot;-1&quot;</code> nếu không thể làm tròn mảng để tổng bằng <code>target</code>. Nếu có thể, hãy trả về sai số làm tròn nhỏ nhất, được định nghĩa là <code>&Sigma; |Round<sub>i</sub>(p<sub>i</sub>) - (p<sub>i</sub>)|</code> với <italic><code>i</code></italic> từ <code>1</code> đến <italic><code>n</code></italic>, dưới dạng chuỗi có ba chữ số sau dấu thập phân.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [&quot;0.700&quot;,&quot;2.800&quot;,&quot;4.900&quot;], target = 8
<strong>Đầu ra:</strong> &quot;1.000&quot;
<strong>Giải thích:</strong>
Dùng lần lượt các phép Floor, Ceil và Ceil để có (0.7 - 0) + (3 - 2.8) + (5 - 4.9) = 0.7 + 0.2 + 0.1 = 1.0 .
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [&quot;1.500&quot;,&quot;2.500&quot;,&quot;3.500&quot;], target = 10
<strong>Đầu ra:</strong> &quot;-1&quot;
<strong>Giải thích:</strong> Không thể đạt được target.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [&quot;1.500&quot;,&quot;2.500&quot;,&quot;3.500&quot;], target = 9
<strong>Đầu ra:</strong> &quot;1.500&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= prices.length &lt;= 500</code></li>
	<li>Mỗi chuỗi&nbsp;<code>prices[i]</code> biểu diễn một số thực trong đoạn <code>[0.0, 1000.0]</code> và có đúng 3 chữ số thập phân.</li>
	<li><code>0 &lt;= target &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi giá, ta có thể lấy Floor hoặc Ceil nếu có phần thập phân. Tổng khi tất cả đều lấy Floor là $\textit{mi}$; số giá có thể làm tròn lên bằng số phần thập phân khác 0. Vì vậy, $target$ phải nằm trong khoảng tương ứng.
>
> Cần làm tròn lên đúng $d=\textit{target}-\textit{mi}$ giá. Sai số khi lấy Ceil là $1-\{p\}$, còn khi lấy Floor là $\{p\}$, nên ta chọn $d$ phần thập phân lớn nhất để làm tròn lên.
>
> Sắp xếp các phần thập phân đó theo thứ tự giảm dần để tính tổng sai số và định dạng đến ba chữ số thập phân.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimizeError(self, prices: List[str], target: int) -> str:
        mi = 0
        arr = []
        for p in prices:
            p = float(p)
            mi += int(p)
            if d := p - int(p):
                arr.append(d)
        if not mi <= target <= mi + len(arr):
            return "-1"
        d = target - mi
        arr.sort(reverse=True)
        ans = d - sum(arr[:d]) + sum(arr[d:])
        return f'{ans:.3f}'
```

#### Java

```java
class Solution {
    public String minimizeError(String[] prices, int target) {
        int mi = 0;
        List<Double> arr = new ArrayList<>();
        for (String p : prices) {
            double price = Double.valueOf(p);
            mi += (int) price;
            double d = price - (int) price;
            if (d > 0) {
                arr.add(d);
            }
        }
        if (target < mi || target > mi + arr.size()) {
            return "-1";
        }
        int d = target - mi;
        arr.sort(Collections.reverseOrder());
        double ans = d;
        for (int i = 0; i < d; ++i) {
            ans -= arr.get(i);
        }
        for (int i = d; i < arr.size(); ++i) {
            ans += arr.get(i);
        }
        DecimalFormat df = new DecimalFormat("#0.000");
        return df.format(ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string minimizeError(vector<string>& prices, int target) {
        int mi = 0;
        vector<double> arr;
        for (auto& p : prices) {
            double price = stod(p);
            mi += (int) price;
            double d = price - (int) price;
            if (d > 0) {
                arr.push_back(d);
            }
        }
        if (target < mi || target > mi + arr.size()) {
            return "-1";
        }
        int d = target - mi;
        sort(arr.rbegin(), arr.rend());
        double ans = d;
        for (int i = 0; i < d; ++i) {
            ans -= arr[i];
        }
        for (int i = d; i < arr.size(); ++i) {
            ans += arr[i];
        }
        string s = to_string(ans);
        return s.substr(0, s.find('.') + 4);
    }
};
```

#### Go

```go
func minimizeError(prices []string, target int) string {
	arr := []float64{}
	mi := 0
	for _, p := range prices {
		price, _ := strconv.ParseFloat(p, 64)
		mi += int(math.Floor(price))
		d := price - float64(math.Floor(price))
		if d > 0 {
			arr = append(arr, d)
		}
	}
	if target < mi || target > mi+len(arr) {
		return "-1"
	}
	d := target - mi
	sort.Float64s(arr)
	ans := float64(d)
	for i := 0; i < d; i++ {
		ans -= arr[len(arr)-i-1]
	}
	for i := d; i < len(arr); i++ {
		ans += arr[len(arr)-i-1]
	}
	return fmt.Sprintf("%.3f", ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
