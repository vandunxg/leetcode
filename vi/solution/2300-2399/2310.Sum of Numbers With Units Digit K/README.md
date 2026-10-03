---
comments: true
difficulty: Medium
rating: 1558
source: Weekly Contest 298 Q2
tags:
    - Greedy
    - Math
    - Dynamic Programming
    - Enumeration
---

<!-- problem:start -->

# [2310. Sum of Numbers With Units Digit K](https://leetcode.com/problems/sum-of-numbers-with-units-digit-k)

[中文文档](/solution/2300-2399/2310.Sum%20of%20Numbers%20With%20Units%20Digit%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>num</code> và <code>k</code>, xét một tập hợp các số nguyên dương có các tính chất sau:</p>

<ul>
	<li>Chữ số hàng đơn vị của mỗi số nguyên là <code>k</code>.</li>
	<li>Tổng các số nguyên bằng <code>num</code>.</li>
</ul>

<p>Trả về <em><strong>số phần tử nhỏ nhất</strong> có thể có của tập hợp đó, hoặc </em><code>-1</code><em> nếu không tồn tại tập hợp như vậy.</em></p>

<p>Lưu ý:</p>

<ul>
	<li>Tập hợp có thể chứa nhiều phần tử giống nhau, và tổng của tập hợp rỗng được xem là <code>0</code>.</li>
	<li><strong>Chữ số hàng đơn vị</strong> của một số là chữ số ngoài cùng bên phải của số đó.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 58, k = 9
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Một tập hợp hợp lệ là [9,49], vì tổng bằng 58 và mỗi số đều có chữ số hàng đơn vị là 9.
Một tập hợp khác là [19,39].
Có thể chứng minh rằng 2 là số phần tử nhỏ nhất có thể có của một tập hợp hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 37, k = 2
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể tạo ra tổng 37 chỉ bằng các số nguyên có chữ số hàng đơn vị là 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 0, k = 7
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Tổng của tập hợp rỗng được xem là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= num &lt;= 3000</code></li>
	<li><code>0 &lt;= k &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi số hạng có dạng $10x+k$, nên chữ số hàng đơn vị của tổng của $n$ số như vậy được xác định bởi $n\times k$. Vì $num \le 3000$, ta liệt kê $n$ và kiểm tra xem $num-n\times k$ có phải là bội không âm của $10$ hay không.
>
> Thử $n$ từ nhỏ đến lớn; lần đầu tiên tìm được giá trị thỏa mãn chính là giá trị nhỏ nhất. Nếu không có giá trị nào thỏa mãn khi thử đến $num$, thì không tồn tại lời giải.

<!-- thinking:end -->

Mỗi số thỏa mãn điều kiện tách có thể được biểu diễn dưới dạng $10x_i + k$. Nếu có $n$ số như vậy, thì $\textit{num} - n \times k$ phải là một bội của $10$.

Ta liệt kê $n$ từ nhỏ đến lớn và tìm giá trị $n$ đầu tiên thỏa mãn $\textit{num} - n \times k$ là một bội của $10$. Vì $n$ không thể lớn hơn $\textit{num}$, giá trị lớn nhất của $n$ là $\textit{num}$.

Ta cũng chỉ cần xét chữ số hàng đơn vị. Nếu chữ số hàng đơn vị thỏa mãn điều kiện, các chữ số ở hàng cao hơn có thể tùy ý.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là kích thước của $\textit{num}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumNumbers(self, num: int, k: int) -> int:
        if num == 0:
            return 0
        for i in range(1, num + 1):
            if (t := num - k * i) >= 0 and t % 10 == 0:
                return i
        return -1
```

#### Java

```java
class Solution {
    public int minimumNumbers(int num, int k) {
        if (num == 0) {
            return 0;
        }
        for (int i = 1; i <= num; ++i) {
            int t = num - k * i;
            if (t >= 0 && t % 10 == 0) {
                return i;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumNumbers(int num, int k) {
        if (num == 0) return 0;
        for (int i = 1; i <= num; ++i) {
            int t = num - k * i;
            if (t >= 0 && t % 10 == 0) return i;
        }
        return -1;
    }
};
```

#### Go

```go
func minimumNumbers(num int, k int) int {
	if num == 0 {
		return 0
	}
	for i := 1; i <= num; i++ {
		t := num - k*i
		if t >= 0 && t%10 == 0 {
			return i
		}
	}
	return -1
}
```

#### TypeScript

```ts
function minimumNumbers(num: number, k: number): number {
    if (!num) return 0;
    let digit = num % 10;
    for (let i = 1; i < 11; i++) {
        let target = i * k;
        if (target <= num && target % 10 == digit) return i;
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Toán học + Liệt kê (Chữ số hàng đơn vị)

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 có thể thử đến $num$ giá trị. Chữ số hàng đơn vị lặp lại sau mỗi $10$ lần, nên chỉ cần kiểm tra $n \le 10$ sao cho $n\times k$ đồng dư với $num$ và không lớn hơn $num$.

<!-- thinking:end -->

Liệt kê tối đa $10$ số có chữ số hàng đơn vị là $k$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumNumbers(self, num: int, k: int) -> int:
        if num == 0:
            return 0
        for i in range(1, 11):
            if (k * i) % 10 == num % 10 and k * i <= num:
                return i
        return -1
```

#### Java

```java
class Solution {
    public int minimumNumbers(int num, int k) {
        if (num == 0) {
            return 0;
        }
        for (int i = 1; i <= 10; ++i) {
            if ((k * i) % 10 == num % 10 && k * i <= num) {
                return i;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumNumbers(int num, int k) {
        if (!num) return 0;
        for (int i = 1; i <= 10; ++i)
            if ((k * i) % 10 == num % 10 && k * i <= num)
                return i;
        return -1;
    }
};
```

#### Go

```go
func minimumNumbers(num int, k int) int {
	if num == 0 {
		return 0
	}
	for i := 1; i <= 10; i++ {
		if (k*i)%10 == num%10 && k*i <= num {
			return i
		}
	}
	return -1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Tìm kiếm với Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Hai phương pháp đầu dựa trên tính chia hết nên ngắn gọn, nhưng sẽ khó mở rộng nếu xuất hiện thêm ràng buộc. Tìm kiếm với memoization trừ dần các số có chữ số hàng đơn vị là $k$; các bài toán con chỉ phụ thuộc vào phần còn lại. Phương pháp này đúng với bài toán hiện tại, nhưng nặng hơn việc liệt kê trực tiếp.

<!-- thinking:end -->

Dùng memoization để lưu số lượng ít nhất các số có chữ số hàng đơn vị là $k$ sao cho tổng của chúng bằng $\textit{num}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumNumbers(self, num: int, k: int) -> int:
        @cache
        def dfs(v):
            if v == 0:
                return 0
            if v < 10 and v % k:
                return inf
            i = 0
            t = inf
            while (x := i * 10 + k) <= v:
                t = min(t, dfs(v - x))
                i += 1
            return t + 1

        if num == 0:
            return 0
        if k == 0:
            return -1 if num % 10 else 1
        ans = dfs(num)
        return -1 if ans >= inf else ans
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
