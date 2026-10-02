---
comments: true
difficulty: Easy
rating: 1167
source: Weekly Contest 169 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [1304. Find N Unique Integers Sum up to Zero](https://leetcode.com/problems/find-n-unique-integers-sum-up-to-zero)

[中文文档](/solution/1300-1399/1304.Find%20N%20Unique%20Integers%20Sum%20up%20to%20Zero/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy trả về <strong>bất kỳ</strong> mảng nào gồm <code>n</code> số nguyên <strong>khác nhau</strong> có tổng bằng <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> [-7,-1,1,3,4]
<strong>Giải thích:</strong> Các mảng sau cũng được chấp nhận: [-5,-1,1,2,3], [-3,-1,2,-2,4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> [-1,0,1]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> [0]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Xây dựng

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần $n$ số nguyên khác nhau có tổng bằng $0$. Chọn số bất kỳ rồi điều chỉnh có thể khiến các số bị trùng. Một cặp số đối nhau có tổng bằng $0$, nên ta thêm lần lượt $1,-1,\ldots,k,-k$. Nếu $n$ lẻ, ta thêm $0$ vào cuối; tổng vẫn bằng 0 và các số vẫn khác nhau.

<!-- thinking:end -->

Ta bắt đầu từ $1$ rồi lần lượt thêm số dương và số âm vào mảng kết quả. Lặp lại thao tác này $\frac{n}{2}$ lần. Nếu $n$ lẻ, ta thêm $0$ vào cuối mảng kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số nguyên được cho. Không tính phần bộ nhớ dùng cho đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumZero(self, n: int) -> List[int]:
        ans = []
        for i in range(n >> 1):
            ans.append(i + 1)
            ans.append(-(i + 1))
        if n & 1:
            ans.append(0)
        return ans
```

#### Java

```java
class Solution {
    public int[] sumZero(int n) {
        int[] ans = new int[n];
        for (int i = 1, j = 0; i <= n / 2; ++i) {
            ans[j++] = i;
            ans[j++] = -i;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> sumZero(int n) {
        vector<int> ans(n);
        for (int i = 1, j = 0; i <= n / 2; ++i) {
            ans[j++] = i;
            ans[j++] = -i;
        }
        return ans;
    }
};
```

#### Go

```go
func sumZero(n int) []int {
	ans := make([]int, n)
	for i, j := 1, 0; i <= n/2; i, j = i+1, j+1 {
		ans[j] = i
		j++
		ans[j] = -i
	}
	return ans
}
```

#### TypeScript

```ts
function sumZero(n: number): number[] {
    const ans: number[] = Array(n).fill(0);
    for (let i = 1, j = 0; i <= n / 2; ++i) {
        ans[j++] = i;
        ans[j++] = -i;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_zero(n: i32) -> Vec<i32> {
        let mut ans = vec![0; n as usize];
        let mut j = 0;
        for i in 1..=n / 2 {
            ans[j] = i;
            j += 1;
            ans[j] = -i;
            j += 1;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Xây dựng + Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Cách ghép cần xử lý riêng trường hợp $n$ chẵn và lẻ. Thêm các số từ $1$ đến $n-1$, rồi thêm số đối của tổng các số đó sẽ đảm bảo tổng bằng $0$; giá trị cuối này không thể trùng với số nguyên dương nào đã thêm. Cách xây dựng này ngắn gọn hơn mà vẫn đảm bảo các phần tử khác nhau.

<!-- thinking:end -->

Ta cũng có thể thêm tất cả số nguyên từ $1$ đến $n-1$ vào mảng kết quả, rồi thêm số đối của tổng $n-1$ số đầu tiên, tức là $-\frac{n(n-1)}{2}$, vào mảng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số nguyên được cho. Không tính phần bộ nhớ dùng cho đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumZero(self, n: int) -> List[int]:
        ans = list(range(1, n))
        ans.append(-sum(ans))
        return ans
```

#### Java

```java
class Solution {
    public int[] sumZero(int n) {
        int[] ans = new int[n];
        for (int i = 1; i < n; ++i) {
            ans[i] = i;
        }
        ans[0] = -(n * (n - 1) / 2);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> sumZero(int n) {
        vector<int> ans(n);
        iota(ans.begin(), ans.end(), 1);
        ans[n - 1] = -(n - 1) * n / 2;
        return ans;
    }
};
```

#### Go

```go
func sumZero(n int) []int {
	ans := make([]int, n)
	for i := 1; i < n; i++ {
		ans[i] = i
	}
	ans[0] = -n * (n - 1) / 2
	return ans
}
```

#### TypeScript

```ts
function sumZero(n: number): number[] {
    const ans = new Array(n).fill(0);
    for (let i = 1; i < n; ++i) {
        ans[i] = i;
    }
    ans[0] = -((n * (n - 1)) / 2);
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_zero(n: i32) -> Vec<i32> {
        let mut ans = vec![0; n as usize];
        for i in 1..n {
            ans[i as usize] = i;
        }
        ans[0] = -(n * (n - 1) / 2);
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
