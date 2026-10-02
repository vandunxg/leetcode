---
comments: true
difficulty: Easy
rating: 1262
source: Weekly Contest 141 Q1
tags:
    - Array
    - Two Pointers
---

<!-- problem:start -->

# [1089. Duplicate Zeros](https://leetcode.com/problems/duplicate-zeros)

[中文文档](/solution/1000-1099/1089.Duplicate%20Zeros/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code> có độ dài cố định, hãy nhân đôi mỗi số 0 và dịch các phần tử còn lại sang phải.</p>

<p><strong>Lưu ý</strong>, không ghi các phần tử vượt quá độ dài ban đầu của mảng. Hãy chỉnh sửa trực tiếp mảng đầu vào và không trả về giá trị nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,0,2,3,0,4,5,0]
<strong>Đầu ra:</strong> [1,0,0,2,3,0,0,4]
<strong>Giải thích:</strong> Sau khi gọi hàm, mảng đầu vào được thay đổi thành: [1,0,0,2,3,0,0,4]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,3]
<strong>Đầu ra:</strong> [1,2,3]
<strong>Giải thích:</strong> Sau khi gọi hàm, mảng đầu vào được thay đổi thành: [1,2,3]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= arr[i] &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two pointers

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi số 0 cần được nhân đôi ngay trong mảng, còn phần đuôi vượt quá độ dài mảng sẽ bị bỏ. Dùng mảng phụ thì không còn là thao tác tại chỗ; ghi từ trái sang sẽ ghi đè lên các giá trị chưa đọc. Vì vậy, trước tiên ta tìm chỉ số cuối cùng của mảng gốc còn vừa với mảng sau khi nhân đôi, rồi ghi từ phải sang trái.
>
> $i$ và độ dài ảo $k$ cùng tăng: thêm $1$ với giá trị khác 0, thêm $2$ với số 0, cho đến khi $k\ge n$. Nếu số 0 cuối cùng làm $k=n+1$, ta chỉ ghi số 0 đó một lần ở cuối mảng.
>
> Sau đó, $j$ duyệt từ $n-1$ về đầu mảng: số 0 chiếm hai vị trí, còn số khác 0 chiếm một vị trí.

<!-- thinking:end -->

Duyệt từ trái sang phải để xác định phần nào của mảng gốc còn nằm trong giới hạn sau khi nhân đôi các số 0. Con trỏ $i$ là chỉ số cuối cùng của mảng gốc còn vừa, còn $k$ là độ dài ảo sau khi nhân đôi: tăng $1$ với giá trị khác 0 và tăng $2$ với số 0. Dừng khi $k \ge n$.

Đặt $j = n - 1$ làm chỉ số ghi. Nếu giá trị cuối cùng được giữ lại là số 0 khiến độ dài vượt giới hạn ($k = n + 1$), ghi số 0 đó một lần vào $arr[j]$ rồi giảm cả $i$ và $j$.

Tiếp theo, điền mảng từ phải sang trái. Sao chép $arr[i]$ một lần vào $arr[j]$ nếu giá trị khác 0, hoặc hai lần nếu đó là số 0.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def duplicateZeros(self, arr: List[int]) -> None:
        """
        Do not return anything, modify arr in-place instead.
        """
        n = len(arr)
        i, k = -1, 0
        while k < n:
            i += 1
            k += 1 if arr[i] else 2
        j = n - 1
        if k == n + 1:
            arr[j] = 0
            i, j = i - 1, j - 1
        while ~j:
            if arr[i] == 0:
                arr[j] = arr[j - 1] = arr[i]
                j -= 1
            else:
                arr[j] = arr[i]
            i, j = i - 1, j - 1
```

#### Java

```java
class Solution {
    public void duplicateZeros(int[] arr) {
        int n = arr.length;
        int i = -1, k = 0;
        while (k < n) {
            ++i;
            k += arr[i] > 0 ? 1 : 2;
        }
        int j = n - 1;
        if (k == n + 1) {
            arr[j--] = 0;
            --i;
        }
        while (j >= 0) {
            arr[j] = arr[i];
            if (arr[i] == 0) {
                arr[--j] = arr[i];
            }
            --i;
            --j;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    void duplicateZeros(vector<int>& arr) {
        int n = arr.size();
        int i = -1, k = 0;
        while (k < n) {
            ++i;
            k += arr[i] ? 1 : 2;
        }
        int j = n - 1;
        if (k == n + 1) {
            arr[j--] = 0;
            --i;
        }
        while (~j) {
            arr[j] = arr[i];
            if (arr[i] == 0) arr[--j] = arr[i];
            --i;
            --j;
        }
    }
};
```

#### Go

```go
func duplicateZeros(arr []int) {
	n := len(arr)
	i, k := -1, 0
	for k < n {
		i, k = i+1, k+1
		if arr[i] == 0 {
			k++
		}
	}
	j := n - 1
	if k == n+1 {
		arr[j] = 0
		i, j = i-1, j-1
	}
	for j >= 0 {
		arr[j] = arr[i]
		if arr[i] == 0 {
			j--
			arr[j] = arr[i]
		}
		i, j = i-1, j-1
	}
}
```

#### Rust

```rust
impl Solution {
    pub fn duplicate_zeros(arr: &mut Vec<i32>) {
        let n = arr.len();
        let mut i = 0;
        let mut j = 0;
        while j < n {
            if arr[i] == 0 {
                j += 1;
            }
            j += 1;
            i += 1;
        }
        while i > 0 {
            if arr[i - 1] == 0 {
                if j <= n {
                    arr[j - 1] = arr[i - 1];
                }
                j -= 1;
            }
            arr[j - 1] = arr[i - 1];
            i -= 1;
            j -= 1;
        }
    }
}
```

#### C

```c
void duplicateZeros(int* arr, int arrSize) {
    int i = 0;
    int j = 0;
    while (j < arrSize) {
        if (arr[i] == 0) {
            j++;
        }
        i++;
        j++;
    }
    i--;
    j--;
    while (i >= 0) {
        if (arr[i] == 0) {
            if (j < arrSize) {
                arr[j] = arr[i];
            }
            j--;
        }
        arr[j] = arr[i];
        i--;
        j--;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
