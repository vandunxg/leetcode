---
comments: true
difficulty: Medium
rating: 1633
source: Weekly Contest 138 Q3
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [1053. Previous Permutation With One Swap](https://leetcode.com/problems/previous-permutation-with-one-swap)

[中文文档](/solution/1000-1099/1053.Previous%20Permutation%20With%20One%20Swap/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên dương <code>arr</code> (các phần tử có thể trùng nhau), hãy trả về <em>hoán vị lớn nhất theo </em><span data-keyword="lexicographically-smaller-array"><em>thứ tự từ điển</em></span><em> nhưng nhỏ hơn </em><code>arr</code>, có thể tạo ra bằng cách <strong>đổi chỗ đúng một lần</strong>. Nếu không thể, hãy trả về chính mảng ban đầu.</p>

<p><strong>Lưu ý</strong>, phép <em>đổi chỗ</em> hoán đổi vị trí của hai số <code>arr[i]</code> và <code>arr[j]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [3,2,1]
<strong>Đầu ra:</strong> [3,1,2]
<strong>Giải thích:</strong> Đổi chỗ 2 và 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,1,5]
<strong>Đầu ra:</strong> [1,1,5]
<strong>Giải thích:</strong> Mảng đã là hoán vị nhỏ nhất.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,9,4,6,7]
<strong>Đầu ra:</strong> [1,7,4,6,9]
<strong>Giải thích:</strong> Đổi chỗ 9 và 7.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= arr[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ đổi chỗ một lần, nên ta cần tạo ra hoán vị lớn nhất theo thứ tự từ điển nhưng vẫn nhỏ hơn mảng ban đầu. Điểm giảm đầu tiên khi duyệt từ phải sang trái, $arr[i-1]>arr[i]$, xác định phần tử cần được đổi thành một giá trị nhỏ hơn ở bên phải.
>
> Phần tử thay thế cần là giá trị lớn nhất ở bên phải nhưng nhỏ hơn $arr[i-1]$. Nếu có nhiều bản sao, chọn bản nằm trái nhất; vì vậy khi duyệt từ phải sang trái, ta bỏ qua giá trị bằng phần tử đứng trước nó.
>
> Đổi chỗ rồi trả về mảng; nếu không có điểm giảm thì mảng đã là hoán vị nhỏ nhất.

<!-- thinking:end -->

Đầu tiên, duyệt mảng từ phải sang trái để tìm chỉ số đầu tiên $i$ thỏa mãn $arr[i - 1] > arr[i]$; khi đó, $arr[i - 1]$ là phần tử cần đổi chỗ. Tiếp theo, lại duyệt từ phải sang trái để tìm chỉ số đầu tiên $j$ thỏa mãn $arr[j] < arr[i - 1]$ và $arr[j] \neq arr[j - 1]$. Sau đó, đổi chỗ $arr[i - 1]$ với $arr[j]$ rồi trả về mảng.

Nếu duyệt hết mảng mà không tìm được chỉ số $i$ thỏa mãn điều kiện, nghĩa là mảng đã là hoán vị nhỏ nhất, nên ta chỉ cần trả về mảng ban đầu.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def prevPermOpt1(self, arr: List[int]) -> List[int]:
        n = len(arr)
        for i in range(n - 1, 0, -1):
            if arr[i - 1] > arr[i]:
                for j in range(n - 1, i - 1, -1):
                    if arr[j] < arr[i - 1] and arr[j] != arr[j - 1]:
                        arr[i - 1], arr[j] = arr[j], arr[i - 1]
                        return arr
        return arr
```

#### Java

```java
class Solution {
    public int[] prevPermOpt1(int[] arr) {
        int n = arr.length;
        for (int i = n - 1; i > 0; --i) {
            if (arr[i - 1] > arr[i]) {
                for (int j = n - 1; j > i - 1; --j) {
                    if (arr[j] < arr[i - 1] && arr[j] != arr[j - 1]) {
                        int t = arr[i - 1];
                        arr[i - 1] = arr[j];
                        arr[j] = t;
                        return arr;
                    }
                }
            }
        }
        return arr;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> prevPermOpt1(vector<int>& arr) {
        int n = arr.size();
        for (int i = n - 1; i > 0; --i) {
            if (arr[i - 1] > arr[i]) {
                for (int j = n - 1; j > i - 1; --j) {
                    if (arr[j] < arr[i - 1] && arr[j] != arr[j - 1]) {
                        swap(arr[i - 1], arr[j]);
                        return arr;
                    }
                }
            }
        }
        return arr;
    }
};
```

#### Go

```go
func prevPermOpt1(arr []int) []int {
	n := len(arr)
	for i := n - 1; i > 0; i-- {
		if arr[i-1] > arr[i] {
			for j := n - 1; j > i-1; j-- {
				if arr[j] < arr[i-1] && arr[j] != arr[j-1] {
					arr[i-1], arr[j] = arr[j], arr[i-1]
					return arr
				}
			}
		}
	}
	return arr
}
```

#### TypeScript

```ts
function prevPermOpt1(arr: number[]): number[] {
    const n = arr.length;
    for (let i = n - 1; i > 0; --i) {
        if (arr[i - 1] > arr[i]) {
            for (let j = n - 1; j > i - 1; --j) {
                if (arr[j] < arr[i - 1] && arr[j] !== arr[j - 1]) {
                    const t = arr[i - 1];
                    arr[i - 1] = arr[j];
                    arr[j] = t;
                    return arr;
                }
            }
        }
    }
    return arr;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
