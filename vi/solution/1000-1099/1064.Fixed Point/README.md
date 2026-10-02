---
comments: true
difficulty: Easy
rating: 1307
source: Biweekly Contest 1 Q1
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [1064. Fixed Point 🔒](https://leetcode.com/problems/fixed-point)

[中文文档](/solution/1000-1099/1064.Fixed%20Point/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên phân biệt <code>arr</code> được sắp xếp theo thứ tự <strong>tăng dần</strong>. Hãy trả về chỉ số nhỏ nhất <code>i</code> thỏa mãn <code>arr[i] == i</code>. Nếu không có chỉ số như vậy, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [-10,-5,0,3,7]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Với mảng đã cho, <code>arr[0] = -10, arr[1] = -5, arr[2] = 0, arr[3] = 3</code>, nên kết quả là 3.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [0,2,5,8,17]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> <code>arr[0] = 0</code>, nên kết quả là 0.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [-10,-5,3,4,7,9]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có <code>i</code> nào thỏa mãn <code>arr[i] == i</code>, nên kết quả là -1.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt; 10<sup>4</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= arr[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Lời giải <code>O(n)</code> khá đơn giản. Ta có thể làm tốt hơn không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt tuyến tính sẽ tìm được $arr[i]=i$ nhỏ nhất; câu hỏi mở rộng yêu cầu độ phức tạp $O(\log n)$. Mảng tăng nghiêm ngặt nên $arr[i]-i$ không giảm, và vị trí đầu tiên có giá trị không âm là ứng viên cần tìm.
>
> Nếu $arr[mid]\ge mid$, đáp án không nằm bên phải; nếu không, đáp án nằm sau $mid$.
>
> Sau khi tìm kiếm, ta kiểm tra đẳng thức tại biên trái; nếu không thỏa, trả về $-1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def fixedPoint(self, arr: List[int]) -> int:
        left, right = 0, len(arr) - 1
        while left < right:
            mid = (left + right) >> 1
            if arr[mid] >= mid:
                right = mid
            else:
                left = mid + 1
        return left if arr[left] == left else -1
```

#### Java

```java
class Solution {
    public int fixedPoint(int[] arr) {
        int left = 0, right = arr.length - 1;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (arr[mid] >= mid) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return arr[left] == left ? left : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int fixedPoint(vector<int>& arr) {
        int left = 0, right = arr.size() - 1;
        while (left < right) {
            int mid = left + right >> 1;
            if (arr[mid] >= mid) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return arr[left] == left ? left : -1;
    }
};
```

#### Go

```go
func fixedPoint(arr []int) int {
	left, right := 0, len(arr)-1
	for left < right {
		mid := (left + right) >> 1
		if arr[mid] >= mid {
			right = mid
		} else {
			left = mid + 1
		}
	}
	if arr[left] == left {
		return left
	}
	return -1
}
```

#### TypeScript

```ts
function fixedPoint(arr: number[]): number {
    let left = 0;
    let right = arr.length - 1;
    while (left < right) {
        const mid = (left + right) >> 1;
        if (arr[mid] >= mid) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return arr[left] === left ? left : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
