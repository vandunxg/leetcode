---
comments: true
difficulty: Easy
rating: 1153
source: Weekly Contest 319 Q1
tags:
    - Math
---

<!-- problem:start -->

# [2469. Convert the Temperature](https://leetcode.com/problems/convert-the-temperature)

[中文文档](/solution/2400-2499/2469.Convert%20the%20Temperature/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số thực không âm được làm tròn đến hai chữ số thập phân <code>celsius</code>, biểu thị <strong>nhiệt độ theo thang Celsius</strong>.</p>

<p>Hãy chuyển đổi Celsius sang <strong>Kelvin</strong> và <strong>Fahrenheit</strong>, rồi trả về dưới dạng mảng <code>ans = [kelvin, fahrenheit]</code>.</p>

<p>Trả về <em>mảng <code>ans</code>. </em>Các đáp án có sai số không quá <code>10<sup>-5</sup></code> so với đáp án thực tế đều được chấp nhận.</p>

<p><strong>Lưu ý rằng:</strong></p>

<ul>
	<li><code>Kelvin = Celsius + 273.15</code></li>
	<li><code>Fahrenheit = Celsius * 1.80 + 32.00</code></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> celsius = 36.50
<strong>Đầu ra:</strong> [309.65000,97.70000]
<strong>Giải thích:</strong> Nhiệt độ 36.50 Celsius khi chuyển sang Kelvin là 309.65 và khi chuyển sang Fahrenheit là 97.70.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> celsius = 122.11
<strong>Đầu ra:</strong> [395.26000,251.79800]
<strong>Giải thích:</strong> Nhiệt độ 122.11 Celsius khi chuyển sang Kelvin là 395.26 và khi chuyển sang Fahrenheit là 251.798.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= celsius &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Kelvin và Fahrenheit đều được tính bằng công thức bậc nhất theo Celsius; chỉ cần áp dụng hai công thức một lần.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp theo mô tả của đề bài.

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def convertTemperature(self, celsius: float) -> List[float]:
        return [celsius + 273.15, celsius * 1.8 + 32]
```

#### Java

```java
class Solution {
    public double[] convertTemperature(double celsius) {
        return new double[] {celsius + 273.15, celsius * 1.8 + 32};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<double> convertTemperature(double celsius) {
        return {celsius + 273.15, celsius * 1.8 + 32};
    }
};
```

#### Go

```go
func convertTemperature(celsius float64) []float64 {
	return []float64{celsius + 273.15, celsius*1.8 + 32}
}
```

#### TypeScript

```ts
function convertTemperature(celsius: number): number[] {
    return [celsius + 273.15, celsius * 1.8 + 32];
}
```

#### Rust

```rust
impl Solution {
    pub fn convert_temperature(celsius: f64) -> Vec<f64> {
        vec![celsius + 273.15, celsius * 1.8 + 32.0]
    }
}
```

#### C

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
double* convertTemperature(double celsius, int* returnSize) {
    double* ans = malloc(sizeof(double) * 2);
    ans[0] = celsius + 273.15;
    ans[1] = celsius * 1.8 + 32;
    *returnSize = 2;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
