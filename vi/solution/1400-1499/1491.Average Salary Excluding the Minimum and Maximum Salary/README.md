---
comments: true
difficulty: Easy
rating: 1201
source: Biweekly Contest 29 Q1
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [1491. Average Salary Excluding the Minimum and Maximum Salary](https://leetcode.com/problems/average-salary-excluding-the-minimum-and-maximum-salary)

[中文文档](/solution/1400-1499/1491.Average%20Salary%20Excluding%20the%20Minimum%20and%20Maximum%20Salary/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>không trùng lặp</strong> <code>salary</code>, trong đó <code>salary[i]</code> là mức lương của nhân viên thứ <code>i</code>.</p>

<p>Trả về <em>mức lương trung bình của các nhân viên sau khi loại bỏ mức lương nhỏ nhất và lớn nhất</em>. Các đáp án có sai số không quá <code>10<sup>-5</sup></code> so với đáp án thực tế đều được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> salary = [4000,3000,1000,2000]
<strong>Đầu ra:</strong> 2500.00000
<strong>Giải thích:</strong> Mức lương nhỏ nhất và lớn nhất lần lượt là 1000 và 4000.
Mức lương trung bình sau khi loại bỏ mức lương nhỏ nhất và lớn nhất là (2000+3000) / 2 = 2500
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> salary = [1000,2000,3000]
<strong>Đầu ra:</strong> 2000.00000
<strong>Giải thích:</strong> Mức lương nhỏ nhất và lớn nhất lần lượt là 1000 và 3000.
Mức lương trung bình sau khi loại bỏ mức lương nhỏ nhất và lớn nhất là (2000) / 1 = 2000
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= salary.length &lt;= 100</code></li>
	<li><code>1000 &lt;= salary[i] &lt;= 10<sup>6</sup></code></li>
	<li>Tất cả các số nguyên trong <code>salary</code> đều <strong>không trùng lặp</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 100$ và các mức lương không trùng lặp. Trừ giá trị nhỏ nhất và lớn nhất khỏi tổng, sau đó chia cho $n-2$.

<!-- thinking:end -->

Mô phỏng theo yêu cầu của đề bài.

Duyệt qua mảng, tìm giá trị lớn nhất và nhỏ nhất, đồng thời tính tổng. Sau đó tính giá trị trung bình sau khi loại bỏ giá trị lớn nhất và nhỏ nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng `salary`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def average(self, salary: List[int]) -> float:
        s = sum(salary) - min(salary) - max(salary)
        return s / (len(salary) - 2)
```

#### Java

```java
class Solution {
    public double average(int[] salary) {
        int s = 0;
        int mi = 10000000, mx = 0;
        for (int v : salary) {
            mi = Math.min(mi, v);
            mx = Math.max(mx, v);
            s += v;
        }
        s -= (mi + mx);
        return s * 1.0 / (salary.length - 2);
    }
}
```

#### C++

```cpp
class Solution {
public:
    double average(vector<int>& salary) {
        int s = 0;
        int mi = 1e7, mx = 0;
        for (int v : salary) {
            s += v;
            mi = min(mi, v);
            mx = max(mx, v);
        }
        s -= (mi + mx);
        return (double) s / (salary.size() - 2);
    }
};
```

#### Go

```go
func average(salary []int) float64 {
	s := 0
	mi, mx := 10000000, 0
	for _, v := range salary {
		s += v
		mi = min(mi, v)
		mx = max(mx, v)
	}
	s -= (mi + mx)
	return float64(s) / float64(len(salary)-2)
}
```

#### TypeScript

```ts
function average(salary: number[]): number {
    let max = -Infinity;
    let min = Infinity;
    let sum = 0;
    for (const v of salary) {
        sum += v;
        max = Math.max(max, v);
        min = Math.min(min, v);
    }
    return (sum - max - min) / (salary.length - 2);
}
```

#### Rust

```rust
impl Solution {
    pub fn average(salary: Vec<i32>) -> f64 {
        let n = salary.len() as i32;
        let mut min = i32::MAX;
        let mut max = i32::MIN;
        let mut sum = 0;
        for &num in salary.iter() {
            min = min.min(num);
            max = max.max(num);
            sum += num;
        }
        f64::from(sum - min - max) / f64::from(n - 2)
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $salary
     * @return Float
     */
    function average($salary) {
        $max = $sum = 0;
        $min = 10 ** 6;
        for ($i = 0; $i < count($salary); $i++) {
            $min = min($min, $salary[$i]);
            $max = max($max, $salary[$i]);
            $sum += $salary[$i];
        }
        return ($sum - $max - $min) / (count($salary) - 2);
    }
}
```

#### C

```c
#define max(a, b) (((a) > (b)) ? (a) : (b))
#define min(a, b) (((a) < (b)) ? (a) : (b))

double average(int* salary, int salarySize) {
    int ma = INT_MIN;
    int mi = INT_MAX;
    int sum = 0;
    for (int i = 0; i < salarySize; i++) {
        sum += salary[i];
        ma = max(ma, salary[i]);
        mi = min(mi, salary[i]);
    }
    return (sum - mi - ma) * 1.0 / (salarySize - 2);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
