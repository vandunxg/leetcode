---
comments: true
difficulty: Easy
rating: 1244
source: Biweekly Contest 11 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [1228. Missing Number In Arithmetic Progression 🔒](https://leetcode.com/problems/missing-number-in-arithmetic-progression)

[中文文档](/solution/1200-1299/1228.Missing%20Number%20In%20Arithmetic%20Progression/README.md)

## Mô tả

<!-- description:start -->

<p>Trong mảng <code>arr</code>, các giá trị tạo thành một cấp số cộng: hiệu <code>arr[i + 1] - arr[i]</code> bằng nhau với mọi <code>0 &lt;= i &lt; arr.length - 1</code>.</p>

<p>Một giá trị trong <code>arr</code> đã bị xóa; giá trị đó <strong>không phải phần tử đầu tiên hay cuối cùng của mảng</strong>.</p>

<p>Cho <code>arr</code>, hãy trả về <em>giá trị đã bị xóa</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [5,7,11,13]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Mảng ban đầu là [5,7,<strong>9</strong>,11,13].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [15,13,12]
<strong>Đầu ra:</strong> 14
<strong>Giải thích:</strong> Mảng ban đầu là [15,<strong>14</strong>,13,12].</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= arr.length &lt;= 1000</code></li>
	<li><code>0 &lt;= arr[i] &lt;= 10<sup>5</sup></code></li>
	<li>Mảng đầu vào được <strong>đảm bảo</strong> là mảng hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Công thức tính tổng cấp số cộng

<!-- thinking:start -->

> **Tư duy**
>
> Mảng là một cấp số cộng bị thiếu một số hạng, với $n \le 1000$. Dãy ban đầu có $n+1$ số hạng và biết hai đầu mút, nên có thể tính tổng bằng công thức; lấy tổng đó trừ tổng mảng hiện tại sẽ được số hạng còn thiếu. Chỉ cần tính tổng một lần, không cần tìm công sai.

<!-- thinking:end -->

Công thức tính tổng cấp số cộng là $\frac{(a_1 + a_n)n}{2}$, trong đó $n$ là số số hạng, $a_1$ là số hạng đầu và $a_n$ là số hạng cuối.

Vì mảng đã cho là một cấp số cộng bị thiếu một số, số số hạng của mảng là $n + 1$, số hạng đầu là $a_1$ và số hạng cuối là $a_n$. Do đó, tổng của mảng là $\frac{(a_1 + a_n)(n + 1)}{2}$.

Vậy số còn thiếu là $\frac{(a_1 + a_n)(n + 1)}{2} - \sum_{i = 0}^n a_i$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def missingNumber(self, arr: List[int]) -> int:
        return (arr[0] + arr[-1]) * (len(arr) + 1) // 2 - sum(arr)
```

#### Java

```java
class Solution {
    public int missingNumber(int[] arr) {
        int n = arr.length;
        int x = (arr[0] + arr[n - 1]) * (n + 1) / 2;
        int y = Arrays.stream(arr).sum();
        return x - y;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int missingNumber(vector<int>& arr) {
        int n = arr.size();
        int x = (arr[0] + arr[n - 1]) * (n + 1) / 2;
        int y = accumulate(arr.begin(), arr.end(), 0);
        return x - y;
    }
};
```

#### Go

```go
func missingNumber(arr []int) int {
	n := len(arr)
	x := (arr[0] + arr[n-1]) * (n + 1) / 2
	y := 0
	for _, v := range arr {
		y += v
	}
	return x - y
}
```

#### TypeScript

```ts
function missingNumber(arr: number[]): number {
    const x = ((arr[0] + arr.at(-1)!) * (arr.length + 1)) >> 1;
    const y = arr.reduce((acc, cur) => acc + cur, 0);
    return x - y;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tìm công sai + Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Công thức tính tổng tìm được giá trị còn thiếu mà không cần công sai. Tính $d$ từ hai đầu mút và độ dài, rồi tìm khoảng cách nào khác $d$ để xác định số hạng thiếu tại đó; nếu mọi khoảng cách đều bằng $d$, tất cả giá trị bằng nhau. Độ phức tạp vẫn tuyến tính và cách này gần với định nghĩa cấp số cộng hơn.

<!-- thinking:end -->

Vì mảng đã cho là một cấp số cộng bị thiếu một số, số hạng đầu là $a_1$ và số hạng cuối là $a_n$. Công sai $d$ bằng $\frac{a_n - a_1}{n}$.

Duyệt mảng; nếu $a_i \neq a_{i - 1} + d$, trả về $a_{i - 1} + d$.

Nếu duyệt hết mà không tìm thấy số còn thiếu, nghĩa là mọi số trong mảng đều bằng nhau. Khi đó, trả về ngay phần tử đầu tiên của mảng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def missingNumber(self, arr: List[int]) -> int:
        n = len(arr)
        d = (arr[-1] - arr[0]) // n
        for i in range(1, n):
            if arr[i] != arr[i - 1] + d:
                return arr[i - 1] + d
        return arr[0]
```

#### Java

```java
class Solution {
    public int missingNumber(int[] arr) {
        int n = arr.length;
        int d = (arr[n - 1] - arr[0]) / n;
        for (int i = 1; i < n; ++i) {
            if (arr[i] != arr[i - 1] + d) {
                return arr[i - 1] + d;
            }
        }
        return arr[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int missingNumber(vector<int>& arr) {
        int n = arr.size();
        int d = (arr[n - 1] - arr[0]) / n;
        for (int i = 1; i < n; ++i) {
            if (arr[i] != arr[i - 1] + d) {
                return arr[i - 1] + d;
            }
        }
        return arr[0];
    }
};
```

#### Go

```go
func missingNumber(arr []int) int {
	n := len(arr)
	d := (arr[n-1] - arr[0]) / n
	for i := 1; i < n; i++ {
		if arr[i] != arr[i-1]+d {
			return arr[i-1] + d
		}
	}
	return arr[0]
}
```

#### TypeScript

```ts
function missingNumber(arr: number[]): number {
    const d = ((arr.at(-1)! - arr[0]) / arr.length) | 0;
    for (let i = 1; i < arr.length; ++i) {
        if (arr[i] - arr[i - 1] !== d) {
            return arr[i - 1] + d;
        }
    }
    return arr[0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
