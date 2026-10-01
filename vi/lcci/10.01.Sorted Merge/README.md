---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [10.01. Sorted Merge](https://leetcode.cn/problems/sorted-merge-lcci)

[中文文档](/lcci/10.01.Sorted%20Merge/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng đã được sắp xếp, A và B, trong đó A có đủ vùng đệm ở cuối để chứa B. Hãy viết một method để gộp B vào A theo thứ tự đã sắp xếp.</p>

<p>Ban đầu số phần tử trong A và B lần lượt là&nbsp;<em>m</em>&nbsp;và&nbsp;<em>n</em>.</p>

<p><strong>Ví dụ:</strong></p>

<pre>

<strong>Đầu vào:</strong>

A = [1,2,3,0,0,0], m = 3

B = [2,5,6],       n = 3



<strong>Đầu ra:</strong>&nbsp;[1,2,2,3,5,6]</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> $B$ phải được gộp vào phần đuôi còn trống của $A$. Nếu ghi từ đầu, chúng ta sẽ ghi đè các giá trị của $A$ chưa đọc. Dùng buffer thứ ba sẽ cần thêm không gian tuyến tính.
>
> Phần đuôi chưa sử dụng của $A$ có thể chứa toàn bộ kết quả, vì vậy đặt phần tử lớn hơn ở trước sẽ giữ nguyên các phần đầu chưa đọc.
>
> Hai con trỏ $i$ và $j$ nằm ở các phần tử cuối còn hiệu lực; $k$ đi lùi từ $m+n-1$. Ghi $A[i]$ khi $B$ đã hết hoặc $A[i]$ lớn hơn, nếu không thì ghi $B[j]$.

<!-- thinking:end -->

Chúng ta dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến cuối các mảng $A$ và $B$, cùng một con trỏ $k$ trỏ đến cuối mảng $A$. Sau đó, chúng ta duyệt các mảng $A$ và $B$ từ cuối về đầu; mỗi lần đặt phần tử lớn hơn vào $A[k]$, rồi giảm con trỏ $k$ và con trỏ của mảng chứa phần tử lớn hơn đi một vị trí.

Độ phức tạp thời gian là $O(m + n)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def merge(self, A: List[int], m: int, B: List[int], n: int) -> None:
        i, j = m - 1, n - 1
        for k in reversed(range(m + n)):
            if j < 0 or i >= 0 and A[i] > B[j]:
                A[k] = A[i]
                i -= 1
            else:
                A[k] = B[j]
                j -= 1
```

#### Java

```java
class Solution {
    public void merge(int[] A, int m, int[] B, int n) {
        int i = m - 1, j = n - 1;
        for (int k = A.length - 1; k >= 0; --k) {
            if (j < 0 || (i >= 0 && A[i] > B[j])) {
                A[k] = A[i--];
            } else {
                A[k] = B[j--];
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    void merge(vector<int>& A, int m, vector<int>& B, int n) {
        int i = m - 1, j = n - 1;
        for (int k = A.size() - 1; ~k; --k) {
            if (j < 0 || (i >= 0 && A[i] > B[j])) {
                A[k] = A[i--];
            } else {
                A[k] = B[j--];
            }
        }
    }
};
```

#### Go

```go
func merge(A []int, m int, B []int, n int) {
	i, j := m-1, n-1
	for k := len(A) - 1; k >= 0; k-- {
		if j < 0 || (i >= 0 && A[i] > B[j]) {
			A[k] = A[i]
			i--
		} else {
			A[k] = B[j]
			j--
		}
	}
}
```

#### TypeScript

```ts
/**
 Do not return anything, modify A in-place instead.
 */
function merge(A: number[], m: number, B: number[], n: number): void {
    let [i, j] = [m - 1, n - 1];
    for (let k = A.length - 1; ~k; --k) {
        if (j < 0 || (i >= 0 && A[i] > B[j])) {
            A[k] = A[i--];
        } else {
            A[k] = B[j--];
        }
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn merge(a: &mut Vec<i32>, m: i32, b: &mut Vec<i32>, n: i32) {
        let (mut i, mut j) = (m - 1, n - 1);
        for k in (0..m + n).rev() {
            if j < 0 || (i >= 0 && a[i as usize] > b[j as usize]) {
                a[k as usize] = a[i as usize];
                i -= 1;
            } else {
                a[k as usize] = b[j as usize];
                j -= 1;
            }
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} A
 * @param {number} m
 * @param {number[]} B
 * @param {number} n
 * @return {void} Do not return anything, modify A in-place instead.
 */
var merge = function (A, m, B, n) {
    let [i, j] = [m - 1, n - 1];
    for (let k = A.length - 1; ~k; --k) {
        if (j < 0 || (i >= 0 && A[i] > B[j])) {
            A[k] = A[i--];
        } else {
            A[k] = B[j--];
        }
    }
};
```

#### Swift

```swift
class Solution {
    func merge(_ A: inout [Int], _ m: Int, _ B: [Int], _ n: Int) {
        var i = m - 1, j = n - 1
        for k in stride(from: m + n - 1, through: 0, by: -1) {
            if j < 0 || (i >= 0 && A[i] > B[j]) {
                A[k] = A[i]
                i -= 1
            } else {
                A[k] = B[j]
                j -= 1
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
