---
comments: true
difficulty: Easy
rating: 1374
source: Biweekly Contest 37 Q1
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [1619. Mean of Array After Removing Some Elements](https://leetcode.com/problems/mean-of-array-after-removing-some-elements)

[中文文档](/solution/1600-1699/1619.Mean%20of%20Array%20After%20Removing%20Some%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code>, hãy trả về <em>giá trị trung bình của các số còn lại sau khi loại bỏ <code>5%</code> phần tử nhỏ nhất và <code>5%</code> phần tử lớn nhất.</em></p>

<p>Câu trả lời sai lệch không quá <code>10<sup>-5</sup></code> so với <strong>đáp án thực</strong> được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,3]
<strong>Output:</strong> 2.00000
<strong>Giải thích:</strong> Sau khi xóa giá trị nhỏ nhất và lớn nhất của mảng, mọi phần tử đều bằng 2 nên giá trị trung bình là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [6,2,7,5,1,2,0,3,10,2,5,0,5,5,0,8,7,6,8,0]
<strong>Output:</strong> 4.00000
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> arr = [6,0,7,0,7,5,7,8,3,4,0,7,8,1,6,8,1,1,2,4,8,1,9,5,4,3,8,5,10,8,6,6,1,0,6,10,8,2,3,4]
<strong>Output:</strong> 4.77778
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>20 &lt;= arr.length &lt;= 1000</code></li>
	<li><code>arr.length</code><b> </b><strong>is a multiple</strong> of <code>20</code>.</li>
	<li><code><font face="monospace">0 &lt;= arr[i] &lt;= 10<sup>5</sup></font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta loại bỏ $5\%$ nhỏ nhất và 5% lớn nhất rồi lấy trung bình phần còn lại. Độ dài là bội của $20$ và đủ nhỏ để chỉ cần sắp xếp rồi cắt mảng.
>
> Sau khi sắp xếp, bỏ $0.05n$ phần tử ở mỗi đầu, tính trung bình phần giữa và làm tròn đến năm chữ số thập phân.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def trimMean(self, arr: List[int]) -> float:
        n = len(arr)
        start, end = int(n * 0.05), int(n * 0.95)
        arr.sort()
        t = arr[start:end]
        return round(sum(t) / len(t), 5)
```

#### Java

```java
class Solution {
    public double trimMean(int[] arr) {
        Arrays.sort(arr);
        int n = arr.length;
        double s = 0;
        for (int start = (int) (n * 0.05), i = start; i < n - start; ++i) {
            s += arr[i];
        }
        return s / (n * 0.9);
    }
}
```

#### C++

```cpp
class Solution {
public:
    double trimMean(vector<int>& arr) {
        sort(arr.begin(), arr.end());
        int n = arr.size();
        double s = 0;
        for (int start = (int) (n * 0.05), i = start; i < n - start; ++i)
            s += arr[i];
        return s / (n * 0.9);
    }
};
```

#### Go

```go
func trimMean(arr []int) float64 {
	sort.Ints(arr)
	n := len(arr)
	sum := 0.0
	for i := n / 20; i < n-n/20; i++ {
		sum += float64(arr[i])
	}
	return sum / (float64(n) * 0.9)
}
```

#### TypeScript

```ts
function trimMean(arr: number[]): number {
    arr.sort((a, b) => a - b);
    let n = arr.length,
        rmLen = n * 0.05;
    let sum = 0;
    for (let i = rmLen; i < n - rmLen; i++) {
        sum += arr[i];
    }
    return sum / (n * 0.9);
}
```

#### Rust

```rust
impl Solution {
    pub fn trim_mean(mut arr: Vec<i32>) -> f64 {
        arr.sort();
        let n = arr.len();
        let count = ((n as f64) * 0.05).floor() as usize;
        let mut sum = 0;
        for i in count..n - count {
            sum += arr[i];
        }
        (sum as f64) / ((n as f64) * 0.9)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
